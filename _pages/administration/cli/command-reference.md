---
UID: 56df3dee1750a
post_title: Command Reference
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: true
hide_from_navigation: false
hide_from_related: false
---
<h1>dcos config</h1>

<p>This command manages your Mesosphere DCOS configuration file.</p>

<p>A default configuration file named <code>dcos.toml</code> is created during initial DCOS CLI setup and is located in <code>~/.dcos/</code> on Unix, Linux, and OS X systems or at <code>C:Users&lt;your username&gt;.dcos</code> on most Windows systems.</p>

<p><code>dcos config append &lt;param-name&gt;&lt;value&gt;</code></p>

<p>Add an additional value to the end of an existing list. Items earlier in the list take precedence over items that follow. For example, you can append a local repository to the <code>packages.sources</code> list so that it is searched after the Mesosphere repository package.</p>

<p><code>dcos config --help</code>, <code>-h</code></p>

<p>Show a description of all command options and positional arguments for the command.</p>

<p><code>dcos config --info</code></p>

<p>Show a description of command.</p>

<p><code>dcos config prepend &lt;param-name&gt; &lt;value&gt;</code></p>

<p>Add an additional value to the beginning of an existing list. Items earlier in the list take precedence over items that follow. For example, you can prepend a local repository to the <code>packages.sources</code> list so that it is searched before the Mesosphere repository package.</p>

<p><code>dcos config set &lt;param-name&gt; &lt;value&gt;</code></p>

<p>Add a parameter to the DCOS configuration file.</p>

<p><code>dcos config show [&lt;param-name&gt;]</code></p>

<p>Display the DCOS configuration file contents.</p>

<p><code>dcos config unset [--index=&lt;index&gt;] &lt;param-name&gt;</code></p>

<p>Remove an entry from an existing configuration parameter list, where  is the numerical position in the list that you want to remove.</p>

<p><code>dcos config validate</code></p>

<p>Validate changes made to the DCOS configuration file. It is recommended that you run this option after making configuration changes.</p>

<h1>dcos marathon</h1>

<p>You can control the native DCOS Marathon instance with this command. You can quickly deploy custom applications to DCOS with the <code>dcos marathon</code> command.</p>

<p>This command works with the same set of JSON application definitions that are used by the <a href="https://mesosphere.github.io/marathon/docs/rest-api.html#post-/v2/apps" target="_blank">Marathon REST API</a>.</p>

<p><code>dcos marathon --config-schema</code></p>

<p>Show the configuration schema information.</p>

<p><code>dcos marathon --help</code>, <code>-h</code></p>

<p>Show a description of all command options and positional arguments for the command.</p>

<p><code>dcos marathon --info</code></p>

<p>Show a description of command.</p>

<p><code>dcos marathon app add [&lt;app-resource&gt;]</code></p>

<p>Add an application, with an optional application resource specified. For more information about application resources, see <a href="https://mesosphere.github.io/marathon/docs/rest-api.html#post-/v2/apps" target="_blank">Marathon REST API</a>.</p>

<p><code>dcos marathon app list</code></p>

<p>List the installed applications.</p>

<p><code>dcos marathon app remove [--force] &lt;app-id&gt;</code></p>

<p>Remove an application, with an optional <code>--force</code> flag and required application ID specified.</p>

<p><code>dcos marathon app restart [--force] &lt;app-id&gt;</code></p>

<p>Restart an application with an optional <code>--force</code> flag and required application ID specified.</p>

<p><code>dcos marathon app show [--app-version=&lt;app-version&gt;] &lt;app-id&gt;</code></p>

<p>Shows the full <code>marathon.json</code> definition of an application with an optional application version and a required application ID specified.</p>

<p><code>dcos marathon app start [--force] &lt;app-id&gt; [&lt;instances&gt;]</code></p>

<p>Start an application with an optional <code>--force</code> flag, required application ID, and optional number of instances specified.</p>

<p><code>dcos marathon app stop [--force] &lt;app-id&gt;</code></p>

<p>Stop an application with an optional <code>--force</code> flag and required application ID specified.</p>

<p><code>dcos marathon app update [--force] &lt;app-id&gt; [&lt;properties&gt;...]</code></p>

