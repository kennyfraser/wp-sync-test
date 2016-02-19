---
ID: 3143
post_title: 'Step 1: Bootstrap node prerequisites'
author: Joel Hamill
post_date: 2016-02-06 07:38:26
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/manual-installation/step-1-bootstrap-prerequisites/
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

## Zookeeper for shared storage

Shared storage is required by DCOS during installation and runtime for bootstrapping and managing the internal DCOS Zookeeper cluster. The Zookeeper for shared storage instance should only be used for bootstrapping the DCOS Exhibitor instance. Do not use this bootstrap Zookeeper instance in production.

Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation. This Zookeeper instance should be separate from your cluster. Consider using a separate directory path for the DCOS cluster so that it does not interfere with other services that use the Zookeeper instance.

Temporary outages while the cluster is running are acceptable, but shared storage should generally be up and running to support replacing failed masters.

To start a Zookeeper instance using Docker, run this command:

        $ sudo docker run -d -p 2181:2181 -p 2888:2888 -p 3888:3888 --name=dcos_int_zk jplock/zookeeper
    

**Tip:** If you've run the `usermod` Docker command in the previous step, you might have to log out and then back in to your boostrap node before starting Zookeeper.

## DCOS setup file

Download and save the DCOS setup file, `dcos_generate_config.sh`, to the `dcos` directory on your bootstrap node. This file is used to create your customized DCOS build file.

**Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

## Next step

[Step 2: Cluster prerequisites][1]

 [1]: ../step-2-cluster-prerequisites/