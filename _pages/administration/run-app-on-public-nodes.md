---
ID: 535
post_title: Running Your App on Public Nodes
author: Joel Hamill
post_date: 2016-03-08 14:03:47
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/administration/running-your-app-on-public-nodes/
published: true
import_src:
  - >
    mesosphere-docs/overview/load-balancing.md
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
  - 56df3def5b3dc
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "11"
---
The native Marathon instance is configured to receive offers from the public agent nodes (`--mesos_role="slave_public"`). Marathon apps are configured to use unreserved resources and launch on private agent nodes (`--default_accepted_resource_roles="*"`).

*   To run a Marathon app on public agent nodes, you must set the `acceptedResourceRoles` property to:
    
    "acceptedResourceRoles":["slave_public"]

*   To run a Marathon app on any agent node type, you must set the `acceptedResourceRoles` property to:
    
    "acceptedResourceRoles":["slave_public", "*"]

For a comprehensive example of deploying an app in the public zone to route HTTP requests, see [Deploying a Containerized App on a Public Node][1].

 [1]: /tutorials/deploywebapp/