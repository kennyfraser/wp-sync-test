---
UID: 56df3def9c7b9
post_title: Log Management with ELK
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can pipe system and application logs from a Mesosphere DCOS cluster to your existing ElasticSearch, Logstash, and Kibana (ELK) server.</p>

<p><strong>Prerequisites</strong></p>

<p>These instructions are based on Mesosphere <a href="../release-notes/community-edition/1-3/">DCOS 1.3</a> on CoreOS 766.4.0 and might differ substantially from other Linux distributions. This document does not explain how to setup and configure an ELK server.</p>

<ul>
<li>A recent ELK stack with an HTTPS interface and certificate.</li>
<li>All DCOS nodes must be able to connect to your ELK server via HTTPS.</li>
<li>The <code>ulimit</code> of open files must be set to <code>unlimited</code> for your user with root access.</li>
</ul>

<h1><a name="all"></a>Step 1: All Nodes</h1>

<p>For all nodes in your DCOS cluster:</p>

<ol>
<li>Install Elastic's <a href="https://github.com/elastic/logstash-forwarder">logstash-forwarder</a>. The project is written in Go and is compiled for most platforms.</li>
<li>Download the HTTPS certificate for your ELK stash. In our examples, we assume the certificate is named <code>elk.crt</code>.</li>
</ol>

<h1><a name="master"></a>Step 2: Master Nodes</h1>

<p>For each Master node in your DCOS cluster:</p>

<ol>
<li><p>Create a <code>logstash.conf</code> configuration file for <code>logstash-forwarder</code> that sends <code>stdin</code> data directly to your ELK server.</p>

<pre><code>{
    "network": {
            "servers": [ "&lt;elk-hostname&gt;:&lt;elk-port&gt;" ],
            "timeout": 15,
            "ssl ca": "elk.crt"
    },
    "files": [
        {
            "paths": [ "-" ]
        }
    ]
}
</code></pre></li>
<li><p>Create a <code>logstash.sh</code> bash script to pipe DCOS service logs from <code>journalctl</code> to <code>logstash-forwarder</code>.</p>

<pre><code>    #!/bin/bash

    journalctl --since="now" -f -u dcos-exhibitor.service -u dcos-marathon.service -u dcos-mesos-dns.service -u dcos-mesos-master.service -u dcos-nginx.service | ./logstash-forwarder -config logstash.conf
</code></pre></li>
<li><p>Run <code>logstash.sh</code> as a service.</p>

<p><strong>Important:</strong> If any of these DCOS services are restarted, you must restart this script.</p></li>
</ol>

<h1><a name="agent"></a>Step 3: Agent Nodes</h1>

<p>For each Agent node in your DCOS cluster:</p>

<ol>
<li><p>Create a <code>logstash.conf</code> configuration file for <code>logstash-forwarder</code> that sends <code>stdin</code> data directly to your ELK server.</p>

<pre><code>{
    "network": {
        "servers": [ "&lt;elk-hostname&gt;:&lt;elk-port&gt;" ],
        "timeout": 15,
        "ssl ca": "elk.crt"
    },
    "files": [
        {
            "paths": [
                "-",
                "/var/lib/mesos/slave/slaves/*/frameworks/*/executors/*/runs/latest/stdout",
                "/var/lib/mesos/slave/slaves/*/frameworks/*/executors/*/runs/latest/stderr"
            ]
        }
    ]
}
</code></pre></li>
<li><p>Create a <code>logstash.sh</code> bash script to pipe DCOS service logs from <code>journalctl</code> to <code>logstash-forwarder</code>.</p>

<pre><code>    #!/bin/bash

    journalctl --since="now" -f -u dcos-mesos-slave-public.service  | ./logstash-forwarder -config logstash.conf
</code></pre></li>
<li><p>Run <code>logstash.sh</code> as a service.</p>

<p><strong>Important:</strong> If any of these DCOS services are restarted, you must restart this script.</p></li>
</ol>

<h3>Known Issue</h3>

<ul>
<li>The agent node logstash configuration expects tasks to write logs to <code>stdout</code> and <code>stderr</code>. Some DCOS services, including Cassandra and Kafka, do not write logs to <code>stdout</code> and <code>stderr</code>. If you want to log these services, you must customize your agent node logstash configuration.</li>
</ul>

<h1>What's Next</h1>

<p>For details on how to filter your logs with ELK, see <a href="./logging/filter-elk/">Filtering DCOS logs with ELK</a>.</p>