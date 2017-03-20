Run a script on a Test Kitchen VM before converge starts
-------------

In the kitchen-vagrant documentation there is mention of a vagrantfiles property. 
This allows you to merge custom vagrant configuration into the vagrantfile that test-kitchen generates behind the scenes.

Create a file called vagrant.rb and place it in the same directory as .kitchen.yml .

``` rb
Vagrant.configure(2) do |config|
  config.vm.provision "shell", inline: <<-SHELL
     sudo yum install -y epel-release
     sudo yum install -y ansible
  SHELL
end

```



.kitchen.yml

```
...
 
platforms:
  - name: centos-7.0
    driver:
      box: centos7-64
      provision: true
      vagrantfiles:
        - vagrant.rb
 
...
```

This functionality has a wide range of uses, which could include adding new NICS or disks independently of the base box.


- setting up resolvable hostname

    vagrant.rb: 
    ``` rb
    $script = <<SCRIPT
    echo I am provisioning...
    date > /etc/vagrant_provisioned_at

    echo "adding host information"
    sed -i '$ a 192.168.10.33 rmq1' /etc/hosts·
    sed -i '$ a 192.168.10.34 rmq2' /etc/hosts·

    SCRIPT

    Vagrant.configure("2") do |config|
          config.vm.provision "shell", inline: $script
    end
    ```
  
More about 
-------

- official doc: [https://github.com/test-kitchen/kitchen-vagrant#-vagrantfile_erb](https://github.com/test-kitchen/kitchen-vagrant#-vagrantfile_erb)
- template file : [https://github.com/test-kitchen/kitchen-vagrant/blob/master/templates/Vagrantfile.erb](https://github.com/test-kitchen/kitchen-vagrant/blob/master/templates/Vagrantfile.erb)
