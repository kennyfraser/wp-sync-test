---
ID: 2779
post_title: Environment Variables
author: Joel Hamill
post_date: 2016-03-08 14:03:40
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/environment-variables/
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
UID:
  - 56df3dec90a96
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "3"
---
The DCOS CLI supports several environment variables that you can set dynamically.

`DCOS_CONFIG` : Sets the path to the DCOS configuration file. By default, this variable is not set.

`DCOS_SSL_VERIFY` : Sets whether to verify SSL certificates for HTTPS, or sets the path to the SSL certificates. Default value is `true`. Equivalent to setting the `core.ssl_config` option in the DCOS configuration file.

`DCOS_LOG_LEVEL` : If set, log messages are printed to `stderr` at or above this level. Valid values, ordered from most verbose to least verbose, are `debug`, `info`, `warning`, `error`, and `critical`. Equivalent to the `--log-level` command-line option.

`DCOS_DEBUG` : If set to `true`, print additional debug messages to `stdout`.