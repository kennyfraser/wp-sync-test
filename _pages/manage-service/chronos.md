---
UID: 56df3def24ea7
post_title: Chronos
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Chronos is the "cron" for your Mesosphere DCOS. It is a highly-available distributed job scheduler, providing the most robust way to run batch jobs in your datacenter. Chronos schedules jobs across the Mesos cluster and manages dependencies between jobs in an intelligent way.</p>

<ul>
<li><a href="#chronosinstall">Installing Chronos on DCOS</a></li>
<li><a href="#uninstall">Uninstalling Chronos</a></li>
</ul>

<h3><a name="chronosinstall"></a>Installing Chronos on DCOS</h3>

<p><strong>Prerequisite</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>.</li>
</ul>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package install chronos
</code></pre>

<p><strong>Tip:</strong> You can specify a JSON configuration file along with the Chronos installation command: <code>dcos package install chronos --option &lt;config_file&gt;</code>. For more information, see the <a href="../administration/introcli/command-reference/">dcos package section of the CLI command reference</a>.</p></li>
<li><p>Verify that Chronos is installed:</p>

<ul>
<li>From the DCOS CLI: <code>dcos package list</code></li>
<li>From the DCOS web interface, go to the Services tab and confirm that Chronos is running. You can click on the Chronos service to go the web interface. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chronostask.png" rel="attachment wp-att-1512"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chronostask.png" alt="chronostask" width="710" height="41" class="alignnone size-full wp-image-1512" /></a></li>
</ul></li>
</ol>

<h3><a name="uninstall"></a>Uninstalling Chronos</h3>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package uninstall chronos
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <code>chronos</code> folder.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a></p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkchronosdelete.png" rel="attachment wp-att-1617"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkchronosdelete.png" alt="zkchronosdelete" width="613" height="339" class="alignnone size-full wp-image-1617" /></a></p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
</ol></li>
</ol>

<p>For more information:</p>

<ul>
<li><a href="http://mesos.github.io/chronos/docs/" target="_blank">Chronos documentation</a></li>
</ul>