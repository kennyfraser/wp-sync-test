---
ID: 1322
post_title: HDFS Capacity
author: Joel Hamill
post_date: 2016-03-08 14:04:13
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/hdfs-capacity/
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
  - ""
hide_from_related:
  - ""
UID:
  - 56df3dedd5ae1
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "100"
---
By default, the capacity of HDFS is 3.3 GB (10GB / 3) with 2 datanodes (a total of 5 agent nodes). If you add nodes to your cluster, you will have greater storage capacity since storage capacity = (number of datanodes * 5 GB) divided by 3. For example, with 5 datanodes (a total of 8 agent nodes), your capacity will be 8.3 GB (25GB / 3).