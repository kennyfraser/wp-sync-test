---
ID: 3361
post_title: Using the DCOS Installer
author: Joel Hamill
post_date: 2016-02-16 15:46:35
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/dcos-installer/
published: true
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
header:
  - "1"
page_header_0_show_page_header:
  - "0"
page_header_0_size:
  - default
page_header_0_fill_screen:
  - "0"
page_header_0_background:
  - transparent
page_header_0_show_background_image:
  - "0"
page_header_0_show_background_video:
  - "0"
page_header_0_headline:
  - ""
page_header_0_headline_size:
  - default
page_header_0_description:
  - ""
page_header_0_description_size:
  - default
page_header_0_show_image:
  - "0"
page_header_0_content_alignment:
  - center
page_header_0_content_style:
  - dark
page_header_0_actions:
  - "0"
page_header_0_show_actions_footnote:
  - "0"
page_header_0_show_video:
  - "0"
page_header:
  - "1"
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "0"
---
You can quickly get up and running with DCOS Enterprise Edition by using the DCOS installer. The DCOS installer provides a guided installation using the most common and recommended defaults.

# Step 1: Bootstrap node prerequisites

Before installing DCOS, you must prepare your bootstrap node that will be used to run the DCOS installation commands. A bootstrap node is any physical, virtual, or cloud machine. It must have IP-to-IP connectivity from the bootstrap node to all nodes in your cluster environment. Your bootstrap node must not be a part of your cluster.

1.  **Docker** Docker version 1.9 or greater must be installed on your bootstrap and cluster nodes. You must run Docker commands as the root user (`sudo`). For more information, see [Docker installation][1].
    
    *   You can install Docker by using this command:
        
            $ sudo curl -sSL https://get.docker.com |  sudo sh
            
        
        If you are using CentOS7 and behind a firewall, you can install Docker by using this command:
        
            $ sudo yum install -y docker
            
    
    *   You must enable the Docker service:
        
            $ sudo systemctl enable docker.service
            
    
    *   If you are using Docker as a non-root user, you must add your user to the "docker" group:
        
            $ sudo usermod -aG docker <user>
            
    
    *   You can test that your Docker build is properly installed with these commands:
        
            $ sudo service docker start 
            $ sudo docker ps
            
    
    *   If you are using RHEL7, Docker must be installed by using a subscription channel. For more information, see <a href="https://access.redhat.com/articles/881893" target="_blank">Docker Formatted Container Images on Red Hat Systems</a>.
    
    *   If you are using Docker Containerizer, you must have network access to a public Docker repository from the agent nodes or to an internal Docker registry.

2.  **Zookeeper for shared storage** Shared storage is required by DCOS during installation and runtime for bootstrapping and managing the internal DCOS Zookeeper cluster. The Zookeeper for shared storage instance is used only for bootstrapping the DCOS Exhibitor instance.
    
    Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation. This Zookeeper instance should be separate from your cluster. Consider using a separate directory path for the DCOS cluster so that it does not interfere with other services that use the Zookeeper instance.
    
    Temporary outages while the cluster is running are acceptable, but shared storage should generally be up and running to support replacing failed masters.
    
    To start a Zookeeper instance using Docker, run this command:
    
        $ sudo docker run -d -p 2181:2181 -p 2888:2888 -p 3888:3888 --name=dcos_int_zk jplock/zookeeper
        
    
    **Tip:** If you've run the `usermod` command in the previous step, you may have to log out and then back in to your installer host machine before starting Zookeeper.

# Step 2: Cluster Prerequisites

Before installing DCOS you must prepare your cluster environment.

1.  **Linux distribution** A supported Linux distribution must be installed on your cluster:
    
    *   Enterprise Linux 7 (RedHat, CentOS)

