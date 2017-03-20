Worksapce setup
------


### Create a user and organization on chef-server

Run the following command to create an administrator:

    $ chef-server-ctl user-create USER_NAME FIRST_NAME LAST_NAME EMAIL 'PASSWORD' --filename FILE_NAME
    
An RSA private key is generated automatically. This is the user’s private key and should be saved to a safe location. The --filename option will save the RSA private key to the specified absolute path.

For example:

    $ chef-server-ctl user-create stevedanno Steve Danno steved@chef.io 'abc123' --filename /path/to/stevedanno.pem
    
Run the following command to create an organization:

    $ chef-server-ctl org-create short_name 'full_organization_name' --association_user user_name --filename ORGANIZATION-validator.pem

The name must begin with a lower-case letter or digit, may only contain lower-case letters, digits, hyphens, and underscores, and must be between 1 and 255 characters. For example: 4thcoffee.

The full name must begin with a non-white space character and must be between 1 and 1023 characters. For example: 'Fourth Coffee, Inc.'.

  The `--association_user` option will associate the user_name with the admins security group on the Chef server.

An RSA private key is generated automatically. This is the chef-validator key and should be saved to a safe location. The --filename option will save the RSA private key to the specified absolute path.

For example:

    $ chef-server-ctl org-create 4thcoffee 'Fourth Coffee, Inc.' --association_user stevedanno --filename /path/to/4thcoffee-validator.pem


## Generate a `chef-repo` directory:

Go to your local laptop and create a workspace.

    $ chef generate repo REPO_NAME
    
REPO_NAME name can be "chef-repo"


Create additional directories using [knife configure](https://docs.chef.io/knife_configure.html)

    $ cd chef-repo
    $ knife configure 
    
From chef-server generate and download user and org private key into **.chef** directory. Yes same keys that you've generated above.

## Combine all the information to generate knife.rb file

### Validatorless Bootstrap
The `ORGANIZATION-validator.pem` is typically added to the .chef directory on the workstation. When a node is bootstrapped from that workstation, the ORGANIZATION-validator.pem is used to authenticate the newly-created node to the Chef server during the initial **chef-client** run. 
Starting with Chef client 12.1, it is possible to bootstrap a node using the **USER.pem** file instead of the **ORGANIZATION-validator.pem** file. This is known as a `“validatorless bootstrap”`.
More about them (here](https://docs.chef.io/knife_bootstrap.html)

###  knife.rb/knife.json file

```
# See https://docs.getchef.com/config_rb_knife.html for more information on knife configuration options

current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "sonu"
client_key               "#{current_dir}/sonu.pem"
chef_server_url          "https://ip-172-31-57-207.ec2.internal/organizations/sonuorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
  
 ```
  - **chef_server_url** : hostname of your chef-server
  - **node_name**: user that you've generated on chef-server
  - **client_key**: keys that you generated when you created a user on chef-server

## make sure certs are valid and same

### knife ssl fetch
 Run the knife ssl fetch to download the self-signed certificate from the Chef server to the /.chef/trusted_certs directory on a workstation.

### knife ssl check

    $ knife ssl check
    Connecting to host ip-172-31-57-207.ec2.internal:443
    Successfully verified certificates from `ip-172-31-57-207.ec2.internal'
    
 If it failed, 

#### Verify Checksums

The SSL certificate that is downloaded to the `/.chef/trusted_certs` directory should be verified to ensure that it is, in fact, the same certificate as the one located on the Chef server. 
This can be done by comparing the SHA-256 checksums.

View the checksum on the Chef server:

     $ ssh ubuntu@chef-server.example.com sudo sha256sum /var/opt/opscode/nginx/ca/chef-server.example.com.crt
The response is similar to:

     <ABC123checksum>  /var/opt/opscode/nginx/ca/chef-server.example.com.crt
View the checksum on the workstation:

     $ gsha256sum .chef/trusted_certs/chef-server.example.com.crt
The response is similar to:

     <ABC123checksum>  .chef/trusted_certs/chef-server.example.com.crt
     
Verify that the checksum values are identical.

## upload cookbook

    $ knife cookbook upload example

## Bootstrap a node

    $ knife bootstrap -i ~/KEYS/<your-aws-key>.pem -x ubuntu -r "recipe[example::default]" 54.174.28.115 --sudo -A -N testclient

## Test it

    $ knife node list
   
  
   
## errors n fixes

- https://docs.chef.io/errors.html
- https://getchef.zendesk.com/hc/en-us/articles/206692583-Change-Hostname-of-Chef-Server
