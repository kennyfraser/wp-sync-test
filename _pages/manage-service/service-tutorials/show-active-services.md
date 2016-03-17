---
UID: 56df3deda3ea0
post_title: Monitoring DCOS Services
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can monitor the installed DCOS services and their health through the DCOS web interface or command line interface.</p>

<p><strong>Prerequisites:</strong></p>

<ul>
<li><a href="../getting-started/installing/">DCOS cluster</a> is up and running.</li>
<li><a href="../install/cli/">DCOS CLI</a> is installed and configured.</li>
</ul>

<h1>Monitoring DCOS services in the DCOS web interface</h1>

<p>From the DCOS web interface, click the <strong>Services</strong> tab. In this example you can see the installed DCOS services Cassandra, Chronos, and HDFS. All of the services are showing a status of Healthy.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/services.png" rel="attachment wp-att-1126"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/services-800x486.png" alt="Services page" width="800" height="486" class="alignnone size-large wp-image-1126" /></a></p>

<ul>
<li><strong>SERVICE NAME</strong> Displays the DCOS service name.</li>
<li><strong>HEALTH</strong> Displays the <a href="https://mesosphere.github.io/marathon/docs/health-checks.html">Marathon health check</a> status for the service.</li>
<li><strong>TASKS</strong> Display the number of running tasks.</li>
<li><strong>CPU</strong> Displays the percentage of CPU in use.</li>
<li><strong>MEM</strong> Displays the amount of memory used.</li>
<li><strong>DISK</strong> Displays the amount of disk space used.</li>
</ul>

<p>For more information about the Services tab, see the DCOS web interface <a href="https://docs.mesosphere.com/administration/webinterface/#scrollNav-2">documentation</a>.</p>

<h1>Monitoring DCOS services in the DCOS CLI</h1>

<p>From the DCOS CLI, enter the <code>dcos service</code> command. In this example you can see the native Marathon instance, and the installed DCOS services Chronos, HDFS, and Kafka.</p>

<pre><code>$ dcos service
NAME      HOST             ACTIVE  TASKS CPU   MEM     DISK   ID                                         
chronos   &lt;privatenode1&gt;   True     0    0.0    0.0     0.0   &lt;service-id1&gt;  
hdfs      &lt;privatenode2&gt;   True     1    0.35  1036.8   0.0   &lt;service-id2&gt;  
kafka     &lt;privatenode3&gt;   True     0    0.0    0.0     0.0   &lt;service-id3&gt; 
marathon  &lt;privatenode3&gt;   True     3    2.0   1843.0  100.0  &lt;service-id4&gt;
</code></pre>

<ul>
<li><strong>NAME</strong> Displays the DCOS service name.</li>
<li><strong>HOST</strong> Displays the private agent node where the service running.</li>
<li><strong>ACTIVE</strong> Indicates whether the service is connected to a scheduler.</li>
<li><strong>TASK</strong> Displays the number of running tasks.</li>
<li><strong>CPU</strong> Displays the percentage of CPU in use.</li>
<li><strong>MEM</strong> Displays the amount of memory used.</li>
<li><strong>DISK</strong> Displays the amount of disk space used.</li>
<li><strong>ID</strong> Displays the DCOS service framework ID. This value is automatically generated and is unique across the cluster.</li>
</ul>