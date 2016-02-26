---
ID: 3712
post_title: Command Reference
post_date: 2016-02-26 12:57:44
post_excerpt: ""
layout: page
permalink: >
  http://local.mesodocs.com/command-reference-3/
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
# dcos config

This command manages your Mesosphere DCOS configuration file.

A default configuration file named `dcos.toml` is created during initial DCOS CLI setup and is located in `~/.dcos/` on Unix, Linux, and OS X systems or at `C:Users<your username>.dcos` on most Windows systems.

`dcos config append <param-name><value>`

Add an additional value to the end of an existing list. Items earlier in the list take precedence over items that follow. For example, you can append a local repository to the `packages.sources` list so that it is searched after the Mesosphere repository package.

`dcos config --help`, `-h`

Show a description of all command options and positional arguments for the command.

`dcos config --info`

Show a description of command.

`dcos config prepend <param-name> <value>`

Add an additional value to the beginning of an existing list. Items earlier in the list take precedence over items that follow. For example, you can prepend a local repository to the `packages.sources` list so that it is searched before the Mesosphere repository package.

`dcos config set <param-name> <value>`

Add a parameter to the DCOS configuration file.

`dcos config show [<param-name>]`

Display the DCOS configuration file contents.

`dcos config unset [--index=<index>] <param-name>`

Remove an entry from an existing configuration parameter list, where <index> is the numerical position in the list that you want to remove.

`dcos config validate`

Validate changes made to the DCOS configuration file. It is recommended that you run this option after making configuration changes.

# dcos marathon

You can control the native DCOS Marathon instance with this command. You can quickly deploy custom applications to DCOS with the `dcos marathon` command.

This command works with the same set of JSON application definitions that are used by the <a href="https://mesosphere.github.io/marathon/docs/rest-api.html#post-/v2/apps" target="_blank">Marathon REST API</a>.

`dcos marathon --config-schema`

Show the configuration schema information.

`dcos marathon --help`, `-h`

Show a description of all command options and positional arguments for the command.

`dcos marathon --info`

Show a description of command.

`dcos marathon app add [<app-resource>]`

Add an application, with an optional application resource specified. For more information about application resources, see <a href="https://mesosphere.github.io/marathon/docs/rest-api.html#post-/v2/apps" target="_blank">Marathon REST API</a>.

`dcos marathon app list`

List the installed applications.

`dcos marathon app remove [--force] <app-id>`

Remove an application, with an optional `--force` flag and required application ID specified.

`dcos marathon app restart [--force] <app-id>`

Restart an application with an optional `--force` flag and required application ID specified.

`dcos marathon app show [--app-version=<app-version>] <app-id>`

Shows the full `marathon.json` definition of an application with an optional application version and a required application ID specified.

`dcos marathon app start [--force] <app-id> [<instances>]`

Start an application with an optional `--force` flag, required application ID, and optional number of instances specified.

`dcos marathon app stop [--force] <app-id>`

Stop an application with an optional `--force` flag and required application ID specified.

`dcos marathon app update [--force] <app-id> [<properties>...]`

Update an application with an optional `--force` flag, required application ID, and optional key-value pairs specified in `<properties>`. Use the equal (=) character as a separator between the key and value. You can also pass JSON or a file via stdin.

`dcos marathon app version list [--max-count=<max-count>] <app-id>`

List the version history of an application with and optional maximum number of entries to fetch and required application ID specified.

`dcos marathon deployment list [<app-id>]`

Show a current list of deployed applications with an optional application ID specified.

`dcos marathon deployment rollback <deployment-id>`

Remove a deployed application with a required application ID specified.

`dcos marathon deployment stop <deployment-id>`

Cancel the in-progress deployment of an application with a required application ID specified.

`dcos marathon deployment watch [--max-count=<max-count>][--interval=<interval>] <deployment-id>`

Monitor deployments with an optional maximum number of entries to fetch, optional number of seconds to wait between monitor calls, and required application ID specified.

