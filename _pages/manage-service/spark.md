---
UID: 56df3deee69d0
post_title: Spark
post_excerpt: ""
layout: page
published: true
menu_order: 9
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Spark is a fast, general-purpose cluster computing system that makes parallel jobs easy to write.</p>

<h1><a name="sparkinstall"></a>Installing Spark on DCOS</h1>

<p><strong>Prerequisites:</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>. </li>
<li>It is recommend that you have minimum of two nodes with at least 2 CPUs and 2 GB of RAM available for the Spark Service and running a Spark job.</li>
</ul>

<ol>
<li><p>From the DCOS CLI, enter this command to install the Spark DCOS service and CLI:</p>

<pre><code>$ dcos package install spark
</code></pre>

<p><strong>Tip:</strong> It can take up to 15 minutes to download and complete deployment, depending on your internet connection.</p></li>
<li><p>Verify that Spark is running:</p>

<ul>
<li>From the DCOS web interface, go to the Services tab and confirm that Spark is running. Click the <strong>spark</strong> row item to view the Spark web interface. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/sparktask.png" rel="attachment wp-att-1236"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/sparktask.png" alt="sparktask" width="717" height="41" class="alignnone size-full wp-image-1236" /></a></li>
<li>From the DCOS CLI: <code>dcos package list</code></li>
<li>From the Mesos web interface at <code>http://&lt;hostname&gt;/mesos</code>, verify that the Spark framework has registered and is starting tasks. There should be several journalnodes, namenodes, and datanodes running as tasks. Wait for all of these to show the RUNNING state.</li>
</ul></li>
</ol>

<h1><a name="uninstall"></a>Uninstalling Spark</h1>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package uninstall spark
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <code>spark_mesos_dispatcher</code> folder.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkspark.png" rel="attachment wp-att-1413"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkspark-600x473.png" alt="zkspark" width="300" height="237" class="alignnone size-medium wp-image-1413" /></a></p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zksparkdelete.png" rel="attachment wp-att-1412"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zksparkdelete-600x336.png" alt="zksparkdelete" width="300" height="168" class="alignnone size-medium wp-image-1412" /></a></p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
</ol></li>
</ol>

<p>For more information, see the <a href="https://github.com/mesosphere/dcos-spark" target="_blank">DCOS Spark CLI</a> documentation on GitHub.</p>