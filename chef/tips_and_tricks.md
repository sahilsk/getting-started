Content
-----

- Stop/abort chef run or throw error
- Object to json
- Create and ADD Custom Box


### Stop/abort chef run or throw error

  raise if node[:platform_family] != 'debian
or
  return if node[:platform_family] != 'debian
 

### Object to json conversion
  
  a = { rabbitmq: 'localhost', port: 5671 }
  a = JSON.pretty_generate a
  
#### ADd or create custom vm box

https://github.com/test-kitchen/test-kitchen/issues/315
