---
ID: 6
post_title: Markdown
author: gitsync
post_date: 2016-02-12 14:28:03
post_excerpt: ""
layout: page
permalink: http://local.gitsync.com/?page_id=6
published: true
---
In this tutorial you install and deploy a containerized Ruby on Rails app named Oinker. With the Oinker app you can post 140-character messages to the internet. With this tutorial you will learn:

<ul>
    <li>How to install a DCOS service</li>
    <li>How to add apps to Marathon</li>
    <li>How to route apps to the public node</li>
    <li>How your apps are discovered</li>
    <li>How to scale your apps</li>
</ul>

<strong>Prerequisites:</strong>

<ul>
    <li><a href="/installing/">DCOS and DCOS CLI</a> are installed.</li>
    <li>DCOS cluster with at least 4 private agent nodes and 1 public node.</li>
    <li>Your DCOS <a href="../administration/managing-a-dcos-cluster-in-aws/#scrollNav-1">public node hostname</a>.</li>
</ul>

<h1>Add the Cassandra database</h1>

The <a href="../manage-service/cassandra/">Cassandra</a> database is used on the back end to store the Oinker app data. Cassandra is installed with a single-command with the DCOS CLI as a DCOS service.

<ol>
    <li>From your terminal, install the Cassandra DCOS service with this single command:
<pre><code>$ dcos package install cassandra
</code></pre>
</li>
    <li>From the DCOS web interface <strong>Services</strong> tab, you can watch Cassandra spin up to at least 4 nodes. You will see the Health status go from Idle to Unhealthy, and finally to Healthy as the nodes come online. <strong>Important:</strong> Do not go on to the next steps until the Cassandra installation is complete and shows as Healthy in the DCOS web interface. It can take up to 10 minutes for Cassandra to initialize with DCOS. <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/services.png" rel="attachment wp-att-1126"><img class="alignnone size-full wp-image-1126" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/services.png" alt="Services page" width="1346" height="818" /></a></li>
</ol>

<h1>Install the Marathon load balancer</h1>

In this step you install the Marathon load balancer. The Marathon load balancer (marathon-lb) is a supplementary service discovery tool that works in conjunction with native Mesos DNS. For more information, see the <a href="https://github.com/mesosphere/marathon-lb">Marathon Load Balancer</a> GitHub repository. <strong>Important:</strong> Do not install Marathon Load Balancer until the Cassandra DCOS service is installed and shows status of Healthy in the DCOS web interface.

<ol>
    <li>Add the <a href="../administration/universe/#scrollNav-2">multiverse</a> package repository:
<pre><code>$ dcos config prepend package.sources https://github.com/mesosphere/multiverse/archive/version-1.x.zip
</code></pre>
</li>
    <li>Update your package repository list:
<pre><code>$ dcos package update
</code></pre>
</li>
    <li>Install the marathon-lb package:
<pre><code>$ dcos package install marathon-lb
</code></pre>
</li>
    <li>Verify that the Marathon load balancer is up and running. Use your public agent node IP and navigate to <code>http://&lt;public slave ip&gt;:9090/haproxy?stats</code>. You’ll see a statistics report page like this: <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/lb2.jpg" rel="attachment wp-att-2820"><img class="alignnone size-full wp-image-2820" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/lb2.jpg" alt="lb2" width="628" height="440" /></a> <strong>Tip:</strong> You can find your public agent IP address in AWS by viewing your EC2 instances and searching for nodes with a Public IP and <code>aws:cloudformation:logical-id PublicSlaveServerGroup</code>.</li>
</ol>

<h1>Deploy the containerized app</h1>

In this step you deploy the Oinker containerized app. For more information, see the <a href="https://github.com/mesosphere/oinker">Oinker</a> GitHub repository. <strong>Important:</strong> Do not install the Oinker app until the Cassandra DCOS service is installed and shows status of Healthy in the DCOS web interface.

<ol>
    <li>Create a Marathon app definition file named <code>oinker-with-marathon-lb.json</code> by using nano, or another text editor of your choice. A Marathon app definition file specifies the required parameters for Marathon.
<pre><code>$ nano oinker-with-marathon-lb.json
</code></pre>
</li>
    <li>Add this content to your <code>oinker-with-marathon-lb.json</code> file, where <code>&lt;your-elb-hostname&gt;</code> is your AWS ELB <strong>DNS Name</strong>. Specified here is Oinker Docker container, the resources required by Oinker, Marathon health checks, the Marathon load balancer settings, and a command to initialize the Cassandra database. <!-- Add link to AWS doc for ELB hostname -->
