---
ID: 611
post_title: Spark CLI
post_date: 2016-02-24 14:49:29
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/adding-a-marathon-user-instance-2/563-revision-v1/spark-cli-2/
published: true
post_parent: 564
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
You can run and manage Spark jobs by using the [Spark CLI][1].

`--help`, `-h`

:   Show a description of all command options and positional arguments for the command.

`--info`

:   Show a brief description of the command.

`--version`

:   Show the version of the installed Spark CLI.

`--config-schema`

:   Show the Spark CLI configuration schema.

`run --help`

:   Show a description of all `dcos spark run` command options and positional arguments.

`run --submit-args=<spark-args>`

:   Run a Spark job with the required `<spark-args>` specified.

`status <submissionId>`

:   Show the status of a Spark job with the required `<spark-args>` specified.

`kill <submissionId>`

:   Kill the Spark job with the required `<spark-args>` specified.

`webui`

:   Show the URL of the Spark web interface.

 [1]: https://github.com/mesosphere/dcos-spark