Data Bags
-------

 A data bag is a global variable that is stored as JSON data on an instance. A data bag can store global variables such as an app's source URL, the instance's hostname, and the associated stack's VPC identifier. 
AWS OpsWorks Stacks stores its data bags on each stack's instances


How-to-use data bags
------

To access a data bag through Chef **search**, use the **search** method, specifying the desired search index.
eg.
``` rb
search("aws_opsworks_app").each do |app|
  Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
  Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
end
```
AWS OpsWorks Stacks provides the following search indexes:

- aws_opsworks_app, which represents a set of deployed apps for a stack.
- aws_opsworks_command, which represents a set of commands that were run on a stack.
- aws_opsworks_ecs_cluster, which represents a set of Amazon EC2 Container Service (Amazon ECS) cluster instances for a stack.
- aws_opsworks_elastic_load_balancer, which represents a set of Elastic Load Balancing load balancers for a stack.
- aws_opsworks_instance, which represents a set of instances for a stack.
- aws_opsworks_layer, which represents a set of layers for a stack.
- aws_opsworks_rds_db_instance, which represents a set of Amazon Relational Database Service (Amazon RDS) instances for a stack.
- aws_opsworks_stack, which represents a stack.
- aws_opsworks_user, which represents a set of users for a stack.

Once you know the search index name, you can access the content of the data bag for that search index.
More info and examples [here](http://docs.aws.amazon.com/opsworks/latest/userguide/data-bags.html)


Using Data Bags
---------

> You cannot create a data bag by adding it to your cookbook repository. You must use custom JSON.

Chef Server allows you to pass user-defined data to your recipes by using data bags.

- You store the data with your cookbooks, and Chef installs it on each instance.
- You can use encrypted data bags for sensitive data such as passwords.

**AWS OpsWorks Stacks** supports data bags; recipes can retrieve the data using exactly the same code as with Chef Server. However, the support has the following limitations and differences:

- Data bags are supported only on Chef 11.10 Linux and later stacks. Windows stacks and Linux stacks running earlier versions of Chef do not support data bags.
- You do not store data bags in your cookbook repository. Instead, you use custom JSON to manage your data bag's data.
- AWS OpsWorks Stacks **does not** support encrypted data bags.

If you need to store sensitive data in encrypted form, such as passwords or certificates, we recommend storing it in a private S3 bucket. You can then create a custom recipe that uses the Amazon SDK for Ruby to retrieve the data. For an example, see Using the SDK for Ruby.



source: [Using Data Bags](http://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-chef11-10.html#workingcookbook-chef11-10-databag)
------


You create a data bag by using custom JSON to add one or more attributes to the [:opsworks][:data_bags] attribute.

 AWS OpsWorks Stacks obtains the data from the instance's stack configuration and deployment attributes. 
 However, AWS OpsWorks Stacks does not currently support Chef environments, so **node.chef_environment** always returns `_default`.
 
 Custom json example:
 
 ```
 {
  "opsworks": {
    "data_bags": {
      "bag_name1": {
        "item_name1: {
          "key1" : “value1”,
          "key2" : “value2”,
          ...
        }
      },
      "bag_name2": {
        "item_name1": {
          "key1" : “value1”,
          "key2" : “value2”,
          ...
        }
      },
      ...
    }
  }
}
```

You typically specify custom JSON for the stack, which installs the custom attributes on every instance for each subsequent lifecycle event. 
You can also specify custom JSON when you deploy an app, but those attributes are installed only for that deployment, and might be installed to only a selected set of instances. For more information, see [Deploying Apps](http://docs.aws.amazon.com/opsworks/latest/userguide/workingapps-deploying.html).


The following custom JSON example creates data bag named **myapp**. It has one item, **mysql**, with two key-value pairs.

``` json
{ "opsworks": {
    "data_bags": {
      "myapp": {
        "mysql": { 
          "username": "default-user",
          "password": "default-pass"
        }
      }
    }
  }
}
```

To use the data in your recipe, you can call **data_bag_item** and pass it the data bag and value names, as shown in the following excerpt.

``` rb
mything = data_bag_item("myapp", "mysql")
Chef::Log.info("The username is '#{mything['username']}' ")
```

To modify the data in the data bag, just modify the custom JSON, and it will be installed on the stack's instances for the next lifecycle event.


References
---

- [AWS OpsWorks Stacks Data Bag Reference](http://docs.aws.amazon.com/opsworks/latest/userguide/data-bags.html)
- [Custom Json](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-json.html)
