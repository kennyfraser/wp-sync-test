---
ID: 95
post_title: Manual command line installation
author: gitsync
post_date: 2016-02-22 18:13:26
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/manual-command-line-installation/
published: true
header_0_background:
  - 'a:1:{i:0;s:4:"fill";}'
header_0_background_fill_style:
  - 'a:1:{i:0;s:4:"dark";}'
header_0_logo_style:
  - 'a:1:{i:0;s:11:"color-light";}'
header_0_navigation_style:
  - 'a:1:{i:0;s:5:"light";}'
header:
  - 'a:1:{i:0;s:1:"1";}'
page_header_0_show_page_header:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_fill_screen:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_background:
  - 'a:1:{i:0;s:11:"transparent";}'
page_header_0_show_background_image:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_background_video:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_headline:
  - 'a:1:{i:0;s:0:"";}'
page_header_0_headline_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_description:
  - 'a:1:{i:0;s:0:"";}'
page_header_0_description_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_show_image:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_content_alignment:
  - 'a:1:{i:0;s:6:"center";}'
page_header_0_content_style:
  - 'a:1:{i:0;s:4:"dark";}'
page_header_0_actions:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_actions_footnote:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_video:
  - 'a:1:{i:0;s:1:"0";}'
page_header:
  - 'a:1:{i:0;s:1:"1";}'
page_options_require_authentication:
  - 'a:1:{i:0;s:0:"";}'
hide_from_navigation:
  - 'a:1:{i:0;s:1:"0";}'
hide_from_related:
  - 'a:1:{i:0;s:1:"0";}'
---
With the manual command line installation method, you package the DCOS distribution yourself and connect to every node manually to run the DCOS installation commands. This installation method is recommended if you don't have SSH access to your cluster or if you want to integrate with an existing system.

The manual command line installation method requires:

*   The bootstrap node must be network accessible from the cluster nodes 
*   The bootstrap node must have the HTTP(S) ports open from the cluster nodes