---
ID: 101
post_title: 'Step 1: Bootstrap node prerequisites'
post_date: 2016-02-22 18:13:26
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/step-1-bootstrap-node-prerequisites/
published: true
post_parent: 0
menu_order: 0
page_options_require_authentication: true
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
Before installing DCOS, you must prepare your bootstrap node that will be used to run the DCOS installation commands. A bootstrap node is any physical, virtual, or cloud machine. It must have IP-to-IP connectivity from the bootstrap node to all nodes in your cluster environment. Your bootstrap node must not be a part of your cluster. testee

## Docker

Docker version 1.9 or greater must be installed on your bootstrap and cluster nodes. You must run Docker commands as the root user (`sudo`). For more information, see <a href="http://docs.docker.com/engine/installation/" target="_blank">Docker installation</a>.

Install Docker by using these commands your Linux distribution. CoreOS includes Docker natively.

*   **RHEL** Install Docker by using a subscription channel. For more information, see <a href="https://access.redhat.com/articles/881893" target="_blank">Docker Formatted Container Images on Red Hat Systems</a>. <!-- $ curl -sSL https://get.docker.com | sudo sh -->

*   **CentOS** Install Docker by using OverlayFS.
    
    1.  Add the Docker yum repo to your bootstrap node:
        
            $ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
            [dockerrepo]
            name=Docker Repository
            baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
            enabled=1
            gpgcheck=1
            gpgkey=https://yum.dockerproject.org/gpg
            EOF
            sudo yum -y update
            sudo mkdir -p /etc/systemd/system/docker.service.d
            sudo tee /etc/systemd/system/docker.service.d/override.conf <<- EOF
            [Service]
            ExecStart=
            ExecStart=/usr/bin/docker daemon --storage-driver=overlay -H fd://
            EOF
            
    
    2.  Install the Docker engine:
        
            $ sudo yum install -y docker-engine
            
    
    3.  Start the Docker daemon:
        
            $ sudo systemctl start docker
            
    
    4.  Enable the Docker service:
        
            $ sudo systemctl enable docker
            
    
    *   If you are using Docker as a non-root user, you must add your user to the "docker" group:
        
            $ sudo usermod -aG docker <user>
            
    
    *   You can test that your Docker build is properly installed with these commands:
        
            $ sudo service docker start 
            $ sudo docker ps
            

*   If you are using Docker Containerizer, you must have network access to a public Docker repository from the agent nodes or to an internal Docker registry.

## ZooKeeper for shared storage

Shared storage is required by DCOS during installation and runtime for bootstrapping and managing the internal DCOS ZooKeeper cluster. The ZooKeeper for shared storage instance should only be used for bootstrapping the DCOS Exhibitor instance. Do not use this bootstrap ZooKeeper instance in production. Multiple ZooKeeper instances are recommended for failover in production environments.

Exhibitor automatically configures your ZooKeeper installation on the master nodes during your DCOS installation. This ZooKeeper instance should be separate from your cluster. Consider using a separate directory path for the DCOS cluster so that it does not interfere with other services that use the ZooKeeper instance.

Temporary outages while the cluster is running are acceptable, but shared storage should generally be up and running to support replacing failed masters.

To start a ZooKeeper instance using Docker, run this command:

        $ sudo docker run -d -p 2181:2181 -p 2888:2888 -p 3888:3888 --name=dcos_int_zk jplock/zookeeper
    

**Tip:** If you've run the `usermod` Docker command in the previous step, you might have to log out and then back in to your boostrap node before starting Zookeeper.

## DCOS setup file

Download and save the DCOS setup file, `dcos_generate_config.sh`, to the `dcos` directory on your bootstrap node. This file is used to create your customized DCOS build file.

**Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

## Next step

[Step 2: Cluster prerequisites][1]

 [1]: ../step-2-cluster-prerequisites/