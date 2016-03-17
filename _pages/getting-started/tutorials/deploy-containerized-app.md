---
UID: 56eaad66e7cb8
post_title: >
  Deploying a Containerized App on a
  Public Node
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
In this tutorial you install and deploy a containerized Ruby on Rails app named Oinker. With the Oinker app you can post 140-character messages to the internet.

With this tutorial you will learn:

*   How to install a DCOS service 
*   How to add apps to Marathon 
*   How to route apps to the public node 
*   How your apps are discovered
*   How to scale your apps

**Prerequisites:**

*   [DCOS and DCOS CLI][1] are installed.
*   DCOS cluster with at least 4 [private agent nodes][2] and 1 [public agent node][2].
*   Your DCOS [public node hostname][3].

# Add the Cassandra database

The [Cassandra][4] database is used on the back end to store the Oinker app data. Cassandra is installed with a single-command with the DCOS CLI as a DCOS service.

1.  From your terminal, install the Cassandra DCOS service with this single command:
    
        $ dcos package install cassandra
        

2.  From the DCOS web interface **Services** tab, you can watch Cassandra spin up to at least 4 nodes. You will see the Health status go from Idle to Unhealthy, and finally to Healthy as the nodes come online.
    
    **Important:** Do not go on to the next steps until the Cassandra installation is complete and shows as Healthy in the DCOS web interface. It can take up to 10 minutes for Cassandra to initialize with DCOS.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/services.png" rel="attachment wp-att-1126"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/services.png" alt="Services page" width="1346" height="818" class="alignnone size-full wp-image-1126" /></a>

# Install the Marathon load balancer

In this step you install the Marathon load balancer. The Marathon load balancer (marathon-lb) is a supplementary service discovery tool that can work in conjunction with native Mesos DNS. For more information, see the [Marathon Load Balancer][5] GitHub repository.

**Important:** Do not install Marathon Load Balancer until the Cassandra DCOS service is installed and shows status of Healthy in the DCOS web interface.

1.  Add the [multiverse][6] package repository. First, check which version of the DCOS CLI is installed:
    
        $ dcos --version
        
    
    If you using DCOS CLI 0.4.0 or above (DCOS version 1.6.1 or above), add the multiverse repository with the following command:
    
        $ dcos package repo add multiverse https://github.com/mesosphere/multiverse/archive/version-2.x.zip
        
    
    If you are using DCOS CLI 0.3.x or below (DCOS version 1.6.0 or below), add the multiverse repository with these commands:
    
        $ dcos config prepend package.sources https://github.com/mesosphere/multiverse/archive/version-2.x.zip
        $ dcos package update --validate
        

2.  Install the marathon-lb package:
    
        $ dcos package install marathon-lb
        

3.  Verify that the Marathon load balancer is up and running. Use your public agent node IP and navigate to `http://<public slave ip>:9090/haproxy?stats`. You’ll see a statistics report page like this:
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/lb2.jpg" rel="attachment wp-att-2820"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/lb2.jpg" alt="lb2" width="628" height="440" class="alignnone size-full wp-image-2820" /></a>
    
    **Tip:** You can find your public agent IP address in AWS by viewing your EC2 instances and searching for nodes with a Public IP and `aws:cloudformation:logical-id PublicSlaveServerGroup`.

# Deploy the containerized app

In this step you deploy the Oinker containerized app. For more information, see the [Oinker][7] GitHub repository.

**Important:** Do not install the Oinker app until the Cassandra DCOS service is installed and shows status of Healthy in the DCOS web interface.

1.  Create a Marathon app definition file named `oinker-with-marathon-lb.json` by using nano, or another text editor of your choice. A Marathon app definition file specifies the required parameters for launching an app with Marathon.
    
        $ nano oinker-with-marathon-lb.json
        

2.  Add this content to your `oinker-with-marathon-lb.json` file, where `<your-elb-hostname>` is your AWS ELB **DNS Name**. Specified here is Oinker Docker container, the resources required by Oinker, Marathon health checks, the Marathon load balancer settings, and a command to initialize the Cassandra database. <!-- Add link to AWS doc for ELB hostname -->
    
        {
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
                "HAPROXY_0_VHOST":"<your-elb-hostname>"
            },
            "cmd": "until rake cassandra:setup; do sleep 5; done && rails server",
            "ports": [0],
            "cpus": 0.25,
            "mem": 256.0
        }
        

3.  Start the Oinker containerized app with this command:
    
        $ dcos marathon app add oinker-with-marathon-lb.json
        

