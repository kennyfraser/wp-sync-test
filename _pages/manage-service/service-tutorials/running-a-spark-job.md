---
UID: 56df3dee0fe80
post_title: Running a Spark Job
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>In this tutorial a Spark job is run by using DCOS.</p>

<p><strong>Prerequisites:</strong></p>

<ul>
<li><a href="../install/awscluster/">Running DCOS cluster</a> with at least 3 private agent nodes.</li>
<li><a href="../install/cli/">Configured DCOS CLI</a>.</li>
<li>Upload your Spark application .jar to an external file server that is reachable by the DCOS cluster. For example: <code>http://external.website/mysparkapp.jar</code>.</li>
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
<li><p>To run a job with the main method in a class called <code>MySampleClass</code> and arguments 10:</p>

<pre><code>$ dcos spark run --submit-args=’--class MySampleClass http://external.website/mysparkapp.jar 10’
</code></pre>

<p>You can also upload jar files and reference those jars in your job. For example:</p>

<pre><code>$ dcos spark run --submit-args='-Dspark.mesos.coarse=true --driver-cores 1 --driver-memory 1024M --class org.apache.spark.examples.SparkPi https://downloads.mesosphere.com/spark/assets/spark-examples_2.10-1.4.0-SNAPSHOT.jar 30'
</code></pre></li>
<li><p>View the Spark scheduler progress by navigating to the Spark web interface as shown with this command:</p>

<pre><code>$ dcos spark webui
</code></pre></li>
<li><p>Set the <code>--supervise</code> flag when running the job to allow the scheduler to restart the job anytime the job finishes.</p>

<pre><code>$ dcos spark run --submit-args=’--supervise --class MySampleClass http://external.website/mysparkapp.jar 10’
</code></pre></li>
<li><p>You can set the amount of cores and memory that the driver will require to be scheduled by passing in <code>--driver-cores</code> and <code>--driver-memory</code> flags. Any options that the usual spark-submit script accepts are also accepted by DCOS Spark CLI.</p></li>
<li><p>To set any Spark properties (e.g. coarse grain mode or fine grain mode), you can also provide a custom <code>spark.properties</code> file and set the environment variable <code>SPARK_CONF_DIR</code> to that directory.</p></li>
</ol>