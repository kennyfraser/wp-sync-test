---
ID: 3156
post_title: 'Step 1: Workstation prerequisites'
author: kennyfraser
post_date: 2016-02-12 06:38:14
post_excerpt: ""
layout: page
permalink: >
  http://local.mesodocs.com/step-1-workstation-prerequisites-2/
published: true
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
Before installing DCOS you must prepare your computer or bootstrap node that is used to run the DCOS installation commands. Your computer or bootstrap node must not be a part of your cluster.

1.  **Docker** Docker version 1.9 or greater must be installed on your workstation and cluster nodes. You must run Docker commands as the root user (`sudo`). For more information, see [Docker installation][1].
    
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
    
        $ docker run -d -p 2181:2181 -p 2888:2888 -p 3888:3888 --name=dcos_int_zk jplock/zookeeper
        
    
    **Tip:** If you've run the `usermod` command in the previous step, you may have to log out and then back in to your installer host machine before starting Zookeeper.

## Next step

[Step 2: Cluster prerequisites][2]

 [1]: http://docs.docker.com/engine/installation/
 [2]: ../step-2-cluster-prerequisites/