---
ID: 3146
post_title: 'Step 2: Cluster prerequisites'
author: Joel Hamill
post_date: 2016-02-06 07:44:15
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/step-2-cluster-prerequisites/
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
  - "1"
---
Before installing DCOS you must prepare your cluster environment.

1.  **Linux distribution** A supported Linux distribution must be installed on your cluster:
    
    *   Enterprise Linux 7 (RedHat, CentOS)
    *   CoreOS

2.  **Nodes**
    
    *   3 Mesos master nodes (2 Cores, 16 GB RAM, 60 GB HDD) <!-- A cluster of 8 or more machines with a supported Linux distribution. The recommended capacity for each node is 16 GB RAM/2 Cores and 16 GB disk space. The minimum size of a node is a machine with 10 GB of disk space and 1 GB of RAM. -->
    
    *   6 or more Mesos agent nodes (4 Cores, 32 GB RAM, 120 GB HDD)
    
    *   A /var directory with 10GB or more of free space. This is used by the sandbox for both Docker and [Mesos Containerizer][1].
    
    *   1 Workstation node (2 Cores, 16 GB RAM, 60 GB HDD)
        
        *   Python, pip, and virtualenv must be installed on the workstation node for the DCOS [CLI][2]. pip must be configured to pull packages from PyPI or your private PyPI, if applicable.
        *   A High-availability (HA) load balancer, such as HAProxy to balance the following TCP ports to all master nodes: 80, 443, 8080, 8181, 2181, 5050
    *   Network Time Protocol (NTP) for clock synchronization must be configured and enabled on all nodes.
    *   All of the nodes in your cluster must be able to send and receive traffic to each other.
    *   IPv6 must be disabled for all nodes. For more information see <a href="https://wiki.centos.org/FAQ/CentOS7#head-8984faf811faccca74c7bcdd74de7467f2fcd8ee" target="_blank">How do I disable IPv6</a>.

3.  **System updates** Make sure you've updated your system to the latest version.
    
    *   On CentOS7 and RHEL7, you can update your systems with this command:
        
            $ sudo yum upgrade -y
            

4.  **Data compression** You must have the <a href="http://www.info-zip.org/UnZip.html" target="_blank">UnZip</a>, <a href="https://www.gnu.org/software/tar/" target="_blank">GNU tar</a>, and <a href="http://tukaani.org/xz/" target="_blank">XZ Utils</a> data compression utilities installed on your cluster nodes.
    
    To install these utilities on CentOS7 and RHEL7:
    
        $ sudo yum install -y tar xz unzip curl
        

5.  **Docker** Docker version 1.9 or greater must be installed on your workstation and cluster nodes. You must run Docker commands as the root user (`sudo`). For more information, see [Docker installation][3].
    
    *   You can install Docker by using this command:
        
            $ curl -sSL https://get.docker.com | sudo sh
            
        
        If you are using CentOS7 and behind a firewall, you can install Docker by using this command:
        
            $ sudo yum install -y docker
            
    
    *   You must enable the Docker service:
        
            $ sudo systemctl enable docker.service
            
    
    *   If you are using Docker as a non-root user, you must add your user to the "docker" group:
        
            $ sudo usermod -aG docker <user>
            
    
    *   You can test that your Docker build is properly installed with these commands:
        
            $ sudo service docker start 
            $ sudo docker ps
            
    
    *   If you are using Docker Containerizer, you must have network access to a public Docker repository from the agent nodes or to an internal Docker registry.
    
    *   If you are using RHEL7, Docker must be installed by using a subscription channel. For more information, see <a href="https://access.redhat.com/articles/881893" target="_blank">Docker Formatted Container Images on Red Hat Systems</a>.

6.  **Port configuration**
    
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

7.  **Set cluster permissions**
    
    In this step you prepare your cluster for DCOS installation.
    
    1.  On each of your master and agent nodes, disable SELinux or set it to permissive mode. This enables DCOS services and Docker to function properly.
        
            $ sudo sed -i s/SELINUX=enforcing/SELINUX=permissive/g /etc/selinux/config
            
        
        **Tip:** You can verify the status of SELinux with the `sestatus`command.
    
    2.  Add `nogroup` to each of your Mesos masters and agents.
        
            $ sudo groupadd nogroup
            
    
    3.  Disable IPV6 by using this command:
        
            $ sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
            $ sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
            
    
    4.  Reboot your cluster for the changes to take affect:
        
            $ sudo reboot

 [1]: http://mesos.apache.org/documentation/latest/containerizer/
 [2]: ../administration/introcli/
 [3]: http://docs.docker.com/engine/installation/