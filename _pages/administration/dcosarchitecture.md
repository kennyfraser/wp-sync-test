---
ID: 531
post_title: Architecture
author: Joel Hamill
post_date: 2016-03-08 14:03:41
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/administration/architecture/
published: true
import_src:
  - >
    mesosphere-docs/overview/dcosarchitecture.md
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
page_options_show_link_unauthenticated:
  - "1"
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3def66bbf
menu_order:
  - "2"
---
The DCOS is based on <a href="http://mesos.apache.org/" target="_blank">Mesos</a> and includes a distributed systems kernel with enterprise-grade security. It also includes a set of core system services, such as a native [Marathon][1] instance to manage processes and installable services, and [Mesos-DNS][2] for service discovery. The DCOS provides a web interface and a command-line interface (CLI) to manage the deployment and scale of your applications.

 [1]: ../../manage-service/marathon/
 [2]: https://docs.mesosphere.com/administration/service-discovery/overview/