2.  **Nodes Hardware Requirements**
    
    *   3 Mesos master nodes (2 Cores, 16 GB RAM, 60 GB HDD) <!-- A cluster of 8 or more machines with a supported Linux distribution. The recommended capacity for each node is 16 GB RAM/2 Cores and 16 GB disk space. The minimum size of a node is a machine with 10 GB of disk space and 1 GB of RAM. -->
    
    *   6 or more Mesos agent nodes (4 Cores, 32 GB RAM, 120 GB HDD)
    
    *   A /var directory with 10GB or more of free space. This is used by the sandbox for both Docker and [Mesos Containerizer][1].
    
    *   1 Workstation node (2 Cores, 16 GB RAM, 60 GB HDD)
        
        *   Python, pip, and virtualenv must be installed on the workstation node for the DCOS [CLI][2]. pip must be configured to pull packages from PyPI or your private PyPI, if applicable.
        *   A High-availability (HA) load balancer, such as HAProxy to balance the following TCP ports to all master nodes: 80, 443, 8080, 8181, 2181, 5050
    *   Network Time Protocol (NTP) for clock synchronization must be configured and enabled on all nodes.
    *   All of the nodes in your cluster must be able to send and receive traffic to each other.
    *   IPv6 must be disabled for all nodes. For more information see <a href="https://wiki.centos.org/FAQ/CentOS7#head-8984faf811faccca74c7bcdd74de7467f2fcd8ee" target="_blank">How do I disable IPv6</a>.

3.  **Port configuration**
    
    *   ICMP must be enabled between the master and the agent nodes.
    *   TCP and UDP enabled port 53 for DNS.
    *   Network Access to a public Docker repository from the agent nodes or to an internal Docker registry.
    *   These ports must be open for communication to the master nodes. <style type="text/css">
          .tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-e3zv{font-weight:bold}
.tg .tg-yw4l{vertical-align:top}
        </style>
        
        <table class="table">
          <tr>
            <th class="tg-e3zv">
              TCP Port
            </th>
            
            <th class="tg-e3zv">
              Description
            </th>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              2181
            </td>
            
            <td class="tg-yw4l">
              Zookeeper, see the <a href="http://zookeeper.apache.org/doc/r3.1.2/zookeeperAdmin.html#sc_zkCommands" target="_blank">ZK Admin Guide</a>
            </td>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              2888
            </td>
            
            <td class="tg-yw4l">
              Exhibitor, see the <a href="https://github.com/Netflix/exhibitor/wiki/REST-Introduction" target="_blank">Exhibitor REST Documentation</a>
            </td>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              3888
            </td>
            
            <td class="tg-yw4l">
              Exhibitor, see the <a href="https://github.com/Netflix/exhibitor/wiki/REST-Introduction" target="_blank">Exhibitor REST Documentation</a>
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              5050
            </td>
            
            <td class="tg-031e">
              Mesos master nodes
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              5051
            </td>
            
            <td class="tg-031e">
              Mesos agent nodes
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              8080
            </td>
            
            <td class="tg-031e">
              Marathon
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              8123
            </td>
            
            <td class="tg-031e">
              Mesos-DNS API
            </td>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              8181
            </td>
            
            <td class="tg-yw4l">
              Exhibitor, see the <a href="https://github.com/Netflix/exhibitor/wiki/REST-Introduction" target="_blank">Exhibitor REST Documentation</a>
            </td>
          </tr>
        </table>
    
    *   These ports must be open for communication to the agent nodes from the master nodes: <style type="text/css">
          .tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-e3zv{font-weight:bold}
.tg .tg-yw4l{vertical-align:top}
        </style>
        
        <table class="table">
          <tr>
            <th class="tg-e3zv">
              TCP Port
            </th>
            
            <th class="tg-e3zv">
              Description
            </th>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              5051
            </td>
            
            <td class="tg-yw4l">
              Mesos agent nodes
            </td>
          </tr>
        </table>
    
    *   All DCOS cluster node hostnames (FQDN and short hostnames) must be resolvable in DNS, both forward and reverse lookups must succeed.

# Step 3: Installation

**Prerequisite:** If your SSH key has a passphrase, you must decrypt your SSH key before installing DCOS. For information on how to decrypt your SSH key, see [these instructions][1].

1.  Download and save the DCOS setup file, `dcos_generate_config.ee.sh`, to the `dcos` directory on your bootstrap node. This file is used to create your customized DCOS build file.
    
    **Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

