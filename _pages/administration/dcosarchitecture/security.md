---
UID: 56df3def54cf0
post_title: Network Security
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The DCOS provides the admin, private, and public security zones.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/security-zones-ce.jpg" rel="attachment wp-att-1583"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/security-zones-ce-800x640.jpg" alt="security-zones-ce" width="800" height="640" class="alignnone size-large wp-image-1583" /></a></p>

<h1>Admin zone</h1>

<p>The <strong>admin</strong> zone is accessible via HTTP/HTTPS and SSH connections, and provides access to your master nodes. It also provides proxy access to the other nodes in your cluster via URL routing. For security, you can configure a whitelist during <a href="../../../getting-started/installing/">cluster creation</a> so that only specific IP address ranges are permitted to access the admin zone. The DCOS template creates 1 or 3 Mesos master nodes in the admin zone.</p>

<h1>Private zone</h1>

<p>The <strong>private</strong> zone is a non-routable network that is only accessible from the admin zone or through the edgerouter from the public zone. Deployed apps and services are run in the private zone. This zone is where the majority of Mesos agent nodes are run. By default, the DCOS template creates 5 Mesos agent nodes in the private zone.</p>

<h1>Public zone</h1>

<p>The optional <strong>public</strong> zone is where publicly accessible applications are run. Generally, only a small number of agent nodes are run in this zone. The edge router forwards traffic to applications running in the private zone. By default, the DCOS template creates 1 agent node in the public zone.</p>

<p>The agent nodes in the public zone are labeled with a special role so that only specific tasks can be scheduled here. These agent nodes have both public and private IP addresses and only specific ports are open in the firewall.</p>

<!-- add more details around public zone -->

<h1>Limitations</h1>

<ul>
<li>The DCOS Community Edition does not provide authentication. Authentication is available in the <a href="https://mesosphere.com/product/#" target="_blank">DCOS Enterprise Edition</a>. </li>
<li>The DCOS CLI and web interface do not currently use an encrypted channel for communication. However, you can upload your own SSL certificate to the masters and change your CLI and web interface configuration to use HTTPS instead of HTTP.</li>
<li>You must secure your cluster by using security rules. It is strongly recommended that you only allow internal traffic.</li>
<li>If there is sensitive data in your cluster, follow standard cloud policies for accessing that data. Either set up a point to point VPN between your secure networks or run a VPN server inside your DCOS cluster.</li>
</ul>