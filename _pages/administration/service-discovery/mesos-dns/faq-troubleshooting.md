---
UID: 56df3decad6eb
post_title: 'FAQ &#038; Troubleshooting'
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<h1>How can I check the Mesos-DNS version?</h1>

<p>You can check the Mesos-DNS version with <code>mesos-dns -version</code>.</p>

<p><strong>Note:</strong> We do not recommend upgrading Mesos-DNS independently of DCOS. Use the version of Mesos-DNS that shipped with your version of DCOS.</p>

<h1>What if Mesos-DNS fails to launch?</h1>

<p>Check that port 53 and port 8123 are available and not in use by other processes.</p>

<h1>What if my Agent nodes cannot connect to Mesos-DNS?</h1>

<ul>
<li><p>Make sure that port 53 is not blocked by a firewall rule on your cluster.</p></li>
<li><p>It is possible that the Master nodes are not running. Run <code>sudo systemctl status dcos-mesos-dns</code> and <code>sudo journalctl -u dcos-gen-resolvconf.service -n 200 -f</code> for more information about Mesos-DNS errors.</p></li>
</ul>

<h1>How do I configure my DCOS cluster to communicate with external hosts and services?</h1>

<p>For DNS requests for hostnames or services outside the DCOS cluster, Mesos-DNS will query an external nameserver. By default, Google's nameserver with IP address 8.8.8.8 will be used. If you need to configure a custom external name server, use the <a href="https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/#config-json"><code>resolvers</code> parameter</a> when you first install DCOS.</p>

<p><strong>Important:</strong> External nameservers can only be set when you install DCOS. They cannot be changed after installation.</p>

<h1><a name="leader"></a>What is the difference between leader.mesos and master.mesos?</h1>

<p>To query the leading master node, always query <code>leader.mesos</code>.</p>

<p>If you try to connect to <code>master.mesos</code> using HTTP, you will be automatically redirected to the leading master node.</p>

<p>However, if you try to query or connect to <code>master.mesos</code> using any method other than HTTP, the results will be unpredictable because the name will resolve to a random master node. For example, a service that attempts to register with <code>master.mesos</code> may communicate with a non-leading master node and will be unable to register as a service on the cluster.</p>