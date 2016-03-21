---
UID: 56effc92adf28
post_title: Monitoring System Health
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
# Overview

You can monitor the health of your entire cluster from the DCOS system health page. The system health page provides the health status of all DCOS system components that are running in systemd.

You can click on each of the components to see a detailed view, including individual node health status and roles.

You can sort by system health. Possible health states are Unhealthy and Healthy:

**Healthy** All cluster nodes are healthy.

**Unhealthy** One or more nodes have issues.

# System Health Implementation

## How Health is Calculated

Health status follows a traditional Nagios-like implementation. Currently, health is tracked on 4 levels from 0-3:

**0**: OK **1**: Critical **2**: Warning **3**: Unknown

It's important to note that systemd units have only binary health of 0 or 1. If the unit is in any state other than "active" or "inactive" or is not loaded, it will be considered a 1, and unhealthy.

## Troubleshooting Guidelines

### SystemD unit state

The system health API assumes a systemd unit is healthy if it is anything but "active" or "inactive" state and is also loaded.

### Misinterpreting System Health by Unit

You're able to sort system health by systemd unit. However, this search can bring up misleading information as the service itself can be healthy but the node on which it runs is not. This manifests itself as a service showing "healthy" but nodes associated with that service as "unhealthy". Some people find this behavior confusing.

## Missing Cluster Hosts

The system health API relies on MesosDNS to know about all the cluster hosts. It finds these hosts by combining a query from `mesos.master` A records as well as `leader.mesos:5050/slaves` to get the complete list of hosts in the cluster.

This system has a known bug where a slave (agent) will not show up in the list returned from `leader.mesos:5050/slaves` if the Mesos slave service is not healthy. This means the system health API will not show this host.

If you experience this behavior it's most likely your Mesos slave service on the missing host is unhealthy.