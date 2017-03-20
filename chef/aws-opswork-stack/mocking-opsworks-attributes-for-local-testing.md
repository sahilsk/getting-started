Mocking opswork attributes
----
**To add stack configuration and deployment attributes to the Test Kitchen environment**

1. Create an environment file named `test.json` with the following contents and save it to the cookbook's `environments` folder.
    
```
    {
      "default_attributes": {
        "opsworks" : {
          "stack" : {
            "name" : "MyStack",
            "id" : "42dfd151-6766-4f1c-9940-ba79e5220b58"
          }
        }
      },
      "chef_type" : "environment",
      "json_class" : "Chef::Environment"
    }
```

The environment file has the following elements:

    * `default_attributes` – The default attributes in JSON format.

These attributes are added to the node object with the `default` attribute type, which is the type used by all of the stack configuration and deployment JSON attributes. This example uses the edited version of the stack configuration and deployment JSON shown earlier.

    * `chef_type` – Set this element to `environment`.

    * `json_class` – Set this element to `Chef::Environment`.

2. Edit `.kitchen.yml` to define the Test Kitchen environment, as follows.

```
        ---
    driver:
      name: vagrant
    
    provisioner:
      name: chef_solo
      environments_path: ./environments
    
    platforms:
      - name: ubuntu-12.04
    
    suites:
      - name: printjson 
        provisioner:
          solo_rb:
            environment: test
        run_list:
          - recipe[printjson::default]
        attributes:
```

You define the environment by adding the following elements to the default `.kitchen.yml` created by `kitchen init`.

**provisioner**
: 

Add the following elements.

    * `name` – Set this element to `chef_solo`.

To replicate the AWS OpsWorks Stacks environment more closely, you could use [Chef client local mode][9] instead of Chef solo. Local mode is a Chef client option that uses a lightweight version of Chef server (Chef Zero) that runs locally on the instance instead of a remote server. It enables your recipes to use Chef server features such as search or data bags without connecting to a remote server.

    * `environments_path` – The cookbook subdirectory that contains the environment file, `./environments` for this example.

**suites:provisioner**
: 

Add a `solo_rb` element with an `environment` element set to the environment file's name, minus the .json extension. This example sets `environment` to `test`.

3. Create a recipe file named `default.rb` with the following content and save it to the cookbook's `recipes` directory.
   
  ```
   log "Stack name: #{node['opsworks']['stack']['name']}"
   log "Stack id: #{node['opsworks']['stack']['id']}"
   ```

This recipe simply logs the two stack configuration and deployment values that you added to the environment. Although the recipe is running locally in Virtual Box, you reference those attributes using the same node syntax that you would if the recipe were running on an AWS OpsWorks Stacks instance.

4. Run `kitchen converge`. You should see something like the following log output.

```
        ...
    Converging 2 resources       
    Recipe: printjson::default       
      * log[Stack name: MyStack] action write[2014-07-01T23:14:09+00:00] INFO: Processing log[Stack name: MyStack] action write (printjson::default line 1)       
    [2014-07-01T23:14:09+00:00] **INFO: Stack name: MyStack**       
                 
      * log[Stack id: 42dfd151-6766-4f1c-9940-ba79e5220b58] action write[2014-07-01T23:14:09+00:00] INFO: Processing log[Stack id: 42dfd151-6766-4f1c-9940-ba79e5220b58] action write (printjson::default line 2)       
    [2014-07-01T23:14:09+00:00] **INFO: Stack id: 42dfd151-6766-4f1c-9940-ba79e5220b58**       
```
