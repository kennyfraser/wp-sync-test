---
ID: 3230
post_title: Security and Authentication
author: Joel Hamill
post_date: 2016-03-08 14:03:47
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/administration/security-and-authentication/
published: true
header:
  - "0"
page_header:
  - "0"
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - "1"
UID:
  - 56df3debbe7cd
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "4"
---
You can enable authorization and authentication in your datacenter with DCOS Enterprise Edition. Authentication is managed through the DCOS web interface. Authorization is defined by access control lists (ACLs). The Admin Router enforces access control.

Users must be authorized in the ACL and then authenticated to access DCOS services. DCOS services are resources that are protected by an ACL. The ACL defines the individual users, user groups, and their list of accessible DCOS services. There is only one level of authorization: authorized or not.

The access control service provides an HTTP API for managing local users, groups and ACLs in a RESTful fashion. The Bootstrap superuser manages the ACL through the DCOS web interface.

You can validate remote user credentials against an external LDAP service.