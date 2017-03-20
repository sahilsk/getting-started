Disrupting changes in chef12
-------

- stack settings are available as Chef data bags and are accessed only through Chef search
- you can't use `node` object for searching stack attributes eg. `node['opsworks][stack][name]` will result in error.

## This will not work
```
Chef::Log.info ("********** The app's short name is '#{node['opsworks']['applications'].first['slug_name']}' **********")
Chef::Log.info("********** The app's URL is '#{node['deploy']['simplephpapp']['scm']['repository']}' **********")
```


## This will work instead

```
app = search("aws_opsworks_app").first

Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
```

To migrate your recipe code that accesses stack settings from Chef 11.10 and earlier versions for Linux to Chef 12 Linux, you must revise your code to:

- Access Chef data bags instead of Chef attributes.
- Use Chef search instead of the Chef node object.
- Use AWS OpsWorks Stacks data bag names such as `aws_opsworks_app`, instead of using AWS OpsWorks Stacks attribute names such as `opsworks` and `deploy`.


Source: [http://docs.aws.amazon.com/opsworks/latest/userguide/attributes-to-data-bags.html](http://docs.aws.amazon.com/opsworks/latest/userguide/attributes-to-data-bags.html)
