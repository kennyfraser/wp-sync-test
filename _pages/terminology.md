---
ID: 538
post_title: Terminology
author: Joel Hamill
post_date: 2015-12-08 08:57:04
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/administration/terminology/
published: true
import_src:
  - mesosphere-docs/overview/terminology.md
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
This page contains terms and definitions for the Mesosphere DCOS.

# Admin router

The admin router runs on the DCOS master servers to provide a proxy for the admin parts of the cluster.

# <a name="agent"></a>Agent node

A Mesos agent node runs a discrete Mesos task on behalf of a framework. It is a agent instance registered with the Mesos master. The synonym of agent node is worker or slave node.

# Cloud template

The [DCOS Community Edition (CE) cloud templates][1] are optimized to run Mesosphere DCOS. The templates are a JSON-formatted text files that describe the resources and properties.

# Containerizer

The [MesosContainerizer][2] provides lightweight containerization and resource isolation of executors using Linux-specific functionality such as control cgroups and namespaces.

# Datacenter operating system

A new class of operating system that spans all of the machines in a datacenter or cloud and organizes them to act as one big computer.

# DCOS

The abbreviated form of the Mesosphere Datacenter Operating System.

# DCOS Cluster

A group of Mesos master and agent nodes.

# DCOS service

DCOS services are Mesosphere-certified applications that are packaged and available from the public [GitHub package repositories][3]. Available DCOS services include Mesosphere-certified Mesos frameworks and other applications. A [Mesos framework][4] is the combination of a Mesos scheduler and an optional custom executor.

# Executor

A framework running on top of Mesos consists of two components: a scheduler that registers with the master to be offered resources, and an executor process that is launched on slave nodes to run the framework’s tasks. For more information about framework schedulers and executors, see the [App/Framework development guide][5].

# Exhibitor for Zookeeper

The DCOS uses ZooKeeper, a high-performance coordination service to manage the installed DCOS services. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.

# Framework

A Mesos framework is the combination of a Mesos scheduler and an optional custom executor. A framework receives resource offers describing CPU, RAM, etc., and allocates them for discrete tasks that can be launched on Mesos agent nodes. Mesosphere-certified Mesos frameworks, called DCOS services, are packaged and available from public [GitHub package repositories][3]. DCOS services include Mesosphere-certified Mesos frameworks and other applications.

# Master

A Mesos master aggregates resource offers from all [agent nodes][6] and provides them to registered frameworks. For more details about the Mesos master, read about <a href="http://open.mesosphere.com/reference/mesos-master/" target="_blank">Mesos Master Configuration</a>.

# Mesos DNS

[Mesos DNS][7] is an open source DCOS component that provides service discovery within the DCOS cluster. Mesos-DNS allows applications and services that are running on Mesos to find each other by using the domain name system (DNS), similar to how services discover each other throughout the Internet.

# Package repository

DCOS services are Mesosphere-certified applications that are packaged and available from the public DCOS package repositories that are hosted on GitHub.

# Offer

An offer represents available resources (e.g. cpu, disk, memory) which an agent offers to the master and the master hands to the registered frameworks in some order.

# Private agent node

The DCOS [private agent nodes][8] run the deployed apps and services.

# Public agent node

The DCOS [public agent nodes][8] provide public access to DCOS applications that are run on a public agent node.

# Slave

See [agent node][6].

# Task

A unit of work scheduled by a Mesos framework and executed on a Mesos agent. In Hadoop terminology, this is a "job". In MySQL terminology, this is a "query" or "statement". A task may simply be a Bash command or a Python script.

# Working directory

A Mesos master requires a directory on the local file system to write replica logs to.

# Zookeeper<a name="zoo"></a>

The DCOS uses ZooKeeper, a high-performance coordination service to manage the installed DCOS services. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.

 [1]: /tag/community/
 [2]: http://mesos.apache.org/documentation/latest/containerizer/
 [3]: https://github.com/mesosphere/universe
 [4]: http://mesos.apache.org/documentation/latest/frameworks/
 [5]: http://mesos.apache.org/documentation/latest/app-framework-development-guide/
 [6]: #agent
 [7]: https://github.com/mesosphere/mesos-dns
 [8]: /dcosarchitecture/components/#scrollNav-2