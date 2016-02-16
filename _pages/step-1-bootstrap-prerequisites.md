---
ID: 3143
post_title: 'Step 1: Bootstrap node prerequisites'
author: Joel Hamill
post_date: 2016-02-06 07:38:26
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/step-1-bootstrap-prerequisites/
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
Before installing DCOS, you must prepare your bootstrap node that will be used to run the DCOS installation commands. A bootstrap node is any physical, virtual, or cloud machine. It must have IP-to-IP connectivity from the bootstrap node to all nodes in your cluster environment. Your bootstrap node must not be a part of your cluster.

1.  **Docker** Docker version 1.9 or greater must be installed on your bootstrap and cluster nodes. You must run Docker commands as the root user (`sudo`). For more information, see [Docker installation][1].
    
    *   You can install Docker by using this command:
        
            $ curl -sSL https://get.docker.com |  sudo sh
            
        
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

## Next step

[Step 2: Cluster prerequisites][2]

 [1]: http://docs.docker.com/engine/installation/
 [2]: ../step-2-cluster-prerequisites/