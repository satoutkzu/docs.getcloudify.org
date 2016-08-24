---

title: Using Plugins in Your Application


weight: 50

---

### Using Plugins

Cloudify utilizes [Wagon](http://github.com/cloudify-cosmo/wagon) to [create](/plugins/creating-your-own-plugin) and install plugins.

#### Installing plugins in Cloudify CLI

To use plugins in Cloudify CLI, you can install them via Wagon's command-line interface itself (which is installed alongside Cloudify CLI).

To install a plugin, run:

```bash
wagon install -s /path/to/wagon/archive.wgn
...

INFO - Installing cloudify_aws_plugin-1.4.1.dev0-py27-none-linux_x86_64-none-none.wgn
...
```

{% call c.note("Note") %}
sudo privileges might be required if you use one of our CLI packages.
{% endcall %}

#### Uploading plugins to Cloudify Manager

Cloudify allows users to upload and download plugins to and from the manager, and also to delete and list plugins already on the manager. These abilities are exposed by the rest client via the REST API as well as via the CLI. 

For a list of plugin packages you can download, see our [downloads page](http://getcloudify.org/downloads/plugin-packages.html).

To upload a plugin to the manager:

```bash
$ cfy plugins upload -p /path/to/wagon/archive.wgn
...

Validating /path/to/wagon/archive.wgn
Plugin validated successfully
Uploading plugin '/path/to/wagon/archive.wgn' to management server x.x.x.215
Uploaded plugin successfully, plugin's id is: f82610f0-42d6-4ce4-9efa-9ad21e4fd557
...
```

The `cfy plugins` command exposes additional commands like downloading and listing plugins found on the Manager.

{% call c.note("Note") %}
When a plugin is uploaded to the manager, if this plugin matches the manager architecture, it will be installed on it. This plugin
can then later be used globally by all deployments that require it as a `central_deployment_agent` plugin.
Conversly, when a plugin is deleted from the manager, it is also uninstalled (if it was installed in the first place), unless at least one
deployment is currently using this plugin, in which case, the delete request will fail.

`central_deployment_agent` plugins are installed using an internal workflow named `install_plugin`. If something goes wrong during plugin installation/uninstallation,
you can get the failed execution id by running `cfy list executions --system-workflows` and look for a failed `install_plugin`
or `uninstall_plugin` execution. Take the execution id and run `cfy events list -vvl -e {EXECUTION_ID}`.

{% endcall %}

##### Using the Web UI
Plugins management is done through the Plugins section in the Web UI.

#### Using plugins with in your blueprint

After having either installed the plugin in the CLI or uploaded the plugin to the Manager, blueprints can make use of it by having the plugin defined in the blueprint.

{% call c.note("Note") %}
Read more about how to define the plugin in the blueprint [here]({{ relRef("blueprints/spec-plugins.md") }}).
{% endcall %}

{% call c.tip("Uploading plugins during bootstrap") %}
Cloudify enables uploading plugins to the Manager during bootstrap. For more on that, please refer to [Plugin Resources]({{ relRef("blueprints/spec-upload-resources.md") }}).
{% endcall %}

# What's Next

Cloudify's Team provides a set of Official Plugins you can use. You can find further details about them here, under the `plugins` section.

You can also write your own plugin. To see how, read [this](/plugins/creating-your-own-plugin).