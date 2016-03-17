---
UID: 56df3def7af97
post_title: Service Logging
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The DCOS services run as Mesos tasks and print their logs to <code>stdout</code> and <code>stderr</code>. The log content varies from service to service, but usually includes task launches, resource accepts, and resource rejects.</p>

<p>You can access these logs in two ways:</p>

<ul>
<li><p>The <code>dcos service log</code> command. For example, enter this command to view the Chronos logs:</p>

<pre><code>  dcos service log chronos
</code></pre>

<p>For more information, see <a href="../introcli/command-reference/#scrollNav-5">dcos service log</a>.</p></li>
<li><p>The Mesos web interface:</p>

<ol>
<li><p>Go to the Mesos web interface at <code>&lt;hostname&gt;/mesos</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p></li>
<li><p>Find the row for your desired service and click the <strong>Sandbox</strong> link to view or download the logs.</p>

<p><strong>Tip:</strong> You can use the filters to narrow the scope.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/mesos-sandbox-log.png" rel="attachment wp-att-1559"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/mesos-sandbox-log.png" alt="mesos-sandbox-log" width="898" height="311" class="alignnone size-full wp-image-1559" /></a></p></li>
</ol></li>
</ul>