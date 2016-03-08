---
ID: 1322
post_title: HDFS Capacity
post_date: 2015-12-23 13:15:01
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/manage-service/hdfs/hdfs-capacity/
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
By default, the capacity of HDFS is 3.3 GB (10GB / 3) with 2 datanodes (a total of 5 agent nodes). If you add nodes to your cluster, you will have greater storage capacity since storage capacity = (number of datanodes * 5 GB) divided by 3. For example, with 5 datanodes (a total of 8 agent nodes), your capacity will be 8.3 GB (25GB / 3).