`dcos marathon group add [<group-resource>]`

Create a new group as specified by `<group-resource>`. For more information, see the <a href="https://mesosphere.github.io/marathon/docs/rest-api.html#post-/v2/groups" target="_blank">Marathon documentation</a>.

`dcos marathon group list [--json]`

Show the list of groups in optional JSON format.

`dcos marathon group show [--group-version=<group-version>] <group-id>`

Show a detailed view of a group with optional `<group-version>`. You can specify the `<group-version>` as an absolute value or as relative value. The absolute version values must be in ISO8601 date format. Relative values must be specified as a negative integer and they represent the version from the currently deployed group definition.

`dcos marathon group remove [--force] <group-id>`

Remove a group with optional `--force` flag.

`dcos marathon task list [<app-id>]`

List the tasks with an optional application ID specified.

`dcos marathon task show <task-id>`

Show the tasks with a required task ID specified.

# dcos node

This command manages your DCOS nodes.

`dcos node --help`, `-h`

Show a description of all command options and positional arguments for the command.

`dcos node --info`

Show a description of command.

`dcos node --json`

Print a list of JSON-formatted nodes.

`dcos node`

List all Mesos agent nodes running on DCOS.

`dcos node log [--follow --lines=<N>] --master --slave=<slave-id>`

Prints the Mesos logs for the leading master node, agent nodes, or both. Optionally, you can specify:

*   Dynamically update the log (`--follow`)
*   Show the last *N* lines, where 10 is the default (`--lines=<N>`)

`dcos node ssh [--option SSHOPT=VAL ...] [--config-file=<path>] [--user=<user>] (--master | --slave=<slave-id>)`

Establish an SSH connection to the master or agent nodes of your DCOS cluster. Optionally, you can specify:

*   The SSH options (`--option SSHOPT=VAL ...`). For more information, type `man ssh_config` in your terminal. 
*   The path to the SSH configuration file (`--config-file=<path>`) 
*   The SSH user, where the default user is "core" (`--user=<user>`)
*   Proxy the SSH connection through a master node (`--master-proxy`). By default, `dcos node ssh` connects to the private IP of the node, which is only accessible from hosts within the same network, so you must use the `--master-proxy` option to access your cluster from an outside network. For example, in the default AWS configuration, the private slaves are unreachable from the public Internet, but you can SSH to them using this option, which will proxy the SSH connection through the publicly reachable master.

# dcos package

You can use the `dcos package` command to install and manage software packages from the public [Mesosphere repository][1]. The Mesosphere repository stores certified versions of datacenter services.

`dcos package --config-schema`

Show the configuration schema information for a package. The configuration schema is a job representation of data that can be stored locally. If your DCOS service has subcommands and stores data in the CLI configuration file, you can define a configuration schema that defines the data type, defaults, and a potential range of values. If your DCOS service doesn't have subcommands, you cannot define a configuration schema.

`dcos package --help`, `-h`

Show a description of all command options and positional arguments for the command.

`dcos package --info`

Show a description of command.

`dcos package describe <package_name> [--app --cli --config --render --package-versions --options=<file>] --package-version=<package_version>]`

Get specific details for packages, where `<package_name>` is the is value for “name” in the Mesosphere repository.

*   Show details for the app only (`--app`) 
*   Show details for the DCOS CLI only (`--cli`) 
*   Show all possible key-values for building a custom `option.json` file. This file can be used when installing the scheduler. (`--config`) 
*   Render the package's `marathon.json` or `command.json` template with the values from `config.json` and `--options`. If not provided, print the raw templates. 
*   Show all versions for the package (`package-versions`) 
*   Show the path to a JSON file that contains the package installation options (`--options=<file>`)

`dcos package install [--options=<file> --app-id=<app_id> --cli --app] <package_name>`

Install a package, where `<package_name>` is the is value for “name” in the Mesosphere repository. Optionally, you can specify:

