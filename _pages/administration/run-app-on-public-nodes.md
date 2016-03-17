---
UID: 56df3def5b3dc
post_title: Running Your App on Public Nodes
post_excerpt: ""
layout: page
published: true
menu_order: 11
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The native Marathon instance is configured to receive offers from the public agent nodes (<code>--mesos_role="slave_public"</code>). Marathon apps are configured to use unreserved resources and launch on private agent nodes (<code>--default_accepted_resource_roles="*"</code>).</p>

<ul>
<li><p>To run a Marathon app on public agent nodes, you must set the <code>acceptedResourceRoles</code> property to:</p>

<p>"acceptedResourceRoles":["slave_public"]</p></li>
<li><p>To run a Marathon app on any agent node type, you must set the <code>acceptedResourceRoles</code> property to:</p>

<p>"acceptedResourceRoles":["slave_public", "*"]</p></li>
</ul>

<p>For a comprehensive example of deploying an app in the public zone to route HTTP requests, see <a href="/tutorials/deploywebapp/">Deploying a Containerized App on a Public Node</a>.</p>