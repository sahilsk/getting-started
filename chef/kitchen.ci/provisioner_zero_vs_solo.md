## For chef-solo

- uses `solo_rb` key for specifying enviornment variable file
- doesn't support chef `search` dsl

```
driver:
  name: vagrant

provisioner:
  name: chef_solo
  environments_path: test/environments
  solo_rb:
    environment: kitchen
 
 ```


## For chef-zero:

- supports chef `search` for searching attributes and data bags. Very useful to write chef-11 and chef-12 compatible opsworks cookbook
- uses `client_rb` key for specifying enviornment variable file

```
driver:
  name: vagrant
 
provisioner:
  name: chef_zero
  environments_path: test/environments
  client_rb:
    environment: kitchen
```
