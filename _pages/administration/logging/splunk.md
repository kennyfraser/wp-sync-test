---
UID: 56df3def73d6c
post_title: Log Management with Splunk
post_excerpt: ""
layout: page
published: true
menu_order: 5
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can pipe system and application logs from a Mesosphere DCOS cluster to your existing Splunk server.</p>

<p>These instructions are based on Mesosphere <a href="../release-notes/community-edition/1-3/">DCOS 1.3</a> on CoreOS 766.4.0 and might differ substantially from other Linux distributions. This document does not explain how to setup and configure a Splunk server.</p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>An existing Splunk installation that can ingest data for indexing.</li>
<li>All DCOS nodes must be able to connect to your Splunk indexer via HTTP or HTTPS. (See Splunk's documentation for instructions on enabling HTTPS.) </li>
<li>The <code>ulimit</code> of open files must be set to <code>unlimited</code> for your user with root access.</li>
</ul>

<h1>Step 1: All Nodes</h1>

<p>For all nodes in your DCOS cluster:</p>

<ol>
<li>Install Splunk's <a href="http://www.splunk.com/en_us/download/universal-forwarder.html">universal forwarder</a>. Splunk provides packages and installation instructions for most platforms.</li>
<li>Make sure the forwarder has the credentials it needs to send data to the indexer. See Splunk's documentation for details.</li>
<li>Start the forwarder.</li>
</ol>

<h1>Step 2: Master Nodes</h1>

<p>For each Master node in your DCOS cluster:</p>

<ol>
<li><p>Create a script <code>$SPLUNK_HOME/bin/scripts/journald-master.sh</code> that will obtain the Mesos master logs from <code>journald</code>:</p>

<pre><code>#!/bin/sh

exec journalctl --since=now -f 
    -u dcos-exhibitor.service 
    -u dcos-marathon.service 
    -u dcos-mesos-dns.service 
    -u dcos-mesos-master.service 
    -u dcos-nginx.service
</code></pre></li>
<li><p>Make the script executable:</p>

<pre><code>chmod +x "$SPLUNK_HOME/bin/scripts/journald-master.sh"
</code></pre></li>
<li><p>Add the script as an input to the forwarder:</p>

<pre><code>"$SPLUNK_HOME/bin/splunk" add exec 
    -source "$SPLUNK_HOME/bin/scripts/journald-master.sh" 
    -interval 0
</code></pre></li>
</ol>

<h1>Step 3: Agent Nodes</h1>

<p>For each agent node in your DCOS cluster:</p>

<ol>
<li><p>Create a script <code>$SPLUNK_HOME/bin/scripts/journald-agent.sh</code> that will obtain the Mesos agent logs from <code>journald</code>:</p>

<pre><code>#!/bin/sh

exec journalctl --since=now -f 
    -u dcos-mesos-slave.service 
    -u dcos-mesos-slave-public.service
</code></pre></li>
<li><p>Make the script executable:</p>

<pre><code>chmod +x "$SPLUNK_HOME/bin/scripts/journald-agent.sh"
</code></pre></li>
<li><p>Add the script as an input to the forwarder:</p>

<pre><code>"$SPLUNK_HOME/bin/splunk" add exec 
    -source "$SPLUNK_HOME/bin/scripts/journald-agent.sh" 
    -interval 0
</code></pre></li>
<li><p>Add the task logs as inputs to the forwarder:</p>

<pre><code>"$SPLUNK_HOME/bin/splunk" add monitor '/var/lib/mesos/slave' 
    -whitelist '/stdout$|/stderr$'
</code></pre></li>
</ol>

<h1>Known Issue</h1>

<ul>
<li>The agent node Splunk forwarder configuration expects tasks to write logs to <code>stdout</code> and <code>stderr</code>. Some DCOS services, including Cassandra and Kafka, do not write logs to <code>stdout</code> and <code>stderr</code>. If you want to log these services, you must customize your agent node Splunk forwarder configuration.</li>
</ul>

<h1>What's Next</h1>

<p>For details on how to filter your logs with Splunk, see <a href="../../logging/filter-splunk/">Filtering DCOS logs with Splunk</a>.</p>