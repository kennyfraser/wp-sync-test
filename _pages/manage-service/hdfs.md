---
UID: 56df3def147be
post_title: HDFS
post_excerpt: ""
layout: page
published: true
menu_order: 5
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>HDFS can be used to store and distribute data across your entire Mesosphere DCOS cluster.</p>

<h1><a name="hdfsinstall"></a>Installing HDFS on DCOS</h1>

<p><strong>Prerequisites</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>.</li>
<li>A DCOS cluster with a minimum of 5 private agents with at least 2 CPUs and 8 GB of RAM available for the HDFS Service.</li>
</ul>

<ol>
<li><p>Install the HDFS DCOS service by using the default or custom installation:</p>

<ul>
<li><p>Default installation:</p>

<ol>
<li><p>Enter this command:</p>

<pre><code>$ dcos package install hdfs
</code></pre></li>
</ol></li>
<li><p>Custom installation</p>

<ol>
<li><p>Create an <code>options.json</code> file with additional configuration options. For example:</p>

<pre><code>{
  "hdfs": {
  "framework-name": "different-name-for-hdfs"
  }
}
</code></pre></li>
<li><p>Install the HDFS package with the <code>options.json</code> file specified:</p>

<pre><code>$ dcos package install --options=options.json hdfs
</code></pre></li>
</ol></li>
</ul></li>
<li><p>Verify that HDFS is successfully installed: <strong>Tip:</strong> HDFS can take up to 5 minutes to launch and fully verify its components.</p>

<ul>
<li>From the DCOS CLI: <code>dcos package list</code></li>
<li>From the Mesosphere DCOS web interface, go to the Services tab and confirm that HDFS running. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/hdfstask.png" rel="attachment wp-att-1524"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/hdfstask.png" alt="hdfstask" width="721" height="41" class="alignnone size-full wp-image-1524" /></a></li>
<li>From the Mesos web interface at <code>&lt;hostname&gt;/mesos</code>, verify that the HDFS service has registered and is starting tasks. There should be several journalnodes, namenodes, and datanodes running as tasks. Wait for all of these to show the RUNNING state.</li>
</ul></li>
<li><p>Optional: To run advanced HDFS operations, <a href="../administration/sshcluster/">SSH into your cluster</a>.</p>

<ol>
<li><p>Using SSH, run the following commands on the agent node to test HDFS:</p>

<pre><code>$ hadoop fs -ls hdfs://hdfs/

$ echo “this is a test” &gt; test.txt

$ hadoop fs -put test.txt hdfs://hdfs/

$ hadoop fs -cat hdfs://hdfs/test.txt
</code></pre></li>
</ol></li>
</ol>

<h1><a name="uninstall"></a>Uninstalling HDFS</h1>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package uninstall hdfs
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <strong>hadoop-ha -&gt; hdfs</strong> folder.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkhdfs2.png" rel="attachment wp-att-1620"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkhdfs2.png" alt="zkhdfs2" width="399" height="246" class="alignnone size-full wp-image-1620" /></a></p></li>
<li><p>Click the <strong>Modify...</strong> button at the bottom of the page.</p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkhdfsdelete.png" rel="attachment wp-att-1621"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkhdfsdelete.png" alt="zkhdfsdelete" width="537" height="285" class="alignnone size-full wp-image-1621" /></a></p></li>
<li><p>You will be prompted to confirm the deletion. If you are sure you want to delete, click the OK button.</p></li>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <strong>hdfs-mesos</strong> folder.</p></li>
<li><p>Repeat the procedure for removing the folder: Click the <strong>Modify...</strong> button, choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
</ol></li>
<li><p>For a complete uninstall, you must delete the data directories for HDFS in the individual agent nodes.</p>

<ol>
<li><p>Identify the HDFS agent nodes to delete by using the DCOS web interface <a href="/getting-started/webinterface/#nodes">Nodes tab</a>. For example, to identify all of the HDFS agent nodes choose the the <strong>hdfs</strong> filter on the Nodes tab:</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/nodeshdfs.png" rel="attachment wp-att-1571"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/nodeshdfs-600x419.png" alt="nodeshdfs" width="300" height="210" class="alignnone size-medium wp-image-1571" /></a></p></li>
<li><p><a href="../administration/sshcluster/">SSH to your agent node</a>.</p></li>
<li><p>Navigate to your data directory in the agent node. In DCOS, the default location for HDFS data directories is <code>/var/lib/hdfs</code>.</p>

<pre><code>$ cd /var/lib
</code></pre></li>
<li><p>Delete the data directory for HDFS:</p>

<pre><code>$ sudo rm -rf &lt;service-name&gt;
</code></pre></li>
<li><p>Close your agent node SSH connection:</p>

<pre><code>$ exit
</code></pre></li>
<li><p>Repeat these steps for each agent node.</p></li>
</ol></li>
</ol>

<p>For more information, see the <a href="https://github.com/mesosphere/hdfs/" target="_blank">HDFS on Mesos documentation</a>.</p>