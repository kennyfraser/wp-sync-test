---
ID: 70
post_title: Architecture
post_date: 2016-02-26 15:32:00
post_excerpt: ""
layout: page
permalink: >
  http://local.gitsync.com/administration/architecture-2/
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The DCOS is based on <a href="http://mesos.apache.org/" target="_blank">Mesos</a> and includes a distributed systems kernel with enterprise-grade security. It also includes a set of core system services, such as a native [Marathon][1] instance to manage processes and installable services, and [Mesos-DNS][2] for service discovery. The DCOS provides a web interface and a command-line interface (CLI) to manage the deployment and scale of your applications.

 [1]: ../../manage-service/marathon/
 [2]: https://docs.mesosphere.com/administration/service-discovery/overview/