4.  Verify that your app is added to Marathon:
    
    **From the CLI**
    
        $ dcos marathon app list
        ID                        MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  CONTAINER  CMD                                                                                                                               
        /cassandra/dcos           512  0.5    1/1    1/1       ---        mesos    $(pwd)/jre*/bin/java $JAVA_OPTS -classpath cassandra-mesos-framework.jar io.mesosphere.mesos.frameworks.cassandra.framework.Main  
        /marathon-lb              256   1     1/1    1/1       ---        DOCKER   [u'sse', u'-m', u'http://master.mesos:8080', u'--health-check', u'--group', u'external']                                          
        /oinker-with-marathon-lb  256  0.25   3/3    3/3       ---        DOCKER   until rake cassandra:setup; do sleep 5; done && rails server               
        
    
    **From the Web interface**  
    The DCOS web interface shows the Oinker application being loaded.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathonapptutorial2.png" rel="attachment wp-att-2833"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathonapptutorial2-800x350.png" alt="marathonapptutorial2" width="800" height="350" class="alignnone size-large wp-image-2833" /></a>

5.  Go to the Amazon EC2 console and find your public ELB.
    
    1.  Select your public load balancer (contains `-PublicSl-` in the name) and click the **Health Check** tab.
    
    2.  Click **Edit Health Check** and change the **ping port** to `9090` and **ping path** to `/_haproxy_health_check`.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/aws-lb-health-check.png" rel="attachment wp-att-3013"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/aws-lb-health-check.png" alt="aws-lb-health-check" width="615" height="199" class="alignnone size-full wp-image-3013" /></a>
    
    3.  Now, if you navigate to the instances tab, you should see the instances listed as `InService` that the ELB is serving to. For example: <!-- in production this is usually 3 instances -->
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/ec2-health-check.png" rel="attachment wp-att-2826"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/ec2-health-check.png" alt="ec2-health-check" width="916" height="223" class="alignnone size-full wp-image-2826" /></a>

**Tip:** Mesos DNS generates a hostname for your internal Marathon LB. You can access internal services this command. You must be [SSH'd into your cluster][8].

    $ curl http://marathon-lb.marathon.mesos:10000/
    

# Try out your app on a public node

Your containerized Oinker app is now available on a DCOS public node and the ELB is able to route traffic to HAProxy.

Service discovery is natively built-in to DCOS through Mesos-DNS. The Cassandra service is assigned the name `cassandra-dcos-node.cassandra.dcos.mesos` and the Oinker app definition includes this value. DCOS applications and services discover the IP addresses and ports of other applications by making DNS queries or by making HTTP requests through a REST API. For more information about service discovery, see [Service Discovery with Mesos-DNS][9].

1.  Find the public ELB DNS name and enter into your browser. To do this, you’ll need to get the public ELB **DNS Name** from the Description tab in the EC2 Management Console. In this example, my public DNS name is `joel-ht5s-PublicSl-WPOWMR3C5291-2090420787.us-west-2.elb.amazonaws.com`.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/ec2-elb-dns.png" rel="attachment wp-att-2828"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/ec2-elb-dns-800x294.png" alt="ec2-elb-dns" width="800" height="294" class="alignnone size-large wp-image-2828" /></a>

2.  From the Oinker user interface, try it out by entering some "oinks."
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/oinkertutorial.png" rel="attachment wp-att-1719"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/oinkertutorial.png" alt="oinkertutorial" width="491" height="261" class="alignnone size-full wp-image-1719" /></a>

# Scale your app up or down with the load balancer

In this step you can see the load balancer in action by removing or adding nodes.

1.  Scale your application down to 2 instances with this command:
    
        $ dcos marathon app update oinker-with-marathon-lb instances=2
        

2.  Click on Marathon in the DCOS web interface to see the Active deployments. There are 2 instances of Oinker.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/oinker-scale-down.png" rel="attachment wp-att-2816"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/oinker-scale-down.png" alt="oinker-scale-down" width="530" height="333" class="alignnone size-full wp-image-2816" /></a>

3.  Go to the Oinker app to see that it's still accessible. Your application should still be up and serving traffic!

4.  You can scale Oinker back up to 4 instances with this command:
    
        $ dcos marathon app update oinker-with-marathon-lb instances=4

 [1]: /installing/
 [2]: ../administration/terminology/
 [3]: ../administration/managing-a-dcos-cluster-in-aws/#scrollNav-1
 [4]: ../manage-service/cassandra/
 [5]: https://github.com/mesosphere/marathon-lb
 [6]: ../administration/package-repo-overview/
 [7]: https://github.com/mesosphere/oinker
 [8]: ../administration/sshcluster/
 [9]: ../administration/service-discovery/