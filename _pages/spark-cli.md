---
ID: 1417
post_title: Spark CLI
author: Joel Hamill
post_date: 2016-03-08 14:04:17
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/spark-cli/
published: true
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
header:
  - "1"
page_header_0_show_page_header:
  - "0"
page_header_0_size:
  - default
page_header_0_fill_screen:
  - "0"
page_header_0_background:
  - transparent
page_header_0_show_background_image:
  - "0"
page_header_0_show_background_video:
  - "0"
page_header_0_headline:
  - ""
page_header_0_headline_size:
  - default
page_header_0_description:
  - ""
page_header_0_description_size:
  - default
page_header_0_show_image:
  - "0"
page_header_0_content_alignment:
  - center
page_header_0_content_style:
  - dark
page_header_0_actions:
  - "0"
page_header_0_show_actions_footnote:
  - "0"
page_header_0_show_video:
  - "0"
page_header:
  - "1"
page_options_topic_page:
  - ""
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3dedceb49
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "100"
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