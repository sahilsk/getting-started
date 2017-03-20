Source: http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-json.html

You can declare custom JSON at the 

- stack levels: Common attributes to all instances of all layers
- layer: Attributes common to all instances of that layer only.
- deployment:  Attributes common to instances chosen for that application deployment only.


You may want to do this if you want some custom JSON to be visible only to an individual deployment or layer.
Or, for example, you may want to temporarily override custom JSON declared at the stack level with custom JSON declared at the layer level.
If you declare custom JSON at multiple levels, custom JSON declared at the deployment level overrides any custom JSON declared at both the layer and stack levels. 
Custom JSON declared at the layer level overrides any custom JSON declared only at the stack level.

Layer level
----

You can declare custom JSON at the deployment, layer, and stack levels. You may want to do this if you want some custom JSON to be visible across the stack or only to an individual deployment. Or, for example, you may want to temporarily override custom JSON declared at the layer level with custom JSON declared at the deployment level. If you declare custom JSON at multiple levels, custom JSON declared at the deployment level overrides any custom JSON declared at both the layer and stack levels. Custom JSON declared at the layer level overrides any custom JSON declared only at the stack level.




Deploy level
-----

If you specify custom JSON here, it is added to the stack configuration and deployment attributes for this deployment only. If you want to add custom JSON permanently, you must add it to the stack. Custom JSON is limited to 80 KB. If you need more capacity, we recommend storing some of the data on Amazon S3. Your custom recipes can then use the AWS CLI or [AWS SDK for Ruby](http://aws.amazon.com/documentation/sdk-for-ruby/) to download the data from the bucket to your instance.
