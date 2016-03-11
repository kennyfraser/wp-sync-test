---
ID: 1425
post_title: Kafka CLI
author: Joel Hamill
post_date: 2016-03-08 14:04:14
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/kafka-cli/
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
  - 56df3dedc13c7
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "100"
---
You can run and manage Kafka jobs by using the Kafka CLI.

`--help`, `-h`

:   Show a description of all command options and positional arguments for the command.

`--info`

:   Show a brief description of the command.

`--version`

:   Show the version of the installed Kafka CLI.

`broker add`

:   Add a new broker.

`broker list`

:   Show the active brokers.

`broker remove`

:   Remove a broker.

`broker update <id-expr> [options]`

:   Update an existing broker. For command syntax and options, type `dcos kafka update --help` on the command line.

`broker start`

:   Start a broker.

`broker stop`

:   Stop a broker.

`topic add`

:   Add a topic.

`topic list`

:   List the topics.

`topic rebalance`

:   Rebalance the topics.

`topic update`

:   Update a topic.

For more information, see the [Kafka CLI][1] documentation.

 [1]: https://github.com/mesosphere/dcos-kafka