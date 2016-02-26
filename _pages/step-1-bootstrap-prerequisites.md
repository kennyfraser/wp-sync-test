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
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: true
page_options_show_link_unauthenticated: false
---
Before installing DCOS, you must prepare your bootstrap node that will be used to run the DCOS installation commands. A bootstrap node is any physical, virtual, or cloud machine. It must have IP-to-IP connectivity from the bootstrap node to all nodes in your cluster environment. Your bootstrap node must not be a part of your cluster.

[docker-prereq]

## ZooKeeper for shared storage

Shared storage is required by DCOS during installation and runtime for bootstrapping and managing the internal DCOS ZooKeeper cluster. The ZooKeeper for shared storage instance should only be used for bootstrapping the DCOS Exhibitor instance. Do not use this bootstrap ZooKeeper instance in production. Multiple ZooKeeper instances are recommended for failover in production environments.

Exhibitor automatically configures your ZooKeeper installation on the master nodes during your DCOS installation. This ZooKeeper instance should be separate from your cluster. Consider using a separate directory path for the DCOS cluster so that it does not interfere with other services that use the ZooKeeper instance.

Temporary outages while the cluster is running are acceptable, but shared storage should generally be up and running to support replacing failed masters.

To start a ZooKeeper instance using Docker, run this command:

        $ sudo docker run -d -p 2181:2181 -p 2888:2888 -p 3888:3888 --name=dcos_int_zk jplock/zookeeper
    

**Tip:** If you've run the `usermod` Docker command in the previous step, you might have to log out and then back in to your bootstrap node before starting Zookeeper.

## DCOS setup file

Download and save the DCOS setup file, `dcos_generate_config.sh`, to the `dcos` directory on your bootstrap node. This file is used to create your customized DCOS build file.

**Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

## Next step

[Step 2: Cluster prerequisites][1]

 [1]: ../step-2-cluster-prerequisites/