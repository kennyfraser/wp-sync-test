---
ID: 531
post_title: Architecture
post_date: 2015-12-08 08:56:50
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheon.io/administration/dcosarchitecture/
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: true
hide_from_navigation: true
hide_from_related: true
---
The DCOS is based on <a href="http://mesos.apache.org/" target="_blank">Mesos</a> and includes a distributed systems kernel with enterprise-grade security. It also includes a set of core system services, such as a native [Marathon][1] instance to manage processes and installable services, and [Mesos-DNS][2] for service discovery. The DCOS provides a web interface and a command-line interface (CLI) to manage the deployment and scale of your applications.

 [1]: ../../manage-service/marathon/
 [2]: https://docs.mesosphere.com/administration/service-discovery/overview/