---
UID: 56df3dec49a8b
post_title: Using marathon-lb
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>To demonstrate marathon-lb, you can boot a DCOS cluster on AWS to run an internal and external load balancer. The external load balancer will be used for routing external HTTP traffic into the cluster, and the internal load balancer will be used for internal service discovery and load balancing. Since we’ll be doing this on AWS, external traffic will first hit an external load balancer configured to expose our "public" agent nodes.</p>

<h2>Prequisistes</h2>

<ul>
<li><a href="/installing">A DCOS cluster</a></li>
<li>(DCOS and DCOS CLI)[introcli/cli/] are installed.</li>
</ul>

<h2>Steps</h2>

<ol>
<li><p>Install marathon-lb. First, add the multiverse repo:</p>

<pre><code>$ dcos config prepend package.sources https://github.com/mesosphere/multiverse/archive/version-2.x.zip
</code></pre>

<p>Then, update your package list:</p>

<pre><code>$ dcos package update
</code></pre>

<p>Now, install <code>marathon-lb</code></p>

<pre><code>$ dcos package install marathon-lb
</code></pre>

<p>To check that marathon-lb is working, <a href="./administration/managing-a-dcos-cluster-in-aws/#scrollNav-1">find the IP for your public node</a> and navigate to <code>http://&lt;public slave ip&gt;:9090/haproxy?stats</code>. You willl see a statistics report page like this:</p>

<p><img src="https://mesosphere.com/wp-content/uploads/2015/12/lb2.jpg" alt="lb2" width="628" height="440" class="aligncenter size-full wp-image-3821" /></p></li>
<li><p>Set up your internal load balancer. To do this, we must first specify some configuration options for the marathon-lb multiverse package. Create a file called <code>options.json</code> with the following contents:</p>

<pre><code>{ "marathon-lb":{ "name":"marathon-lb-internal", "haproxy-group":"internal", "bind-http-https":false, "role":"" } }
</code></pre>

<p>In this options file, we’re changing the name of the app instance and the name of the HAProxy group. The options file also disables the HTTP and HTTPS forwarding on ports 80 and 443 because it is not needed.</p>

<p>Next, run the install command with the new options:</p>

<pre><code>$ dcos package install --options=options.json marathon-lb
</code></pre></li>
<li><p>Now there are 2 load balancers: an internal load balancer and an external one, which was installed along with marathon-lb. Launch an external version of nginx to demonstrate the features. Launch this app on DCOS by pasting the JSON below into a file called <code>nginx-external.json</code>.</p>

<pre><code>{
  "id": "nginx-external",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx:1.7.7",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 1,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external"
  }
}
</code></pre>

<p>Run the following command to add the app:</p>

<pre><code>$ dcos marathon app add nginx-external.json
</code></pre>

<p>The application definition includes a special label with the key <code>HAPROXY_GROUP</code>. This label tells marathon-lb whether or not to expose the application. The external marathon-lb was started with the <code>--group</code> parameter set to <code>external</code>, which is the default.</p></li>
<li><p>Now, launch the internal nginx.</p>

<pre><code>{ "id": "nginx-internal", "container": { "type": "DOCKER", "docker": { "image": "nginx:1.7.7", "network": "BRIDGE", "portMappings": [ { "hostPort": 0, "containerPort": 80, "servicePort": 10001 } ], "forcePullImage":true } }, "instances": 1, "cpus": 0.1, "mem": 65, "healthChecks": [{ "protocol": "HTTP", "path": "/", "portIndex": 0, "timeoutSeconds": 10, "gracePeriodSeconds": 10, "intervalSeconds": 2, "maxConsecutiveFailures": 10 }], "labels":{ "HAPROXY_GROUP":"internal" } }
</code></pre>

<p>Notice that we’re specifying a servicePort parameter. The servicePort is the port that exposes this service on marathon-lb. By default, port 10000 through to 10100 are reserved for marathon-lb services, so you should begin numbering your service ports from 10000.</p>

<p>Add one more instance of nginx to be exposed both internally and externally:</p>

<pre><code>{
  "id": "nginx-everywhere",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx:1.7.7",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10002 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 1,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external,internal"
  }
}
</code></pre>

<p>Note the servicePort does not overlap with the other nginx instances.</p>

<p>Service ports can be defined either by using port mappings (as in the examples above), or with the <code>ports</code> parameter in the Marathon app definition.</p>

<p>To test the configuration, <a href="/administration/sshcluster/">SSH into one of the instances in the cluster</a> (such as a master), and try</p>

<p><code>curl</code>-ing the endpoints:</p>

<pre><code>$ curl http://marathon-lb.marathon.mesos:10000/

$ curl http://marathon-lb-internal.marathon.mesos:10001/

$ curl http://marathon-lb.marathon.mesos:10002/

$ curl http://marathon-lb-internal.marathon.mesos:10002/
</code></pre>

<p>Each of these should return the nginx ‘Welcome’ page:</p>

<p><img src="https://mesosphere.com/wp-content/uploads/2015/12/lb3.jpg" alt="lb3" width="625" height="625" class="aligncenter size-full wp-image-3822" /></p></li>
</ol>

<h2>Virtual hosts</h2>

<p>An important feature of marathon-lb is support for virtual hosts. This allows you to route HTTP traffic for multiple hosts (FQDNs) and route requests to the correct endpoint. For example, you could have two distinct web properties, <code>ilovesteak.com</code> and <code>steaknow.com</code>, with DNS for both pointing to the same LB on the same port, and HAProxy will route traffic to the correct endpoint based on the domain name.</p>

<p>To test the vhost feature, navigate to the AWS console and look for your public ELB. We’re going to make 2 changes to the public ELB in order to test it. First, we’ll modify the health checks to use HAProxy’s built in health check:</p>

<p><img src="https://mesosphere.com/wp-content/uploads/2015/12/lb4.jpg" alt="lb4" width="624" height="409" class="aligncenter size-full wp-image-3823" /></p>

<p>Change the health check to ping the hosts on port 9090, at the path <code>/_haproxy_health_check</code>. Now, if you navigate to the instances tab, you should see the instances listed as <code>InService</code>, like this:</p>

<p><img src="https://mesosphere.com/wp-content/uploads/2015/12/lb5.jpg" alt="lb5" width="629" height="201" class="aligncenter size-full wp-image-3824" /></p>

<p>Now our ELB is able to route traffic to HAProxy. Next, let’s modify our nginx app to expose our service. To do this, you’ll need to get the public DNS name for the ELB from the <code>Description</code> tab. In this example, my public DNS name is <code>brenden-j-PublicSl-1LTLKZEH6B2G6-1145355943.us-west-2.elb.amazonaws.com</code>.</p>

<p>Modify the external nginx app to look like this:</p>

<pre><code>{
  "id": "nginx-external",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx:1.7.7",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 1,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"brenden-j-PublicSl-1LTLKZEH6B2G6-1145355943.us-west-2.elb.amazonaws.com"
  }
}
</code></pre>

<p>We’ve added the label <code>HAPROXY_0_VHOST</code>, which tells marathon-lb to expose nginx on the external load balancer with a vhost. The <code>0</code> in the label key corresponds to the servicePort index, beginning from 0. If you had multiple servicePort definitions, you would iterate them as 0, 1, 2, and so on.</p>

<p>Now, if you navigate to the ELB public DNS address in your browser, you should see the following:</p>

<p><img src="https://mesosphere.com/wp-content/uploads/2015/12/lb6.jpg" alt="lb6" width="621" height="405" class="aligncenter size-full wp-image-3826" /></p>