*   A custom JSON file (`--options=<file>`) 
*   A specific application ID (`--app-id=<app_id>`) 
*   Install the DCOS CLI only (`--cli`) 
*   Install the app only (`--app`)

`dcos package list [--endpoints --app-id=<app-id> <package_name>]`

List the installed packages. Optionally, you can:

*   Specify a specific application ID (`--app-id=<app_id>`) 
*   Specify the package name (`<package_name>`) 
*   Return the hosts and ports of all the Mesos tasks associated with package (`--endpoints`)

`dcos package search <query>`

Search the Mesosphere repository for packages with the optional `<query>` value specified. You can use complete or partial values for `<query>`.

`dcos package sources`

Show the current package repository source. Possible sources include a local file, HTTPS, and Git.

`dcos package uninstall [--all | --app-id=<app-id>] <package_name>`

Uninstall a package, where `<package_name>` is the is value for “name” in the Mesosphere repository. Optionally, you can specify:

*   All services (`--all`) 
*   A specific application ID (`--app-id=<app_id>`)

`dcos package update [--validate]`

Update the local cache of the package repository and optionally validate the package content.

# dcos service

You can use the `dcos service` command to get the status of DCOS services.

`dcos service --help`, `-h`

Show a description of all command options and positional arguments for the command.

`dcos service --info`

Show a description of command.

`dcos service [--inactive --json]`

List all of the active installed DCOS services. Optionally, you can specify:

*   Show the inactive DCOS services (`--inactive`). Inactive services are those that have been disconnected from the master, but haven't yet reached their failover timeout. 
*   Display in the JSON-format (`--json`). 

`dcos service --version`

Show the version of the DCOS CLI.

`dcos service log [--follow --lines=<N> --ssh-config-file=<path>] <service> [<file>]`

Print the service logs, where `<service>` is the DCOS service name. Optionally, you can specify:

*   Print data as the log file grows (`--follow`)
*   Show the last *N* lines, where 10 is the default (`--lines=<N>`) 
*   The path to the SSH config file. This is used to access the Marathon logs. (`--ssh-config-file=<path>`)
*   The service log filename from the Mesos sandbox (`<file>`). The default file is stdout.

**Important:** To view the native DCOS Marathon logs by using the `dcos service log marathon` command, you must be on the same network or connected by VPN to your cluster. For more information, see [Accessing native DCOS Marathon logs][2].

`dcos service shutdown <service-id>`

Shutdown a service as specified by `<service-id>`. The `<service-id>` is listed in the ID column of `dcos service` output.

# dcos task

This command shows the status of your DCOS tasks.

`dcos task --help`, `-h`

Show a description of all command options and positional arguments for the command.

`dcos task --info`

Show a description of command.

`dcos task`

List all DCOS tasks that are running.

`dcos task [--completed --json <task>]`

Show the DCOS tasks that are running. Optionally, you can specify:

*   Show the completed DCOS tasks (`--completed`)
*   Print task data in JSON format (`--json <task>`)

`dcos task log [--completed --follow --lines=<N>] <task> [<file>]`

Show the task logs.

`<task>` can be the full task ID, a partial task ID, or a regular expression.

If you specify a partial task ID, logs for all task names that contain the partial task ID are displayed. For example, if you have tasks named `spark1`, `spark2`, and `spark3`, then the command `dcos task log spark` will display task logs for all three of these tasks; you do not need to use wildcards.

If you use a regular expression, you must enclose the task ID in double quotation marks and include an asterisk at the end of the task ID. For example, `dcos task log "spark[13]*"` will display information for tasks `spark1` and `spark3`, but not `spark2`.

Optionally, you can:

*   Read logs from an existing file (`<file>`). `stdout` is the default.
*   Dynamically display new logging data as it is added (`--follow`)
*   Show the last *N* lines, where 10 is the default (`--lines=<N>`)

 [1]: https://github.com/mesosphere/universe
 [2]: /logging/system-logs/