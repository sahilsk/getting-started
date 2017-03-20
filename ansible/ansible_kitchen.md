Getting Start with Ansible
------


# Pre-requisite

Install ruby on (mac)

    brew update
    brew install rbenv

    export PATH="$HOME/.rbenv/bin:$PATH"
    eval "$(rbenv init -)"

    rbenv install 2.3.1
    rbenv global 2.3.1
    rbenv rehash
    gem install bundler
    
### Install ansible

    pip install ansible

### Install test kitchen

    gem install test-kitchen

### Install kitchen-xxx

kitchen-ansible  
https://github.com/neillturner/kitchen-ansible

    gem install kitchen-ansible
  
kitchen-vagrant

    gem install kitchen-vagrant
  
Verifier

    gem install kitchen-verifier-serverspec
  
    
## Setting up workspace

### Directory

In the Ansible repository specify:

- spec files with the roles.
- spec_helper in the spec folder (with code as below). 



```
+-- roles
¦   +-- common
¦   ¦   +-- spec
¦   ¦   ¦   +-- common_spec.rb
¦   ¦   +-- tasks
¦   ¦   ¦   +-- main.yml
¦   +-- nginx
¦       +-- handlers
¦       ¦   +-- main.yml
¦       +-- spec
¦       ¦   +-- nginx_spec.rb
¦       +-- tasks
¦       ¦   +-- main.yml
¦       +-- templates
¦       ¦   +-- nginx.repo
¦       +-- vars
¦           +-- main.yml
+-- spec
    +-- spec_helper.rb
    +-- my_private_key.pem
```


```
`-- test
    `-- integration
        |-- #{SUITE 1}
        |   `-- #{TEST FRAMEWORK}
        `-- #{SUITE 2}
            `-- #{TEST FRAMEWORK}
        ...

```

#### spec_helper.rb
``` rb
require 'rubygems'
require 'bundler/setup'

require 'serverspec'
require 'pathname'
require 'net/ssh'

RSpec.configure do |config|
  set :host,  ENV['TARGET_HOST']
  # ssh options at http://net-ssh.github.io/ssh/v1/chapter-2.html
  # ssh via password
  set :ssh_options, :user => ENV['LOGIN_USER'], :paranoid => false, :verbose => :error, :password => ENV['LOGIN_PASSWORD'] if ENV['LOGIN_PASSWORD']
  # ssh via ssh key
  set :ssh_options, :user => ENV['LOGIN_USER'], :paranoid => false, :verbose => :error, :host_key => 'ssh-rsa', :keys => [ ENV['SSH_KEY'] ] if ENV['SSH_KEY']
  set :backend, :ssh
  set :request_pty, true
end
```

  
Create .kitchen.yml
  
``` yml
---
driver:
  name: vagrant

provisioner:
  name: ansible_playbook
  roles_path: roles
  hosts: myservers
  require_ansible_repo: true
  ansible_verbose: true
  ansible_version: latest

platforms:
  - name: ubuntu_trusty_64
    driver_config:
      box: "ubuntu/trusty64"
      network:
      - ['private_network', {ip: '192.168.33.11'}]

verifier:
  name: serverspec

suites:
  - name: package
    provisioner:
      name: ansible_playbook
      playbook: test/integration/package.yml
      hosts: myservers
    verifier:
      patterns:
        - roles/common/spec/common_spec.yml
      bundler_path: '/usr/local/bin'
      rspec_path: '/usr/local/bin'
```


## Run it

    kitchen converge
    kitchen verify
    kitchen destroy
    
or

    kitchen test

## Using Roles from Ansible Galaxy

Use librarian-ansible by creating an Ansiblefile in the top level of the repository 
and kitchen-ansible will automatically call librarian-ansible during convergence.

For a description of setting up an Ansiblefile 
see [here](https://werner-dijkerman.nl/2015/08/15/using-librarian-ansible-to-install-ansible-roles-from-gitlab/).

-------

 References
------

- https://github.com/neillturner/kitchen-ansible
