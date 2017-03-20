> Best way of using chef is to use it with kitchen.ci


Installation And setup
----

- install [chef-dk]( https://downloads.chef.io/chef-dk/)

Create cookbook/app

    chef generate cookbook lampstack
    
Install kitchen stuff

    kitchen init --driver=kitchen-vagrant
    

Writig Test
------


[Source](http://kitchen.ci/docs/getting-started/writing-test "Permalink to  Writing a Test - KitchenCI ")

#  Writing a Test - KitchenCI 


## Writing a Test

Being able to manually verify the Chef run is great but it would be even better if we had an executable test that would fail if our assumptions were ever proven to be false. Remember how we manually verified that Git was installed in the previous section? Seems like a pretty decent first test to me!

Perhaps Ironically Test Kitchen isn't very choosy when it comes to tests. If you don't have any that's cool; a successful Chef run already gives you a ton of useful information.

Now if you **are** interested in writing some tests, Test Kitchen has several options available to you. The component that helps facilitate testing on your instances is called **Busser**. Just like Test Kitchen it is a RubyGem library and it provides a plugin system so that you can wire in whatever testing framework your heart desires. A quick [search][8] on the RubyGems website returns several testing frameworks currently available to you.

To keep things simple we're going to use the `busser-bats` runner plugin which uses the [Bash Automated Testing System][9] also known as **bats**.

| ----- |
|  &nbsp; |  Pro-Tip |  
|  &nbsp; |  Testing using shell scripts is a great first start, but once you get to testing multiple platforms, you soon find that you're needing to invest some time in handling the differences between distributions and behaviours. That might indicate to you that you should consider a testing tool that handles that hard work for you. We'll see more about this shortly. | 

We need to put our test files in a specifc location, so let's create the directory:
    
    
    $ mkdir -p test/integration/default/bats
    

It looks long and dense, but each directory has some meaning to Test Kitchen and the Busser helper:

* `test/integration`: Test Kitchen will look for tests to run under this directory. It allows you to put unit or other tests in `test/unit`, `spec`, `acceptance`, or wherever without mixing them up. This is configurable, if desired.
* `default`: This corresponds exactly to the **Suite** name we set up in the `.kitchen.yml` file. If we had a suite called `"server-only"`, then you would put tests for the server only suite under `test/integration/server-only`.
* `bats`: This tells Test Kitchen (and Busser) which Busser runner plugin needs to be installed on the remote instance. In other words the `bats` directory name will cause Busser to install `busser-bats` from RubyGems.

Okay, long directories are long. Let's write a test. Create a new file called `test/integration/default/bats/git_installed.bats` with the following:
    
    
    #!/usr/bin/env bats
    
    @test "git binary is found in PATH" {
      run which git
      [ "$status" -eq 0 ]
    }
    

* * *
  


Dependency management
--

- [Berksfile](berkshelf.com)
- kitchen.yml reference: https://docs.chef.io/config_yml_kitchen.html
