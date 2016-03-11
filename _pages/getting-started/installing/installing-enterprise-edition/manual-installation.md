---
ID: 3364
post_title: Manual command line installation
author: Joel Hamill
post_date: 2016-03-08 14:04:04
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/getting-started/installing/installing-enterprise-edition/manual-command-line-installation/
published: true
header:
  - "1"
page_header:
  - "1"
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
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
UID:
  - 56df3deb9aa4b
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "2"
---
With the manual command line installation method, you package the DCOS distribution yourself and connect to every node manually to run the DCOS installation commands. This installation method is recommended if you don't have SSH access to your cluster or if you want to integrate with an existing system.

The manual command line installation method requires:

*   The bootstrap node must be network accessible from the cluster nodes 
*   The bootstrap node must have the HTTP(S) ports open from the cluster nodes

[installing-enterprise-edition-hardware] [installing-enterprise-edition-software]