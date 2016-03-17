---
UID: 56df3debbe7cd
post_title: Security and Authentication
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: true
---
<p>You can enable authorization and authentication in your datacenter with DCOS Enterprise Edition. Authentication is managed through the DCOS web interface. Authorization is defined by access control lists (ACLs). The Admin Router enforces access control.</p>

<p>Users must be authorized in the ACL and then authenticated to access DCOS services. DCOS services are resources that are protected by an ACL. The ACL defines the individual users, user groups, and their list of accessible DCOS services. There is only one level of authorization: authorized or not.</p>

<p>The access control service provides an HTTP API for managing local users, groups and ACLs in a RESTful fashion. The Bootstrap superuser manages the ACL through the DCOS web interface.</p>

<p>You can validate remote user credentials against an external LDAP service.</p>