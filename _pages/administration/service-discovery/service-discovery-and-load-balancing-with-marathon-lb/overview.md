---
UID: 56df3dec56bda
post_title: Overview of marathon-lb
post_excerpt: ""
layout: page
published: true
menu_order: 90
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>After you boot a DCOS cluster, all tasks can be discovered using Mesos-DNS. Discovery through DNS, however, has some limitations that include:</p>

<ul>
<li>DNS does not identify service ports, unless you use an SRV query; most apps are not able to use SRV records "out of the box."</li>
<li>DNS does not have fast failover.</li>
<li>DNS records have a TTL (time to live) and Mesos-DNS uses polling to create the DNS records; this can result in stale records.</li>
<li>DNS records do not provide any service health data.</li>
<li>Some applications and libraries do not correctly handle multiple A records; in some cases the query might be cached and not correctly reloaded as required.</li>
</ul>

<p>To address these concerns, we provide a tool for Marathon called Marathon Load Balancer, or marathon-lb for short.</p>

<p>Marathon-lb is based on HAProxy, a rapid proxy and load balancer. HAProxy provides proxying and load balancing for TCP and HTTP based applications, with features such as SSL support, HTTP compression, health checking, Lua scripting and more. Marathon-lb subscribes to Marathon’s event bus and updates the HAProxy configuration in real time.</p>

<p>You can can configure marathon-lb with various topologies. Here are some examples of how you might use marathon-lb:</p>

<ul>
<li>Use marathon-lb as your edge load balancer and service discovery mechanism. You could run marathon-lb on public-facing nodes to route ingress traffic. You would use the IP addresses of your public-facing nodes in the A-records for your internal or external DNS records (depending on your use-case).</li>
<li>Use marathon-lb as an internal LB and service discovery mechanism, with a separate HA load balancer for routing public traffic in. For example, you may use an external F5 load balancer on-premise, or an Elastic Load Balancer on Amazon Web Services.</li>
<li>Use marathon-lb strictly as an internal load balancer and service discovery mechanism.</li>
<li>You might also want to use a combination of internal and external load balancers, with different services exposed on different load balancers.</li>
</ul>

<p>Here, we discuss the fourth option above in order to highlight the features of marathon-lb.</p>

<p><img src="https://mesosphere.com/wp-content/uploads/2015/12/lb1.jpg" alt="lb1" width="640" height="647" class="aligncenter size-full wp-image-3820" /></p>