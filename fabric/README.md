Fabric
-----

## Installation

    pip install pycrypto paramiko fabric


## Basics

- **run**: run command on remote host. If no host is given it run locally
- **local**: run command on local workstation
- hosts list:
  - can be provided by enviornment:  export HOSTS=['web1', 'web2']
  - can be provide in the program itself. eg. env.hosts = ['host1', 'host2']
  - can be provided via roles. eg. env.roledefs['webservers'] = ['www1', 'www2', 'www3']
  - can be provided via commandline eg. fab -H host1,host2 mytask
  

-----

### Defining [hosts](http://docs.fabfile.org/en/1.12/usage/execution.html#hosts)

#### Via command line

``` python
from fabric.api import run

def mytask():
    run('ls /var/www')
```
``` shell
$ fab mytask:hosts="host1;host2"
```

Extends hosts

```python
from fabric.api import env, run
env.hosts.extend(['host3', 'host4'])

# $fab mytask:hosts="host1;host2"
def mytask():
    ## will run on host1,2,3,4
    run('ls /var/www')
```


#### Programmically


##### Define them via decorators

``` python
from fabric.api import hosts, run

@hosts('host1', 'host2')
def mytask():
    run('ls /var/www')

## OR

my_hosts = ('host1', 'host2')
@hosts(my_hosts)
def mytask():
    # ...
```

##### Set in a function

``` python
from fabric.api import env, run

def set_hosts():
    env.hosts = ['host1', 'host2']

def mytask():
    run('ls /var/www')
```

``` bash
$ fab set_hosts mytask
```

**set_hosts** : It doen'st have any host set. So, it'll run locally
**mytask**: it'll run on hosts previously set by **set_hosts** function


##### Define via : [Roles](http://docs.fabfile.org/en/1.12/usage/execution.html#roles)

``` python
from fabric.api import env, hosts, roles, run

env.roledefs['webservers'] = ['www1', 'www2', 'www3']
#or
env.roledefs = {
    'web': ['www1', 'www2', 'www3'],
    'dns': ['ns1', 'ns2']
}
#or
env.roledefs = {
    'web': {
        'hosts': ['www1', 'www2', 'www3'],
        'foo': 'bar'
    },
    'dns': {
        'hosts': ['ns1', 'ns2'],
        'foo': 'baz'
    }
}

@roles('db')
def migrate():
    # Database stuff here.
    pass

@roles('web')
def update():
    # Code updates here.
    pass

def deploy():
    execute(migrate)
    execute(update)
```
``` bash
$ fab migrate update
#or 
$ fab deploy
```


##### Combining

``` python
from fabric.api import env, hosts, roles, run

env.roledefs = {'role1': ['b', 'c']}

# union of hosts and roles: a,b,c
@hosts('a', 'b')
@roles('role1')
def mytask():
    run('ls /var/www')
```

---

## [@task](http://docs.fabfile.org/en/1.12/usage/execution.html#defining-tasks)

When this decorator is used, it signals to Fabric that only functions wrapped in the decorator are to be loaded up as valid tasks. 


``` python
from fabric.api import task, run

@task
def mytask():
    run("a command")
```


Put alias to methods

    @task(alias='dwm')
    

### More examples


``` python
#deploy.rb
from fabric.api import task

@task
def migrate():
    pass

@task
def push():
    pass

@task
def provision():
    pass

@task
def full_deploy():
    if not provisioned:
        provision()
    push()
    migrate()
```

``` python
#fabfile.py
import deploy
```


``` bash
$ fab --list
Available commands:

    deploy.full_deploy
    deploy.migrate
    deploy.provision
    deploy.push
```

Using the default kwarg to @task, we can tag e.g. full_deploy as the default task:

``` python
@task(default=True)
def full_deploy():
    pass
```
Now try fab list

``` bash
$ fab --list
Available commands:

    deploy
    deploy.full_deploy
    deploy.migrate
    deploy.provision
    deploy.push
```

### Namespaces

http://docs.fabfile.org/en/1.12/usage/tasks.html#namespaces


```
.
├── __init__.py
├── db
│   ├── __init__.py
│   └── migrations.py
└── lb.py
```
``` python
# fabfile.py
import lb
import db
```

``` bash
$ fab --list
  deploy
  compress
  lb.add_backend
  db.migrations.list
  db.migrations.run
```


###  Renaming namespaces


```python
import db as database
```

``` shell
$ fab --list
deploy
compress
lb.add_backend
database.migrations.list
database.migrations.run

```

### Nest list ouput

``` shell
$ fab --list-format=nested --list
Available commands (remember to call as module.[...].task):

    deploy
    compress
    lb:
        add_backend
    database:
        migrations:
            list
            run
```

use `execute` for dynamically added host execution
------------

``` python
@task
  def init(app):
      instances = get_elb_instances(app)
      canary_instance = None
      if len(instances) > 1:
          canary_instance = instances.pop(-1)
      env.roledefs = {
          'canary': [canary_instance.get_private_ip()],
          'half': ['ns1', 'ns2']
      }
      env.forward_agent = True
      env.gateway = '172.25.156.36'
      env.user = 'ubuntu'


  @task
  def canary_deploy(app):
      execute(init, app)
      execute(deploy_to_canary_instance, app)
      disconnect_all()


  @task
  @roles('canary')
  def deploy_to_canary_instance(app, version=None):
      srv = get_canary_instance(app)
```

How to use through bastion/jump/ssh tunnel?
------------------

Simply provide [env.gateway](http://docs.fabfile.org/en/1.12/usage/env.html#gateway)

Enables SSH-driven gatewaying through the indicated host. The value should be a normal Fabric host string as used in e.g. env.host_string. When this is set, newly created connections will be set to route their SSH traffic through the remote SSH daemon to the final destination.

``` python
  @task
  def init(app):
      instances = get_elb_instances(app)
      canary_instance = None
      if len(instances) > 1:
          canary_instance = instances.pop(-1)
      env.roledefs = {
          'canary': [canary_instance.get_private_ip()],
          'half': ['ns1', 'ns2']
      }
      env.forward_agent = True
      env.gateway = '<jump host ip>'
      env.user = '<jump host user>'
```


How to run
--

``` python
@task
def task(something=''):
  print "you said %s" % something

``` python
 fab task:'hello world'
 fab task:something='hello'
 fab task:foo=99,bar=True
 fab task:foo,bar
```



Userful methods
------------------


- [fabric.contrib.project.rsync_project(*args, **kwargs)](http://docs.fabfile.org/en/1.12/api/contrib/project.html?highlight=project#fabric.contrib.project.rsync_project)
    Synchronize a remote directory with the current project directory via rsync.

- [fabric.contrib.project.upload_project](http://docs.fabfile.org/en/1.12/api/contrib/project.html?highlight=project#fabric.contrib.project.upload_project)
   Upload the current project to a remote system via tar/gzip.

- [fabric.operations.put](http://docs.fabfile.org/en/1.12/api/core/operations.html?highlight=put#fabric.operations.put)
   Upload one or more files to a remote host.

- [fabric.operations.get](http://docs.fabfile.org/en/1.12/api/core/operations.html?highlight=put#fabric.operations.get)
   Download one or more files from a remote host.



