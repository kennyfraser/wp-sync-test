---
UID: 56df3dedceb49
post_title: Spark CLI
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can run and manage Spark jobs by using the <a href="https://github.com/mesosphere/dcos-spark">Spark CLI</a>.</p>

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
<p>Show the version of the installed Spark CLI.</p>
</dd>

<dt><code>--config-schema</code></dt>
<dd>
<p>Show the Spark CLI configuration schema.</p>
</dd>

<dt><code>run --help</code></dt>
<dd>
<p>Show a description of all <code>dcos spark run</code> command options and positional arguments.</p>
</dd>

<dt><code>run --submit-args=&lt;spark-args&gt;</code></dt>
<dd>
<p>Run a Spark job with the required <code>&lt;spark-args&gt;</code> specified.</p>
</dd>

<dt><code>status &lt;submissionId&gt;</code></dt>
<dd>
<p>Show the status of a Spark job with the required <code>&lt;spark-args&gt;</code> specified.</p>
</dd>

<dt><code>kill &lt;submissionId&gt;</code></dt>
<dd>
<p>Kill the Spark job with the required <code>&lt;spark-args&gt;</code> specified.</p>
</dd>

<dt><code>webui</code></dt>
<dd>
<p>Show the URL of the Spark web interface.</p>
</dd>
</dl>