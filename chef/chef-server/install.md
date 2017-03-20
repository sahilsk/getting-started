Download the package from https://downloads.chef.io/chef-server/.

Upload the package to the machine that will run the Chef server, and then record its location on the file system. The rest of these steps assume this location is in the /tmp directory.

As a root user, install the Chef server package on the server, using the name of the package provided by Chef. For Red Hat and CentOS 6:

    $ dpkg -i /tmp/chef-server-core-<version>.deb
    
After a few minutes, the Chef server will be installed.

Run the following to start all of the services:

    $ chef-server-ctl reconfigure
    
Because the Chef server is composed of many different services that work together to create a functioning system, this step may take a few minutes to complete.

Run the following command to create an administrator:

    $ chef-server-ctl user-create USER_NAME FIRST_NAME LAST_NAME EMAIL 'PASSWORD' --filename FILE_NAME
    
eg

    $ chef-server-ctl user-create sonu sonu kumar sonukr666@gmail.com 'sonu123' --filename sonu_private_chef_server

Run the following command to create an organization:

    $ chef-server-ctl org-create short_name 'full_organization_name' --association_user user_name --filename ORGANIZATION-validator.pem
    
eg

    $ chef-server-ctl org-create sonuorg 'Sonu Organization, Inc.' --association_user sonu --filename sonuorg-validator.pem

Reconfigure the Chef server and the Chef management console (standalone and frontend group members

    $ sudo chef-server-ctl reconfigure
    $ sudo chef-manage-ctl reconfigure
