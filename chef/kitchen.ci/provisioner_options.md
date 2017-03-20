
[Source](https://www.vagrantup.com/docs/provisioning/chef_solo.html "Permalink to ")

OPTIONS

This section lists the complete set of available options for the Chef Solo provisioner. More detailed examples of how to use the provisioner are available below this section.

- cookbooks_path (string or array) - A list of paths to where cookbooks are stored. By default this is "cookbooks", expecting a cookbooks folder relative to the Vagrantfile location.

- data_bags_path (string or array) - A path where data bags are stored. By default, no data bag path is set. Chef 12 or higher is required to use the array option. Chef 11 and lower only accept a string value.

- environments_path (string) - A path where environment definitions are located. By default, no environments folder is set.

- nodes_path (string or array) - A list of paths where node objects (in JSON format) are stored. By default, no nodes path is set.

- environment (string) - The environment you want the Chef run to be a part of. This requires Chef 11.6.0 or later, and that environments_path is set.

- recipe_url (string) - URL to an archive of cookbooks that Chef will download and use.

- roles_path (string or array) - A list of paths where roles are defined. By default this is empty. Multiple role directories are only supported by Chef 11.8.0 and later.

- synced_folder_type (string) - The type of synced folders to use when sharing the data required for the provisioner to work properly. By default this will use the default synced folder type. For example, you can set this to "nfs" to use NFS synced folders.


Common to all
--

[Source](https://www.vagrantup.com/docs/provisioning/chef_common.html)
