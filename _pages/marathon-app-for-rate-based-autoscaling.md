---
UID: 56df3dee02b2e
post_title: Autoscaling based on app request rate
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can use the <a href="https://github.com/mesosphere/marathon-lb-autoscale">marathon-lb-autoscale</a> application to implement request rate-based autoscaling with Marathon. The marathon-lb-autoscale application works with any application that uses TCP traffic and can be routed through HAProxy.</p>

<p>marathon-lb-autoscale collects data from all HAProxy instances to determine the current RPS (requests per second) for your apps. The autoscale controller then attempts to maintain a defined target number of requests per second per app instance. marathon-lb-autoscale makes API calls to Marathon to scale the app.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathon-lb-autoscale-1.png" rel="attachment wp-att-1259"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathon-lb-autoscale-1.png" alt="marathon-lb-autoscale" width="552" height="604" class="alignnone size-full wp-image-1259" /></a></p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>A <a href="https://docs.mesosphere.com/install/awscluster/">running DCOS cluster</a>.</li>
<li>A <a href="https://docs.mesosphere.com/install/cli/">configured DCOS CLI</a>.</li>
<li>An application running on the native Marathon instance.</li>
<li>Ruby installed on your master node if you will run the application on the command line.</li>
</ul>

<p><strong>Application Parameters</strong></p>

<ul>
<li>marathon (URL) - The URL for Marathon</li>
<li>haproxy (URLs) - A comma-separated list of URLs for HAProxy. If this is a Mesos-DNS A-record, all backends will be polled.</li>
<li>interval (float) - The number of seconds (N) between update intervals. The default is 60.</li>
<li>samples (integer) - The number of samples to average. The default is 10.</li>
<li>cooldown (integer) - The number of additional intervals to wait after making a scale change. The default is 5.</li>
<li>target-rps (integer) - The target number of requests per second per app instance. The default is 1000.</li>
<li>apps (APPS) - A comma-separated list of <code>&lt;app&gt;_&lt;service port&gt;</code> pairs to monitor.</li>
<li>threshold-percent (float) - Scaling will occur when the target RPS differs from the current RPS by at least this amount. The default is 0.5.</li>
<li>threshold-instances (integer) - Scaling will occur when the target number of instances differs from the actual number by at least this amount. The default is 3.</li>
<li>intervals-past-threshold (integer) - An app won't be scaled until it's past it's threshold for this many intervals. The default is 3.</li>
</ul>

<p>Common options: -h, --help - Show this message</p>

<p><strong>Run marathon-lb-autoscale on Marathon</strong></p>

<ol>
<li><p>Create a JSON file</p>

<pre><code>$ nano marathon-lb-autoscale.json
</code></pre></li>
<li><p>Paste this into the file. Edit the arguments to your your desired parameters:</p>

<pre><code>{
  "id": "marathon-lb-autoscale",
  "args":[
    "--marathon", "http://leader.mesos:8080",
    "--haproxy", "http://marathon-lb.marathon.mesos:9090",
    "--apps", "nginx_10000"
  ],
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mesosphere/marathon-lb-autoscale",
      "network": "HOST",
      "forcePullImage": true
    }
  }
}
</code></pre></li>
<li><p>Add the app to Marathon</p>

<pre><code>$ dcos marathon app add marathon-lb-autoscale.json
</code></pre></li>
<li><p>Verify that the app has been added:</p>

<pre><code>$ dcos marathon app list
</code></pre></li>
</ol>

<p><strong>Note:</strong> You can also run autoscale.rb from the command line of your master node if you have Ruby installed.</p>