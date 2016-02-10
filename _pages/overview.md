---
ID: 848
post_title: Overview
author: Joel Hamill
post_date: 2015-12-11 10:27:18
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheon.io/getting-started/overview/
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
  - "1"
page_header_0_size:
  - default
page_header_0_fill_screen:
  - "1"
page_header_0_background:
  - transparent
page_header_0_show_background_image:
  - "1"
page_header_0_show_background_video:
  - "1"
page_header_0_headline:
  - '[""]'
page_header_0_headline_size:
  - default
page_header_0_description:
  - ""
page_header_0_description_size:
  - default
page_header_0_show_image:
  - "1"
page_header_0_content_alignment:
  - center
page_header_0_content_style:
  - dark
page_header_0_actions:
  - "1"
page_header_0_show_actions_footnote:
  - "1"
page_header_0_show_video:
  - "1"
page_header:
  - "1"
page_options_topic_page:
  - 'a:1:{i:0;s:0:"";}'
page_options_require_authentication:
  - 'a:1:{i:0;s:1:"1";}'
hide_from_navigation:
  - "1"
hide_from_related:
  - "1"
page_header_0_center_content_vertically:
  - "1"
page_header_0_fill_screen_size_limits:
  - "0"
page_header_0_background_image:
  - ""
page_header_0_background_image_position_horizontal:
  - center
page_header_0_background_image_position_vertical:
  - center
page_header_0_background_video_webm:
  - ""
page_header_0_background_video_mp4:
  - ""
page_header_0_background_video_swf:
  - ""
page_header_0_background_video_ogg:
  - ""
page_header_0_image:
  - ""
page_header_0_actions_0_label:
  - ""
page_header_0_actions_0_trigger:
  - link
page_header_0_actions_0_link_url:
  - ""
page_header_0_actions_0_open_link_in_new_tab:
  - "0"
page_header_0_actions_0_style:
  - default
page_header_0_actions_0_invert_style:
  - "0"
page_header_0_actions_0_appearance:
  - fill
page_header_0_actions_footnote:
  - ""
page_header_0_video_source:
  - youtube
page_header_0_video_url:
  - ""
page_options_show_link_unauthenticated:
  - 'a:1:{i:0;s:1:"1";}'
---
The Mesosphere Datacenter Operating System (DCOS) spans all of the machines in your datacenter or cloud and treats them as a single, shared set of resources. DCOS provides a highly elastic and highly scalable way to build, deploy and manage modern applications using containers, microservices and big data systems.

<!-- DCOS is available in a Community Edition on supported cloud providers and a commercial Enterprise Edition that can be hosted on cloud providers, on-premise, or in a hybrid cloud configuration. -->

<a href="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/dcos-overview.jpg" rel="attachment wp-att-2170"><img src="https://dev-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/dcos-overview-800x346.jpg" alt="dcos-overview" width="800" height="346" class="alignnone size-large wp-image-2170" /></a>

The DCOS is built on top of open-source components.

### Mesos

<a href="http://mesos.apache.org/" target="_blank">Mesos</a> is the distributed systems kernel that is at the core of DCOS. Mesos provides efficient resource isolation and sharing across distributed applications or frameworks. Mesos pools your infrastructure, automatically allocating resources and scheduling tasks based on demands and policy.

Mesosphere is a significant contributor to the Apache Mesos project and is the primary organization writing open-source services for Mesos, such as Marathon and Chronos.

### Marathon

<a href="http://mesosphere.github.io/marathon/" target="_blank">Marathon</a> is the DCOS container-orchestration engine that provides cluster-wide init and control for microservices in cgroups or Docker containers. Marathon is an open-source Mesos framework created by Mesosphere that launches long-running applications for DCOS. It starts and manages applications and services inside of Mesos, automatically healing failures.

### Mesos-DNS

<a href="https://github.com/mesosphere/mesos-dns" target="_blank">Mesos-DNS</a> provides service discovery in DCOS so that applications and services can find each other. Mesos-DNS allows applications and services that are running on Mesos to find each other by using the domain name system (DNS), similar to how services discover each other throughout the Internet.

### ZooKeeper

ZooKeeper is a high-performance coordination service that manages the DCOS services. To coordinate and manage the ZooKeeper system, we've integrated <a href="https://github.com/Netflix/exhibitor" target="_blank">Exhibitor for ZooKeeper</a>.

### Admin Router

The <a href="https://github.com/mesosphere/adminrouter-public" target="_blank">admin router</a> is an open-source Nginx configuration created by Mesosphere that provides central authentication and proxy to DCOS services within the cluster.