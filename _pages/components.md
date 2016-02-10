---
ID: 2052
post_title: Components
author: Joel Hamill
post_date: 2016-01-04 13:30:05
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/administration/dcosarchitecture/components/
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
page_options_topic_page:
  - ""
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "0"
---
The Mesosphere DCOS is comprised of Mesos master and agent nodes, a native DCOS Marathon instance, Mesos-DNS for service discovery, Admin Router for central authentication and proxy to DCOS services, and Zookeeper to coordinate and manage the installed DCOS services.

<img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/Enterprise-Architecture-Diagram.png" alt="Enterprise Architecture Diagram" width="2468" height="1420" class="alignnone size-full wp-image-834" />

# DCOS master nodes

The DCOS master node includes the Mesos master process, Mesos-DNS, DCOS Marathon, ZooKeeper, and the Admin Router.

When a leading master fails due to a crash or goes offline for an upgrade, a standby master automatically becomes the leader without causing any disruption to running services. Leader election is performed by using ZooKeeper.

### Mesos master process

The `mesos-master` process orchestrates tasks that are run on agent nodes. The Mesos master process receives resource offers from agents and distributes those resources to registered DCOS services, such as Marathon or Chronos. For more information, see the [Mesos Master Configuration][1] documentation.

### Mesos-DNS

Mesos-DNS provides service discovery within the cluster. Mesos-DNS allows applications and services that are running on Mesos to find each other by using the domain name system (DNS), similar to how services discover each other throughout the Internet.

### DCOS Marathon

The native Marathon instance that is the “init system” for DCOS. It starts and monitors DCOS applications and services.

### ZooKeeper

ZooKeeper is a high-performance coordination service that manages the DCOS services. Exhibitor automatically configures your Zookeeper installation on the master nodes during installation.

### Admin router

The [admin router][2] is an open source Nginx configuration created by Mesosphere that provides central authentication and proxy to DCOS services within the cluster.

# DCOS agent nodes

The DCOS private agent nodes run the deployed apps and services. The optional DCOS public agent nodes can provide public access to DCOS applications that are run on a public agent node.

### Mesos agent process

The `mesos-slave` process offers up available resources on a node to the Mesos masters. The agent node also accepts schedule requests from the master and invokes an executor to launch a task. For more information, see the [Mesos Slave Configuration][2] documentation.

### Mesos containerizer

The Mesos Containerizer provides lightweight containerization and resource isolation of executors using Linux-specific functionality such as control cgroups and namespaces. For more information, see the [Mesos Containerizer][3] documentation.

### Docker container

The Mesos Docker Containizer provides support for launching tasks that contain Docker images. For more information, see the [Docker Containerizer][4] documentation.

 [1]: https://open.mesosphere.com/reference/mesos-master/
 [2]: https://open.mesosphere.com/reference/mesos-slave/
 [3]: http://mesos.apache.org/documentation/latest/containerizer/
 [4]: http://mesos.apache.org/documentation/latest/docker-containerizer/