<p>Update an application with an optional <code>--force</code> flag, required application ID, and optional key-value pairs specified in <code>&lt;properties&gt;</code>. Use the equal (=) character as a separator between the key and value. You can also pass JSON or a file via stdin.</p>

<p><code>dcos marathon app version list [--max-count=&lt;max-count&gt;] &lt;app-id&gt;</code></p>

<p>List the version history of an application with and optional maximum number of entries to fetch and required application ID specified.</p>

<p><code>dcos marathon deployment list [&lt;app-id&gt;]</code></p>

<p>Show a current list of deployed applications with an optional application ID specified.</p>

<p><code>dcos marathon deployment rollback &lt;deployment-id&gt;</code></p>

<p>Remove a deployed application with a required application ID specified.</p>

<p><code>dcos marathon deployment stop &lt;deployment-id&gt;</code></p>

<p>Cancel the in-progress deployment of an application with a required application ID specified.</p>

<p><code>dcos marathon deployment watch [--max-count=&lt;max-count&gt;][--interval=&lt;interval&gt;] &lt;deployment-id&gt;</code></p>

<p>Monitor deployments with an optional maximum number of entries to fetch, optional number of seconds to wait between monitor calls, and required application ID specified.</p>

<p><code>dcos marathon group add [&lt;group-resource&gt;]</code></p>

<p>Create a new group as specified by <code>&lt;group-resource&gt;</code>. For more information, see the <a href="https://mesosphere.github.io/marathon/docs/rest-api.html#post-/v2/groups" target="_blank">Marathon documentation</a>.</p>

<p><code>dcos marathon group list [--json]</code></p>

<p>Show the list of groups in optional JSON format.</p>

<p><code>dcos marathon group show [--group-version=&lt;group-version&gt;] &lt;group-id&gt;</code></p>

<p>Show a detailed view of a group with optional <code>&lt;group-version&gt;</code>. You can specify the <code>&lt;group-version&gt;</code> as an absolute value or as relative value. The absolute version values must be in ISO8601 date format. Relative values must be specified as a negative integer and they represent the version from the currently deployed group definition.</p>

<p><code>dcos marathon group remove [--force] &lt;group-id&gt;</code></p>

<p>Remove a group with optional <code>--force</code> flag.</p>

<p><code>dcos marathon task list [&lt;app-id&gt;]</code></p>

<p>List the tasks with an optional application ID specified.</p>

<p><code>dcos marathon task show &lt;task-id&gt;</code></p>

<p>Show the tasks with a required task ID specified.</p>

<h1>dcos node</h1>

<p>This command manages your DCOS nodes.</p>

<p><code>dcos node --help</code>, <code>-h</code></p>

<p>Show a description of all command options and positional arguments for the command.</p>

<p><code>dcos node --info</code></p>

<p>Show a description of command.</p>

<p><code>dcos node --json</code></p>

<p>Print a list of JSON-formatted nodes.</p>

<p><code>dcos node</code></p>

<p>List all Mesos agent nodes running on DCOS.</p>

<p><code>dcos node log [--follow --lines=&lt;N&gt;] --master --slave=&lt;slave-id&gt;</code></p>

<p>Prints the Mesos logs for the leading master node, agent nodes, or both. Optionally, you can specify:</p>

<ul>
<li>Dynamically update the log (<code>--follow</code>)</li>
<li>Show the last <em>N</em> lines, where 10 is the default (<code>--lines=&lt;N&gt;</code>)</li>
</ul>

<p><code>dcos node ssh [--option SSHOPT=VAL ...] [--config-file=&lt;path&gt;] [--user=&lt;user&gt;] (--master | --slave=&lt;slave-id&gt;)</code></p>

<p>Establish an SSH connection to the master or agent nodes of your DCOS cluster. Optionally, you can specify:</p>

