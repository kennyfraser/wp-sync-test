---
ID: 95
post_title: Manual command line installation
post_date: 2016-02-22 18:13:26
post_excerpt: ""
layout: page
permalink: >
  http://local.gitsync.com/manual-command-line-installation/
published: true
post_parent: 0
menu_order: 0
page_options_require_authentication: true
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
With the manual command line installation method, you package the DCOS distribution yourself and connect to every node manually to run the DCOS installation commands. This installation method is recommended if you don't have SSH access to your cluster or if you want to integrate with an existing system.

The manual command line installation method requires:

*   The bootstrap node must be network accessible from the cluster nodes 
*   The bootstrap node must have the HTTP(S) ports open from the cluster nodes