---
UID: 56df3dede7bdf
post_title: Running multiple Cassandra instances
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>In this tutorial, an additional instance of the DCOS Cassandra service is installed.</p>

<p>For each new instance, you must provide a unique Cassandra instance name in an <code>options.json</code> file and specify this file during Cassandra installation.</p>

<p>By default, the DCOS Cassandra service creates an instance named <code>cassandra</code>. Additional instance names are created as <code>cassandra.&lt;new-user&gt;</code>.</p>

<p><strong>Prerequisites:</strong></p>

<ul>
<li><a href="../administering/installing/">DCOS is installed</a></li>
<li>Sufficient resources to add more Cassandra hosts</li>
</ul>

<ol>
<li><p>Create an <code>options.json</code> file and specify the new Cassandra instance as <code>user3</code>. This new Cassandra instance is named: <code>cassandra.user3</code>.</p>

<pre><code>{
  "cassandra": {
    "cluster-name": "user3"
  }
}
</code></pre></li>
<li><p>From the DCOS CLI, run this command to install Cassandra with the options file specified:</p>

<pre><code>dcos package install cassandra --options=options.json
</code></pre></li>
<li><p>Verify that Cassandra is successfully installed:</p>

<ul>
<li>From the DCOS CLI: <code>dcos package list</code></li>
<li>From the Mesosphere DCOS web interface, go to the Services tab and confirm that the new Cassandra instance is running. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandrauser3.png" rel="attachment wp-att-1282"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandrauser3.png" alt="cassandrauser3" width="669" height="48" class="alignnone size-full wp-image-1282" /></a> </li>
<li><p>From the Mesos web interface at <code>&lt;hostname&gt;/mesos</code>, verify that the Cassandra framework has registered and is starting tasks.</p>

<p><strong>Tip:</strong> It can take up to 6 minutes for the Cassandra ring to bootstrap - during this time, the DCOS web interface may show the service as Unhealthy. There should be 6 tasks running with the following IDs:</p></li>
</ul>

<p>cassandra.dcos.node.0.executor cassandra.dcos.node.0.executor.server cassandra.dcos.node.1.executor cassandra.dcos.node.1.executor.server cassandra.dcos.node.2.executor cassandra.dcos.node.2.executor.server</p>

<ul>
<li><p>To run advanced Cassandra jobs, you must SSH into your cluster. For more information on SSHing into your cluster, see &#091;Establishing an SSH Connection&#093;&#091;5&#093;.</p>

<p>For example, to view several system tables in the Cassandra servers, from any node in your cluster, run the following command. Because this example downloads a Docker file, it may take a while.</p>

<p>$ docker run -i -t --net=host --entrypoint=/usr/bin/cqlsh spotify/cassandra -e " SELECT * FROM system.schema_keyspaces ; SELECT * FROM system.schema_columns ; SELECT * FROM system.schema_columnfamilies ;" cassandra-dcos-node.cassandra.dcos.mesos 9160</p></li>
</ul></li>
<li><p>Access the Cassandra cluster by using the DNS name created by Mesos-DNS: <code>cassandra-user3-node.cassandra.user3.mesos</code>.</p>

<p>For more information on Mesos-DNS naming, see <a href="https://docs.mesosphere.com/administration/service-discovery/service-naming/">Service Naming</a>.</p></li>
</ol>