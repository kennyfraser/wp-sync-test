---
UID: 56df3deeed82d
post_title: Marathon
post_excerpt: ""
layout: page
published: true
menu_order: 10
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The DCOS uses Marathon to manage the processes and services and is the "init system" for the DCOS. Marathon starts and monitors your applications and services, automatically healing failures.</p>

<p>A native Marathon instance is installed as a part of the DCOS installation. After DCOS has been started, you can manage the native Marathon instance through the web interface at <code>&lt;hostname&gt;/marathon</code> or from the DCOS CLI with the <code>dcos marathon</code> command.</p>

<p>You can create additional Marathon instances for specific users by using the instructions below.</p>

<h1><a name="install"></a>Installing a Marathon instance on DCOS</h1>

<p><strong>Prerequisite</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>.</li>
</ul>

<ol>
<li><p>Install a Marathon instance:</p>

<ul>
<li><p>To install a single Marathon instance, enter this command from the DCOS CLI:</p>

<pre><code>$ dcos package install marathon
</code></pre>

<p>By default the DCOS Service name is <code>marathon-user</code>. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathontask.png" rel="attachment wp-att-1410"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathontask.png" alt="marathontask" width="709" height="44" class="alignnone size-full wp-image-1410" /></a></p></li>
<li><p>To install multiple Marathon instances:</p>

<ol>
<li><p>Create a custom JSON configuration file that includes this entry where <code>&lt;name&gt;</code> is the unique Marathon instance name:</p>

<pre><code> {"marathon": {"framework-name": "marathon-&lt;name&gt;" }}
</code></pre>

<p><strong>Tip:</strong> You must create a separate JSON configuration file for each Marathon instance.</p></li>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code> $ dcos package install --options=&lt;config-file&gt;.json marathon
</code></pre></li>
<li><p>From the DCOS web interface <strong>Services</strong> tab, click on your Marathon service name to navigate to the web interface. For more information, see <a href="/tutorials/marathon-add-user/">Deploying Multiple Marathon Instances</a>.</p></li>
</ol></li>
</ul></li>
</ol>

<h1><a name="uninstall"></a>Uninstalling a Marathon Instance</h1>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package uninstall marathon
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <code>universe/&lt;marathon-user&gt;</code> folder.</p>

<p><strong>Important:</strong> Do not delete the <code>marathon</code> folder. This is the native DCOS Marathon instance.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathon.png" rel="attachment wp-att-1407"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathon-600x482.png" alt="zkmarathon" width="300" height="241" class="alignnone size-medium wp-image-1407" /></a></p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathondelete.png" rel="attachment wp-att-1409"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathondelete-600x331.png" alt="zkmarathondelete" width="300" height="166" class="alignnone size-medium wp-image-1409" /></a></p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
</ol></li>
</ol>

<p>For more information:</p>

<ul>
<li><a href="http://mesosphere.github.io/marathon/docs/" target="_blank">Marathon documentation</a></li>
<li><a href="/tutorials/deploywebapp/">Deploying a Web App</a></li>
</ul>