---
UID: 56eac5674ae21
post_title: Environment Variables
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The DCOS CLI supports several environment variables that you can set dynamically.

`DCOS_CONFIG` : Sets the path to the DCOS configuration file. By default, this variable is not set.

`DCOS_SSL_VERIFY` : Sets whether to verify SSL certificates for HTTPS, or sets the path to the SSL certificates. Default value is `true`. Equivalent to setting the `core.ssl_config` option in the DCOS configuration file.

`DCOS_LOG_LEVEL` : If set, log messages are printed to `stderr` at or above this level. Valid values, ordered from most verbose to least verbose, are `debug`, `info`, `warning`, `error`, and `critical`. Equivalent to the `--log-level` command-line option.

`DCOS_DEBUG` : If set to `true`, print additional debug messages to `stdout`.