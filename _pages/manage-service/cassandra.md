---
UID: 56df3def2fedc
post_title: Cassandra
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Cassandra is an open source distributed database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure. Cassandra is well suited to run on Mesos due to its peer-to-peer architecture. Cassandra has great features that back today’s web applications with its horizontal scalability, no single point of failure, and a simple query language (CQL).</p>

<p>For information about how Mesos DNS is implemented for the Cassandra DCOS service, see the <a href="http://mesosphere.github.io/cassandra-mesos/docs/mesos-dns.html" target="_blank">Cassandra-Mesos documentation</a>.</p>

<h1><a name="cassandrainstall"></a>Installing Cassandra on DCOS</h1>

<p><strong>Prerequisite</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>.</li>
<li>A DCOS cluster with at least 3 private nodes, each with 0.3 CPU shares, 1184MB of memory and 272MB of disk.</li>
</ul>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package install cassandra
</code></pre></li>
<li><p>Verify that Cassandra is successfully installed:</p>

<ul>
<li>From the DCOS CLI: <code>dcos package list</code></li>
<li>From the Mesosphere DCOS web interface, go to the Services tab and confirm that Cassandra running. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandratask.png" rel="attachment wp-att-1326"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandratask.png" alt="cassandratask" width="713" height="45" class="alignnone size-full wp-image-1326" /></a> </li>
<li>From the Mesos web interface at <code>&lt;hostname&gt;/mesos</code>, verify that the Cassandra service has registered and is starting tasks. </li>
</ul>

<p><strong>Tip:</strong> It can take up to 6 minutes for the Cassandra ring to bootstrap - during this time, the DCOS web interface may show the service as Unhealthy. There should be 6 tasks running with the following IDs:</p>

<pre><code>cassandra.dcos.node.0.executor
cassandra.dcos.node.0.executor.server
cassandra.dcos.node.1.executor
cassandra.dcos.node.1.executor.server
cassandra.dcos.node.2.executor
cassandra.dcos.node.2.executor.server
</code></pre></li>
<li><p>To run advanced Cassandra jobs, you must SSH into your cluster. For more information on SSHing into your cluster, see <a href="/administration/sshcluster/">Establishing an SSH Connection</a>.</p>

<p>For example, to view several system tables in the Cassandra servers, from any node in your cluster, run the following command. Because this example downloads a Docker file, it may take a while.</p>

<pre><code>$ docker run -i -t --net=host 
    --entrypoint=/usr/bin/cqlsh 
    spotify/cassandra -e "
       SELECT * FROM system.schema_keyspaces ;
       SELECT * FROM system.schema_columns ;
       SELECT * FROM system.schema_columnfamilies ;" 
    cassandra-dcos-node.cassandra.dcos.mesos 9160
</code></pre></li>
</ol>

<h1><a name="uninstall"></a>Uninstalling Cassandra</h1>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package uninstall cassandra
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <code>cassandra-mesos</code> folder.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandra.png" rel="attachment wp-att-1329"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandra-150x150.png" alt="zkcassandra" width="150" height="150" class="alignnone size-thumbnail wp-image-1329" /></a></p></li>
<li><p>Click the <strong>Modify</strong> button on the lower-left side of browser.</p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandradelete.png" rel="attachment wp-att-1332"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandradelete-600x334.png" alt="zkcassandradelete" width="300" height="167" class="alignnone size-medium wp-image-1332" /></a></p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
</ol></li>
<li><p>Clear your data directories. By default the Cassandra DCOS Service data and log directories are written into the Mesos task sandbox. You can change this by setting the <code>cassandra.data-directory</code> option when you install the Cassandra DCOS Service.</p></li>
</ol>

<p>For more information:</p>

<ul>
<li><a href="http://mesosphere.github.io/cassandra-mesos/" target="_blank">Cassandra Mesos documentation</a></li>
<li><a href="../getting-started/tutorials/deploy-containerized-app/">Deploying a Containerized App on a Public Node</a></li>
</ul>