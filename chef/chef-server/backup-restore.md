AWS pre-requisites
----

- Setup public hosted zone ( org.net ) ( or private hosted zone if you have vpn setup at office to reach instance by private ips)
- create new dns entry:  stage-chef-server.org.net => public ip of chef-server instance


Backup
----

``` bash
#!/bin/bash -e

SHELL=/bin/bash
HOME=/opt/opscode/bin
PATH=/usr/bin:/usr/sbin:/bin:/opt/opscode/embedded/bin:/opt/opscode/bin
MAILTO=sonu.meena@abc.com
#17 23 * * * root drbd-backups -g opscode -l drbd > /dev/null 2>&1

export THEDATE=`date '+%Y-%m-%d-%H-%M-%S'`
echo "Make a postgres export "
su -  opscode-pgsql -m -s /bin/bash -c "/opt/opscode/embedded/bin/pg_dumpall -c | gzip --fast > /tmp/postgresql-dump-$THEDATE.gz"

echo "Synchronize to make sure that all of the data is present on-disk"
sync
echo "Backup the /etc/opscode and /var/opt/opscode directories and include the postgres export file as root"
tar cvfzp var-opt-opscode-$THEDATE.tar.gz /etc/opscode /var/opt/opscode /tmp/postgresql-dump-$THEDATE.gz
```


Restore
------


- Download backup tars to target server

```
#/bin/bash -e

export THEDATE='2017-02-27-12-01-07'

echo "Transfer hostname"
hostname stage-chef-server.abc.net
hostnamectl set-hostname stage-chef-server.abc.net
echo '127.0.0.1  stage-chef-server.abc.net stage-chef-server' >> /etc/hosts

echo "download and install chef-server core"
wget https://packages.chef.io/files/stable/chef-server/12.13.0/ubuntu/16.04/chef-server-core_12.13.0-1_amd64.deb 
dpkg -i chef-server-core_12.13.0-1_amd64.deb
chef-server-ctl reconfigure
chef-server-ctl install opscode-manage
chef-server-ctl stop

tar xvfzp var-opt-opscode-$THEDATE.tar.gz --exclude='var/opt/opscode/drbd/data/postgresql_9.2' -C /
chef-server-ctl start postgresql
su – opscode-pgsql -p
gunzip -c postgresql-dump-$THEDATE.gz | /opt/opscode/embedded/bin/psql -U “opscode-pgsql” -d postgres
exit
chef-server-ctl reconfigure
chef-server-ctl start
chef-manage-ctl reconfigure

```


SSL certificate
---

Let the ssl termination be at elb level. So, configure ssl certificates at elb and make chef-server part of elb. 
If nginx level then copy them too.


``` bash
mkdir -p /etc/pki/tls/private
copy cer and key files to private directory
configure SSL certificate
cat >/etc/opscode/chef-server.rb <<EOL
nginx[‘ssl_certificate’] = “/etc/pki/tls/private/your-signed-certificate.cer”
nginx[‘ssl_certificate_key’] = “/etc/pki/tls/private/your-certificate-key.key”
nginx[‘ssl_ciphers’] = “HIGH:MEDIUM:!LOW:!kEDH:!aNULL:!ADH:!eNULL:!EXP:!SSLv2:!SEED:!CAMELLIA:!PSK”
nginx[‘ssl_protocols’] = “TLSv1 TLSv1.1 TLSv1.2”
EOL
chef-server-ctl start
```


change rabbitmq user passwords
-----

Update the following file `/opt/opscode/embedded/bin/rabbitmq-defaults`
replace ERL_DIR= with ERL_DIR=dirname $0/
   
    $ grep -A 1 -E ‘(user”: “chef”,|actions_user”: “actions”,|jobs_user”: “jobs”,)’ /etc/opscode/chef-server-running.json

run the following command for each user and password outputted from the above command

    $ /opt/opscode/embedded/bin/rabbitmqctl change_password <user> <password>

then

    $ chef-server-ctl reconfigure
    $ opscode-manage-ctl reconfigure


Resources
---

- https://docs.chef.io/server_backup_restore.html
- https://www.harrington.io/backup-and-restore-chef-server-12/