<pre><code>{
    "id": "/oinker-with-marathon-lb",
    "instances": 3,
    "container": {
        "type": "DOCKER",
        "docker": {
            "image": "mesosphere/oinker:shakespeare",
            "network": "BRIDGE",
            "portMappings": [
                {
                    "containerPort": 3000,
                    "hostPort": 0,
                    "servicePort": 10000,
                    "protocol": "tcp"
                }
            ]
        }
    },
    "healthChecks": [{
        "protocol": "HTTP",
        "portIndex": 0
    }],
    "labels":{
        "HAPROXY_GROUP":"external",
        "HAPROXY_0_VHOST":"&lt;your-elb-hostname&gt;"
    },
    "cmd": "until rake cassandra:setup; do sleep 5; done &amp;&amp; rails server",
    "ports": [0],
    "cpus": 0.25,
    "mem": 256.0
}
</code></pre>
</li>
    <li>Start the Oinker containerized app with this command:
<pre><code>$ dcos marathon app add oinker-with-marathon-lb.json
</code></pre>
</li>
    <li>Verify that your app is added to Marathon: <strong>From the CLI</strong>
<pre><code>$ dcos marathon app list
ID                        MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  CONTAINER  CMD                                                                                                                               
/cassandra/dcos           512  0.5    1/1    1/1       ---        mesos    $(pwd)/jre*/bin/java $JAVA_OPTS -classpath cassandra-mesos-framework.jar io.mesosphere.mesos.frameworks.cassandra.framework.Main  
/marathon-lb              256   1     1/1    1/1       ---        DOCKER   [u'sse', u'-m', u'http://master.mesos:8080', u'--health-check', u'--group', u'external']                                          
/oinker-with-marathon-lb  256  0.25   3/3    3/3       ---        DOCKER   until rake cassandra:setup; do sleep 5; done &amp;&amp; rails server               
</code></pre>
<strong>From the Web interface</strong> The DCOS web interface shows the Oinker application being loaded. <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/marathonapptutorial2.png" rel="attachment wp-att-2833"><img class="alignnone size-large wp-image-2833" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/marathonapptutorial2-800x350.png" alt="marathonapptutorial2" width="800" height="350" /></a></li>
    <li>Go to the Amazon EC2 console and find your public ELB. Now, if you navigate to the instances tab, you should see the instances listed as InService that the ELB is serving to. For example: <!-- in production this is usually 3 instances --> <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/ec2-health-check.png" rel="attachment wp-att-2826"><img class="alignnone size-full wp-image-2826" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/ec2-health-check.png" alt="ec2-health-check" width="916" height="223" /></a></li>
</ol>

<strong>Tip:</strong> Mesos DNS generates a hostname for your internal Marathon LB. You can access internal services this command. You must be <a href="../administration/sshcluster/">SSH'd into your cluster</a>.

<pre><code>$ curl http://marathon-lb.marathon.mesos:10000/
</code></pre>

<h1>Try out your app on a public node</h1>

Your containerized Oinker app is now available on a DCOS public node and the ELB is able to route traffic to HAProxy. Service discovery is natively built-in to DCOS through Mesos-DNS. The Cassandra service is assigned the name <code>cassandra-dcos-node.cassandra.dcos.mesos</code> and the Oinker app definition includes this value. DCOS applications and services discover the IP addresses and ports of other applications by making DNS queries or by making HTTP requests through a REST API. For more information about service discovery, see <a href="../administration/service-discovery/">Service Discovery with Mesos-DNS</a>.

<ol>
    <li>Find the public ELB DNS name and enter into your browser. To do this, you’ll need to get the public ELB <strong>DNS Name</strong> from the Description tab in the EC2 Management Console. In this example, my public DNS name is <code>joel-ht5s-PublicSl-WPOWMR3C5291-2090420787.us-west-2.elb.amazonaws.com</code>. <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/ec2-elb-dns.png" rel="attachment wp-att-2828"><img class="alignnone size-large wp-image-2828" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/ec2-elb-dns-800x294.png" alt="ec2-elb-dns" width="800" height="294" /></a></li>
    <li>From the Oinker user interface, try it out by entering some "oinks." <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/oinkertutorial.png" rel="attachment wp-att-1719"><img class="alignnone size-full wp-image-1719" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/oinkertutorial.png" alt="oinkertutorial" width="491" height="261" /></a></li>
</ol>

<h1>Scale your app up or down with the load balancer</h1>

In this step you can see the load balancer in action by removing or adding nodes.

<ol>
    <li>Scale your application down to 2 instances with this command:
<pre><code>$ dcos marathon app update oinker instances=2
</code></pre>
</li>
    <li>Click on Marathon in the DCOS web interface to see the Active deployments. There are 2 instances of Oinker. <a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/oinker-scale-down.png" rel="attachment wp-att-2816"><img class="alignnone size-full wp-image-2816" src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/oinker-scale-down.png" alt="oinker-scale-down" width="530" height="333" /></a></li>
    <li>Go to the Oinker app to see that it's still accessible. Your application should still be up and serving traffic!</li>
    <li>You can scale Oinker back up to 4 instances with this command:
<pre><code>$ dcos marathon app update oinker instances=4
</code></pre>
</li>
</ol>