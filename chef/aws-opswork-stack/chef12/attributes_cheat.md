## Get ip of instance

   server =  search("aws_opsworks_instance", "self:true").first
   server[:private_ip]

## Get layers of instance

    instance = search("aws_opsworks_instance", "self:true").first
    layerId = instance[:layer_ids].first
    layer = search("aws_opsworks_layer", "layer_id:#{layerId}").first
    log "::::::::::: Layer name: #{layer[:name]} "
    

## Get instances in a layer


    #layer_name = 'xyz'
    layer = search(:aws_opsworks_layer, "name:#{layer_name}").first
    layer_id = layer[:layer_id]
    instances = search("aws_opsworks_instance", "layer_ids:#{layer_id}")
    instance_ips = instances.map { |server|  server[:private_ip] }
    instance_hostnames = instances.map { |server|  server[:hostname] }

##  extras


    search(:aws_opsworks_app, "name:myapp")
    search(:aws_opsworks_app, ”deploy:true")
    search(:aws_opsworks_layer, "name:my_layer*")
    search(:aws_opsworks_rds_db_instance)
    search(:aws_opsworks_volume)
    search(:aws_opsworks_ecs_cluster)
    search(:aws_opsworks_elastic_load_balancer)
    search(:aws_opsworks_user)
