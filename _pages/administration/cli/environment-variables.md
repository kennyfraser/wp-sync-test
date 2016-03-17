---
UID: 56df3dec90a96
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
<p>The DCOS CLI supports several environment variables that you can set dynamically.</p>

<p><code>DCOS_CONFIG</code> : Sets the path to the DCOS configuration file. By default, this variable is not set.</p>

<p><code>DCOS_SSL_VERIFY</code> : Sets whether to verify SSL certificates for HTTPS, or sets the path to the SSL certificates. Default value is <code>true</code>. Equivalent to setting the <code>core.ssl_config</code> option in the DCOS configuration file.</p>

<p><code>DCOS_LOG_LEVEL</code> : If set, log messages are printed to <code>stderr</code> at or above this level. Valid values, ordered from most verbose to least verbose, are <code>debug</code>, <code>info</code>, <code>warning</code>, <code>error</code>, and <code>critical</code>. Equivalent to the <code>--log-level</code> command-line option.</p>

<p><code>DCOS_DEBUG</code> : If set to <code>true</code>, print additional debug messages to <code>stdout</code>.</p>