---
ID: 3230
post_title: Security and Authentication
author: Joel Hamill
post_date: 2016-02-09 17:01:52
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/security-and-authentication/
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
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "1"
---
You can enable authorization and authentication in your datacenter with DCOS Enterprise Edition. By default, DCOS provides role-based access control (RBAC) authentication managed through the DCOS web interface. Authorization is defined by access control lists (ACLs). The Admin Router enforces access control.

Users must be authorized and authenticate to access DCOS services. A DCOS service is a resource protected by an ACL. The ACL defines the individual users, user groups, and their list of accessible DCOS services. There is only one level of authorization: authorized or not.

The access control service provides an HTTP API for managing local users, groups and ACLs in a RESTful fashion. The **Bootstrap superuser** manages the ACL through the DCOS web interface.

Optionally, you can validate remote user credentials against an external LDAP service.