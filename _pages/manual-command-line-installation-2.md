---
ID: 561
post_title: Manual command line installation
post_date: 2016-02-24 14:49:27
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/manual-command-line-installation-2/
published: true
post_parent: 2957
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
With the manual command line installation method, you package the DCOS distribution yourself and connect to every node manually to run the DCOS installation commands. This installation method is recommended if you don't have SSH access to your cluster or if you want to integrate with an existing system.

The manual command line installation method requires:

*   The bootstrap node must be network accessible from the cluster nodes 
*   The bootstrap node must have the HTTP(S) ports open from the cluster nodes