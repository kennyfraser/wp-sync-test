---
ID: 3364
post_title: Manual command line installation
author: Joel Hamill
post_date: 2016-02-16 15:44:37
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/manual-installation/
published: true
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: false
page_options_show_link_unauthenticated: false
---
With the manual command line installation method, you package the DCOS distribution yourself and connect to every node manually to run the DCOS installation commands. This installation method is recommended if you don't have SSH access to your cluster or if you want to integrate with an existing system.

The manual command line installation method requires:

*   The bootstrap node must be network accessible from the cluster nodes 
*   The bootstrap node must have the HTTP(S) ports open from the cluster nodes

[installing-enterprise-edition-hardware] [installing-enterprise-edition-software] [installing-enterprise-edition-ip-detect]