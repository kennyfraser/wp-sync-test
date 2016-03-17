---
UID: 56df3ded9de0b
post_title: Customizing Your Spark Installation
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<h1>Installing the Spark app only, not CLI</h1>

<p>In this example, the Spark app is installed without the Spark CLI:</p>

<pre><code>$ dcos package install —app spark
</code></pre>

<h1>Installing Spark with A Custom Name</h1>

<p>In this example, Spark is installed with the explicit application ID <code>alice-spark</code> specified:</p>

<pre><code>$ dcos package install --app-id=alices-spark spark
Installing package [spark] version [1.4.0-SNAPSHOT] with app id [alices-spark]
Installing CLI subcommand for package [spark]
Spark cluster mode on Mesos is ready!
</code></pre>