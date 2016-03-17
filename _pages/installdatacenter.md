---
UID: 56df3defb6268
post_title: Installing a DCOS service
post_excerpt: ""
layout: page
published: true
menu_order: 53
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can install datacenter application packages directly from the DCOS package <a href="../overview/universe/">repository</a>.</p>

<p><strong>Prerequisite:</strong></p>

<ul>
<li>DCOS must be <a href="../administering/installing/">installed</a>.</li>
</ul>

<p>To install a DCOS service:</p>

<ol>
<li><p>Check for DCOS CLI package updates:</p>

<pre><code> dcos package update
</code></pre></li>
<li><p>Install the datacenter service:</p>

<pre><code> dcos package install &lt;servicename&gt;
</code></pre>

<p>For example, to install Chronos:</p>

<pre><code> dcos package install chronos
</code></pre></li>
<li><p>Verify that the service is successfully installed:</p>

<ul>
<li>From the DCOS CLI: <code>dcos package list</code></li>
<li>From the Mesosphere DCOS web interface: Go to the Services tab and confirm that the datacenter services are running. <a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/services.png" rel="attachment wp-att-1126"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/services-800x486.png" alt="Services page" width="800" height="486" class="alignnone size-large wp-image-1126" /></a></li>
<li>From the Mesos web interface at <code>&lt;hostname&gt;/mesos</code>, verify that the service has registered and is starting tasks.</li>
</ul></li>
</ol>