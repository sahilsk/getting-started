Using the SDK for Ruby: Downloading Files from Amazon S3
---------


#### Testing by kitchen
- Create an environment file named test.json with the following default_attributes section, replacing the access_key and secret_key values with the corresponding keys for your IAM user. Save the file to the cookbook's environments folder.

```
{
  "default_attributes" : {
    "cookbooks_101" : {
      "access_key": "AKIAIOSFODNN7EXAMPLE",
      "secret_key" : "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
    }
  },
  "chef_type" : "environment",
  "json_class" : "Chef::Environment"
}
```


``` yml
#.kitchen.yml
--- 
driver:
  name: vagrant

provisioner:
  name: chef_solo
  environments_path: ./environments

platforms:
  - name: ubuntu-12.04

suites:
  - name: s3bucket
    provisioner:
      solo_rb:
        environment: test
    run_list:
      - recipe[s3bucket::default]
    attributes:
```

``` rb

gem_package "aws-sdk" do
  action :install
end

ruby_block "download-object" do
  block do
    require 'aws-sdk'

    s3 = AWS::S3.new(
          :access_key_id => "#{node['cookbooks_101']['access_key']}",
          :secret_access_key => "#{node['cookbooks_101']['secret_key']}")

    myfile = s3.buckets['cookbook_bucket'].objects['myfile.txt']
    Dir.chdir("/tmp")
    File.open("myfile.txt", "w") do |f|
      f.write(myfile.read)
      f.close
    end
  end
  action :run
end

```

More [here](http://docs.aws.amazon.com/opsworks/latest/userguide/cookbooks-101-opsworks-s3.html)


#### ON opswork stack

- AWS OpsWorks Stacks has already installed the SDK for Ruby, the chef_gem resource has been deleted.
- Do  not pass any credentials to AWS::S3.new. Credentials are automatically assigned to the application based on the instance's role.


``` rb
Chef::Log.info("******Downloading a file from Amazon S3.******")

ruby_block "download-object" do
  block do
    require 'aws-sdk'

    s3 = AWS::S3.new

    myfile = s3.buckets['cookbook_bucket'].objects['myfile.txt']
    Dir.chdir("/tmp")
    File.open("myfile.txt", "w") do |f|
      f.syswrite(myfile.read)
      f.close
    end
  end
  action :run
end

```