<ul>
<li>The SSH options (<code>--option SSHOPT=VAL ...</code>). For more information, type <code>man ssh_config</code> in your terminal. </li>
<li>The path to the SSH configuration file (<code>--config-file=&lt;path&gt;</code>) </li>
<li>The SSH user, where the default user is "core" (<code>--user=&lt;user&gt;</code>)</li>
<li>Proxy the SSH connection through a master node (<code>--master-proxy</code>). By default, <code>dcos node ssh</code> connects to the private IP of the node, which is only accessible from hosts within the same network, so you must use the <code>--master-proxy</code> option to access your cluster from an outside network. For example, in the default AWS configuration, the private slaves are unreachable from the public Internet, but you can SSH to them using this option, which will proxy the SSH connection through the publicly reachable master.</li>
</ul>

<h1>dcos package</h1>

<p>You can use the <code>dcos package</code> command to install and manage software packages from the public <a href="https://github.com/mesosphere/universe">Mesosphere repository</a>. The Mesosphere repository stores certified versions of datacenter services.</p>

<p><code>dcos package --config-schema</code></p>

<p>Show the configuration schema information for a package. The configuration schema is a job representation of data that can be stored locally. If your DCOS service has subcommands and stores data in the CLI configuration file, you can define a configuration schema that defines the data type, defaults, and a potential range of values. If your DCOS service doesn't have subcommands, you cannot define a configuration schema.</p>

<p><code>dcos package --help</code>, <code>-h</code></p>

<p>Show a description of all command options and positional arguments for the command.</p>

<p><code>dcos package --info</code></p>

<p>Show a description of command.</p>

<p><code>dcos package describe &lt;package_name&gt; [--app --cli --config --render --package-versions --options=&lt;file&gt;] --package-version=&lt;package_version&gt;]</code></p>

<p>Get specific details for packages, where <code>&lt;package_name&gt;</code> is the is value for “name” in the Mesosphere repository.</p>

<ul>
<li>Show details for the app only (<code>--app</code>) </li>
<li>Show details for the DCOS CLI only (<code>--cli</code>) </li>
<li>Show all possible key-values for building a custom <code>option.json</code> file. This file can be used when installing the scheduler. (<code>--config</code>) </li>
<li>Render the package's <code>marathon.json</code> or <code>command.json</code> template with the values from <code>config.json</code> and <code>--options</code>. If not provided, print the raw templates. </li>
<li>Show all versions for the package (<code>package-versions</code>) </li>
<li>Show the path to a JSON file that contains the package installation options (<code>--options=&lt;file&gt;</code>)</li>
</ul>

<p><code>dcos package install [--options=&lt;file&gt; --app-id=&lt;app_id&gt; --cli --app] &lt;package_name&gt;</code></p>

<p>Install a package, where <code>&lt;package_name&gt;</code> is the is value for “name” in the Mesosphere repository. Optionally, you can specify:</p>

<ul>
<li>A custom JSON file (<code>--options=&lt;file&gt;</code>) </li>
<li>A specific application ID (<code>--app-id=&lt;app_id&gt;</code>) </li>
<li>Install the DCOS CLI only (<code>--cli</code>) </li>
<li>Install the app only (<code>--app</code>)</li>
</ul>

<p><code>dcos package list [--endpoints --app-id=&lt;app-id&gt; &lt;package_name&gt;]</code></p>

<p>List the installed packages. Optionally, you can:</p>

<ul>
<li>Specify a specific application ID (<code>--app-id=&lt;app_id&gt;</code>) </li>
<li>Specify the package name (<code>&lt;package_name&gt;</code>) </li>
<li>Return the hosts and ports of all the Mesos tasks associated with package (<code>--endpoints</code>)</li>
</ul>

<p><code>dcos package search &lt;query&gt;</code></p>

<p>Search the Mesosphere repository for packages with the optional <code>&lt;query&gt;</code> value specified. You can use complete or partial values for <code>&lt;query&gt;</code>.</p>

<p><code>dcos package sources</code></p>

<p>Show the current package repository source. Possible sources include a local file, HTTPS, and Git.</p>

<p><code>dcos package uninstall [--all | --app-id=&lt;app-id&gt;] &lt;package_name&gt;</code></p>

<p>Uninstall a package, where <code>&lt;package_name&gt;</code> is the is value for “name” in the Mesosphere repository. Optionally, you can specify:</p>

<ul>
<li>All services (<code>--all</code>) </li>
<li>A specific application ID (<code>--app-id=&lt;app_id&gt;</code>)</li>
</ul>

<p><code>dcos package update [--validate]</code></p>

