---
UID: 56df3dec0f535
post_title: 'Step 1: Create a script for IP address discovery'
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>In this step you create an IP detect script to broadcast the IP address of each node across the cluster. Each node in a DCOS cluster has a unique IP address that is used to communicate between nodes in the cluster. The IP detect script prints the unique IPv4 address of a node to STDOUT each time DCOS is started on the node.</p>

<p><strong>Important:</strong> The IP address of a node must not change after DCOS is installed on the node. For example, the IP address must not change when a node is rebooted or if the DHCP lease is renewed. If the IP address of a node does change, the node must be <a href="../getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/">wiped and reinstalled</a>.</p>

<ol>
<li><p>Create a directory named <code>genconf</code> on your bootstrap node and navigate to it.</p>

<pre><code>$ mkdir -p genconf &amp;&amp; cd genconf
</code></pre></li>
<li><p>Create an IP detection script for your environment and save as <code>genconf/ip-detect</code>. You can use the examples below.</p>

<ul>
<li><h4>Use the AWS Metadata Server</h4>

<p>This method uses the AWS Metadata service to get the IP address:</p>

<pre><code>#!/bin/sh
# Example ip-detect script using an external authority
# Uses the AWS Metadata Service to get the node's internal
# ipv4 address
curl -fsSL http://169.254.169.254/latest/meta-data/local-ipv4
</code></pre></li>
<li><h4>Use the GCE Metadata Server</h4>

<p>This method uses the GCE Metadata Server to get the IP address:</p>

<pre><code>#!/bin/sh
# Example ip-detect script using an external authority
# Uses the GCE metadata server to get the node's internal
# ipv4 address

curl -fsSl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/network-interfaces/0/ip
</code></pre></li>
<li><h4>Use the IP address of an existing interface</h4>

<p>This method discovers the IP address of a particular interface of the node.</p>

<p>If you have multiple generations of hardware with different internals, the interface names can change between hosts. The IP detection script must account for the interface name changes. The example script could also be confused if you attach multiple IP addresses to a single interface, or do complex Linux networking, etc.</p>

<pre><code>#!/usr/bin/env bash
set -o nounset -o errexit
export PATH=/usr/sbin:/usr/bin:$PATH
echo $(ip addr show eth0 | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | head -1)
</code></pre></li>
<li><h4>Use the network route to the Mesos master</h4>

<p>This method uses the route to a Mesos master to find the source IP address to then communicate with that node.</p>

<p>In this example, we assume that the Mesos master has an IP address of <code>172.28.128.3</code>. You can use any language for this script. Your Shebang line must be pointed at the correct environment for the language used and the output must be the correct IP address.</p>

<pre><code>#!/usr/bin/env bash
set -o nounset -o errexit

MASTER_IP=172.28.128.3

echo $(/usr/sbin/ip route show to match 172.28.128.3 | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | tail -1)
</code></pre></li>
</ul></li>
</ol>

<h2>Next step</h2>

<p><a href="../configure-and-install-dcos/">Step 2: Configure and install DCOS</a></p>