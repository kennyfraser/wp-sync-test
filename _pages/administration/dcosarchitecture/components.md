---
UID: 56df3ded589b0
post_title: Components
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The Mesosphere DCOS is comprised of Mesos master and agent nodes, a native DCOS Marathon instance, Mesos-DNS for service discovery, Admin Router for central authentication and proxy to DCOS services, and Zookeeper to coordinate and manage the installed DCOS services.</p>

<p><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/Enterprise-Architecture-Diagram.png" alt="Enterprise Architecture Diagram" width="2468" height="1420" class="alignnone size-full wp-image-834" /></p>

<h1>DCOS master nodes</h1>

<p>The DCOS master node includes the Mesos master process, Mesos-DNS, DCOS Marathon, ZooKeeper, and the Admin Router.</p>

<p>When a leading master fails due to a crash or goes offline for an upgrade, a standby master automatically becomes the leader without causing any disruption to running services. Leader election is performed by using ZooKeeper.</p>

<h3>Mesos master process</h3>

<p>The <code>mesos-master</code> process orchestrates tasks that are run on agent nodes. The Mesos master process receives resource offers from agents and distributes those resources to registered DCOS services, such as Marathon or Chronos. For more information, see the <a href="https://open.mesosphere.com/reference/mesos-master/">Mesos Master Configuration</a> documentation.</p>

<h3>Mesos DNS</h3>

<p>Mesos DNS provides service discovery within the cluster. Mesos DNS allows applications and services that are running on Mesos to find each other by using the domain name system (DNS), similar to how services discover each other throughout the Internet.</p>

<h3>DCOS Marathon</h3>

<p>The native Marathon instance that is the “init system” for DCOS. It starts and monitors DCOS applications and services.</p>

<h3>ZooKeeper</h3>

<p>ZooKeeper is a high-performance coordination service that manages the DCOS services. Exhibitor automatically configures your Zookeeper installation on the master nodes during installation.</p>

<h3>Admin router</h3>

<p>The <a href="https://open.mesosphere.com/reference/mesos-slave/">admin router</a> is an open source Nginx configuration created by Mesosphere that provides central authentication and proxy to DCOS services within the cluster.</p>

<h1>DCOS agent nodes</h1>

<p>The DCOS private agent nodes run the deployed apps and services. The optional DCOS public agent nodes can provide public access to DCOS applications that are run on a public agent node.</p>

<h3>Mesos agent process</h3>

<p>The <code>mesos-slave</code> process offers up available resources on a node to the Mesos masters. The agent node also accepts schedule requests from the master and invokes an executor to launch a task. For more information, see the <a href="https://open.mesosphere.com/reference/mesos-slave/">Mesos Slave Configuration</a> documentation.</p>

<h3>Mesos containerizer</h3>

<p>The Mesos Containerizer provides lightweight containerization and resource isolation of executors using Linux-specific functionality such as cgroups and namespaces. For more information, see the <a href="http://mesos.apache.org/documentation/latest/containerizer/">Mesos Containerizer</a> documentation.</p>

<h3>Docker container</h3>

<p>The Mesos Docker Containerizer provides support for launching tasks that contain Docker images. For more information, see the <a href="http://mesos.apache.org/documentation/latest/docker-containerizer/">Docker Containerizer</a> documentation.</p>