<p>Update the local cache of the package repository and optionally validate the package content.</p>

<h1>dcos service</h1>

<p>You can use the <code>dcos service</code> command to get the status of DCOS services.</p>

<p><code>dcos service --help</code>, <code>-h</code></p>

<p>Show a description of all command options and positional arguments for the command.</p>

<p><code>dcos service --info</code></p>

<p>Show a description of command.</p>

<p><code>dcos service [--inactive --json]</code></p>

<p>List all of the active installed DCOS services. Optionally, you can specify:</p>

<ul>
<li>Show the inactive DCOS services (<code>--inactive</code>). Inactive services are those that have been disconnected from the master, but haven't yet reached their failover timeout. </li>
<li>Display in the JSON-format (<code>--json</code>). </li>
</ul>

<p><code>dcos service --version</code></p>

<p>Show the version of the DCOS CLI.</p>

<p><code>dcos service log [--follow --lines=&lt;N&gt; --ssh-config-file=&lt;path&gt;] &lt;service&gt; [&lt;file&gt;]</code></p>

<p>Print the service logs, where <code>&lt;service&gt;</code> is the DCOS service name. Optionally, you can specify:</p>

<ul>
<li>Print data as the log file grows (<code>--follow</code>)</li>
<li>Show the last <em>N</em> lines, where 10 is the default (<code>--lines=&lt;N&gt;</code>) </li>
<li>The path to the SSH config file. This is used to access the Marathon logs. (<code>--ssh-config-file=&lt;path&gt;</code>)</li>
<li>The service log filename from the Mesos sandbox (<code>&lt;file&gt;</code>). The default file is stdout.</li>
</ul>

<p><strong>Important:</strong> To view the native DCOS Marathon logs by using the <code>dcos service log marathon</code> command, you must be on the same network or connected by VPN to your cluster. For more information, see <a href="/logging/system-logs/">Accessing native DCOS Marathon logs</a>.</p>

<p><code>dcos service shutdown &lt;service-id&gt;</code></p>

<p>Shutdown a service as specified by <code>&lt;service-id&gt;</code>. The <code>&lt;service-id&gt;</code> is listed in the ID column of <code>dcos service</code> output.</p>

<h1>dcos task</h1>

<p>This command shows the status of your DCOS tasks.</p>

<p><code>dcos task --help</code>, <code>-h</code></p>

<p>Show a description of all command options and positional arguments for the command.</p>

<p><code>dcos task --info</code></p>

<p>Show a description of command.</p>

<p><code>dcos task</code></p>

<p>List all DCOS tasks that are running.</p>

<p><code>dcos task [--completed --json &lt;task&gt;]</code></p>

<p>Show the DCOS tasks that are running. Optionally, you can specify:</p>

<ul>
<li>Show the completed DCOS tasks (<code>--completed</code>)</li>
<li>Print task data in JSON format (<code>--json &lt;task&gt;</code>)</li>
</ul>

<p><code>dcos task log [--completed --follow --lines=&lt;N&gt;] &lt;task&gt; [&lt;file&gt;]</code></p>

<p>Show the task logs.</p>

<p><code>&lt;task&gt;</code> can be the full task ID, a partial task ID, or a regular expression.</p>

<p>If you specify a partial task ID, logs for all task names that contain the partial task ID are displayed. For example, if you have tasks named <code>spark1</code>, <code>spark2</code>, and <code>spark3</code>, then the command <code>dcos task log spark</code> will display task logs for all three of these tasks; you do not need to use wildcards.</p>

<p>If you use a regular expression, you must enclose the task ID in double quotation marks and include an asterisk at the end of the task ID. For example, <code>dcos task log "spark[13]*"</code> will display information for tasks <code>spark1</code> and <code>spark3</code>, but not <code>spark2</code>.</p>

<p>Optionally, you can:</p>

<ul>
<li>Read logs from an existing file (<code>&lt;file&gt;</code>). <code>stdout</code> is the default.</li>
<li>Dynamically display new logging data as it is added (<code>--follow</code>)</li>
<li>Show the last <em>N</em> lines, where 10 is the default (<code>--lines=&lt;N&gt;</code>)</li>
</ul>