---
UID: 56df3dedef592
post_title: >
  Autoscaling based on memory and CPU
  metrics
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>A Python application, marathon-autoscale.py, autoscales your Marathon application based on the utilization metrics Mesos reports.</p>

<p>The application runs on any system that has Python 3 installed and has access to the Marathon server via HTTP TCP Port 80 and the Mesos Agent nodes over TCP port 5051. marathon-autoscale.py is intended to demonstrate what is possible when you run your applications on Marathon and Mesos. It periodically monitors the aggregate CPU and memory utilization for all the individual tasks running on the Mesos agents that make up the specified Marathon application. When the threshold you set for these metrics is reached, marathon-autoscale.py adds instances to the application based on the multiplier you specify.</p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>A <a href="https://docs.mesosphere.com/install/awscluster/">running DCOS cluster</a>.</li>
<li>Python 3 installed on the system you will run marathon-autoscale.py. <strong>Note:</strong> This can be one of the Mesos master nodes.</li>
<li>An application running on the native Marathon instance that you intend to autoscale.</li>
<li>TCP Port 80 open to the Marathon instance and TCP Port 5051 open to the Mesos Agent hosts.</li>
</ul>

<h1>Install the application</h1>

<p>SSH to the system where you will run marathon-autoscale.py and install it:</p>

<pre><code>    $ git clone https://github.com/mesosphere/marathon-autoscale.git 
    $ cd marathon-autoscale
</code></pre>

<h1>Run the application</h1>

<p>When you run the application, you'll be prompted for the following parameters:</p>

<ul>
<li><strong>marathon_host (string)</strong> - Fully qualified domain name or IP of the Marathon host (without http://).</li>
<li><strong>marathon_app (string)</strong> - The name of the Marathon app to autoscale (without "/").</li>
<li><strong>max_mem_percent (int)</strong> - The percentage of average memory utilization across all tasks for the target Marathon app before scaleout is triggered.</li>
<li><strong>max_cpu_time (int)</strong> - The average CPU time across all tasks for the target Marathon app before scaleout is triggered.</li>
<li><strong>trigger_mode (string)</strong> - 'or' or 'and' determines whether both CPU and memory must be triggered or just one or the other.</li>
<li><strong>autoscale_multiplier (float)</strong> - The number by which current instances will be multiplied to determine how many instances to add during scaleout.</li>
<li><p><strong>max_instances (int)</strong> - The ceiling for the number of instances to stop scaling out EVEN if thresholds are crossed.</p>

<p>$ python marathon-autoscale.py Enter the DNS hostname or IP of your Marathon Instance : ip-**-<em>-</em>-*** Enter the Marathon Application Name to Configure Autoscale for from the Marathon UI : testing Enter the Max percent of Mem Usage averaged across all Application Instances to trigger Autoscale (ie. 80) : 5 Enter the Max percent of CPU Usage averaged across all Application Instances to trigger Autoscale (ie. 80) : 5 Enter which metric(s) to trigger Autoscale ('and', 'or') : or Enter Autoscale multiplier for triggered Autoscale (ie 1.5) : 2 Enter the Max instances that should ever exist for this application (ie. 20) : 10</p></li>
</ul>