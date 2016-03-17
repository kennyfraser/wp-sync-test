---
UID: 56df3def0bb8f
post_title: Kafka
post_excerpt: ""
layout: page
published: true
menu_order: 6
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Kafka is a distributed, partitioned, replicated commit log service. It provides the functionality of a messaging system, but with a unique design. The Kafka DCOS service also has its own CLI as well.</p>

<h1><a name="kafkainstall"></a>Installing Kafka on DCOS</h1>

<p><strong>Prerequisite</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>.</li>
</ul>

<ol>
<li><p>From the DCOS CLI, enter this command to install the Kafka DCOS service and CLI:</p>

<pre><code>$ dcos package install kafka
</code></pre>

<p><strong>Tip:</strong> It can take a few minutes to download and complete deployment, depending on your internet connection.</p></li>
<li><p>Verify that Kafka is installed:</p>

<ul>
<li>From the DCOS CLI: <code>dcos package list</code> </li>
<li>View the Kafka CLI command options: <code>dcos kafka help</code></li>
<li>From the DCOS web interface, go to the <strong>Services</strong> tab and confirm that the Kafka is running. <img src="https://github.com/mesosphere/dcos-kafka" alt="" /></li>
</ul></li>
</ol>

<h1><a name="uninstall"></a>Uninstalling Kafka</h1>

<ol>
<li><p>From the DCOS CLI, enter this command:</p>

<pre><code>$ dcos package uninstall kafka
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on the <strong>Explorer</strong> tab and navigate to the <code>kafka-mesos</code> folder.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkkafka.png" rel="attachment wp-att-1395"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkkafka-600x456.png" alt="zkkafka" width="300" height="228" class="alignnone size-medium wp-image-1395" /></a></p></li>
<li><p>Click the <strong>Modify</strong> button on the lower-left side of browser.</p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkkafkadelete.png" rel="attachment wp-att-1393"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkkafkadelete-600x331.png" alt="zkkafkadelete" width="300" height="166" class="alignnone size-medium wp-image-1393" /></a></p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
<li><p>If a broker is added to Kafka, repeat steps 1-3 for <code>admin</code>, <code>brokers</code>, <code>config</code>, <code>consumers</code>, and <code>controller_epoch</code> folders. And if it exists, the <code>controller</code> znode.</p>

<p><strong>Optional:</strong> By default the Kafka broker logs are written into their Mesos task sandboxes, which are automatically cleaned up if necessary. If you created your brokers by using <code>--options log.dirs</code> to specify an alternative log location, you must clear that data manually to completely uninstall Kafka:</p>

<ol>
<li><p>Identify the agent nodes that were running your Kafka brokers using the Mesos UI.</p></li>
<li><p><a href="/sshcluster/">SSH into each broker agent</a> and delete the data written to the directory you specified with the <code>--options log.dirs</code> option.</p></li>
</ol></li>
</ol></li>
</ol>

<p>For more information:</p>

<ul>
<li><a href="https://github.com/mesosphere/kafka/blob/master/README.md" target="_blank">Kafka Mesos Framework</a></li>
<li><a href="http://kafka.apache.org/documentation.html" target="_blank">Apache Kafka Documentation</a></li>
</ul>