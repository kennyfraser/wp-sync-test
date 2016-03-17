---
UID: 56df3dedc13c7
post_title: Kafka CLI
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can run and manage Kafka jobs by using the Kafka CLI.</p>

<dl>
<dt><code>--help</code>, <code>-h</code></dt>
<dd>
<p>Show a description of all command options and positional arguments for the command.</p>
</dd>

<dt><code>--info</code></dt>
<dd>
<p>Show a brief description of the command.</p>
</dd>

<dt><code>--version</code></dt>
<dd>
<p>Show the version of the installed Kafka CLI.</p>
</dd>

<dt><code>broker add</code></dt>
<dd>
<p>Add a new broker.</p>
</dd>

<dt><code>broker list</code></dt>
<dd>
<p>Show the active brokers.</p>
</dd>

<dt><code>broker remove</code></dt>
<dd>
<p>Remove a broker.</p>
</dd>

<dt><code>broker update &lt;id-expr&gt; [options]</code></dt>
<dd>
<p>Update an existing broker. For command syntax and options, type <code>dcos kafka update --help</code> on the command line.</p>
</dd>

<dt><code>broker start</code></dt>
<dd>
<p>Start a broker.</p>
</dd>

<dt><code>broker stop</code></dt>
<dd>
<p>Stop a broker.</p>
</dd>

<dt><code>topic add</code></dt>
<dd>
<p>Add a topic.</p>
</dd>

<dt><code>topic list</code></dt>
<dd>
<p>List the topics.</p>
</dd>

<dt><code>topic rebalance</code></dt>
<dd>
<p>Rebalance the topics.</p>
</dd>

<dt><code>topic update</code></dt>
<dd>
<p>Update a topic.</p>
</dd>
</dl>

<p>For more information, see the <a href="https://github.com/mesosphere/dcos-kafka">Kafka CLI</a> documentation.</p>