2.  From your terminal, start the DCOS installer with this command.
    
        $ sudo bash dcos_generate_config.ee.sh --web
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        16:36:09 dcos_installer.action_lib.prettyprint:: ====> Starting DCOS installer in web mode
        16:36:09 root:: Starting server ('0.0.0.0', 9000)
        
    
    You can add the verbose (`-v`) flag to see the debug output:
    
        $ sudo bash dcos_generate_config.ee.sh --web -v
        

3.  Launch the DCOS web installer in your browser at: `http://<public-ip>:9000`.

4.  Click **Begin Installation**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin.png" rel="attachment wp-att-3190"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin-800x510.png" alt="ui-installer-begin" width="800" height="510" class="alignnone size-large wp-image-3190" /></a>

5.  Specify your Deployment and DCOS Environment settings:
    
    ### Deployment Settings
    
    **Master IP Address List**
    :   Specify a comma-separated list of your static master IP addresses.
    
    **Agent IP Address List**
    :   Specify a comma-separated list of your static agent IP addresses.
    
    **Public IP Address**
    :   Specify the IP address to a publicly accessible proxy to one of your master nodes. If you either don't have a proxy or you already have access to the network where you are deploying this cluster, you can use one of the master IP's that you specified in the master list. This proxy IP address is used to access the DCOS web interface after the deployment succeeds.
    
    **SSH Username**
    :   Specify the SSH username, for example `centos`. SSH Listening Port
    
    **SSH Listening Port**
    :   Specify the port to SSH to, for example `22`. SSH Key
    
    **SSH Key**
    :   Specify the SSH key with access to your master IPs.
    
    ### DCOS Environment Settings
    
    **Username**
    :   Specify the administrator username. This username is required for using DCOS.
    
    **Password**
    :   Specify the administrator password. This password is required for using DCOS.
    
    **Bootstrapping Zookeeper IP Address(es)**
    :   Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it’s configuration.
    
    **Bootstrapping Zookeeper Port**
    :   Specify the Zookeeper port. For example, `2181`.
    
    **Upstream DNS Servers**
    :   Specify the DNS servers, which can be on your private network or the public internet. Caution: If you set this parameter incorrectly you will have to reinstall DCOS. For more information about service discovery, see this [documentation][2].
    
    **IP Detect Script**
    :   Specify an IP detect script to broadcast the IP address of each node across the cluster. For more information, see the [documentation][3].

6.  Click **Run Pre-Flight**. The preflight script validates that your cluster is installable. This step can take up to 15 minutes to complete. If errors any errors are found, fix and then click **Retry**.
    
    **Important:** If you exit your GUI installation before launching DCOS, you must do this before reinstalling:
    
    *   SSH to each node in your cluster and run `rm -rf /opt/mesosphere`.
    *   SSH to your bootstrap master node and run `rm -rf /var/lib/zookeeper`
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" rel="attachment wp-att-3197"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" alt="ui-installer-pre-flight1" width="626" height="405" class="alignnone size-full wp-image-3197" /></a>

7.  Click **Deploy** to install DCOS on your cluster. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" rel="attachment wp-att-3195"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" alt="ui-installer-deploy1" width="628" height="406" class="alignnone size-full wp-image-3195" /></a>

8.  Click **Run Post-Flight**. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" rel="attachment wp-att-3196"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" alt="ui-installer-post-flight1" width="623" height="366" class="alignnone size-full wp-image-3196" /></a>
    
    **Tip:** You can click **Download Logs** to view your logs locally.

9.  Click **Log In To DCOS**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a>

10. Enter your administrator username and password.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a>
    
    You are done!
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a>

### Next Steps

Now you can [assign user roles][4].

 [1]: https://techjourney.net/how-to-decrypt-an-enrypted-ssl-rsa-private-key-pem-key/
 [2]: ../installing-enterprise-edition-1-6/#scrollNav-3
 [3]: ../configuration-parameters-1-6/
 [4]: ../security-and-authentication/managing-authorization/