---
ID: 524
post_title: Log Management with ELK
author: Joel Hamill
post_date: 2016-03-08 14:03:42
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/administration/logging/log-management-with-elk/
published: true
import_src:
  - mesosphere-docs/logging/elk.md
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
page_options_topic_page:
  - ""
page_options_require_authentication:
  - ""
UID:
  - 56df3def9c7b9
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "3"
---
You can pipe system and application logs from a Mesosphere DCOS cluster to your existing ElasticSearch, Logstash, and Kibana (ELK) server.

**Prerequisites**

These instructions are based on Mesosphere [DCOS 1.3][1] on CoreOS 766.4.0 and might differ substantially from other Linux distributions. This document does not explain how to setup and configure an ELK server.

*   A recent ELK stack with an HTTPS interface and certificate.
*   All DCOS nodes must be able to connect to your ELK server via HTTPS.
*   The `ulimit` of open files must be set to `unlimited` for your user with root access.

# <a name="all"></a>Step 1: All Nodes

For all nodes in your DCOS cluster:

1.  Install Elastic's [logstash-forwarder][2]. The project is written in Go and is compiled for most platforms.
2.  Download the HTTPS certificate for your ELK stash. In our examples, we assume the certificate is named `elk.crt`.

# <a name="master"></a>Step 2: Master Nodes

For each Master node in your DCOS cluster:

1.  Create a `logstash.conf` configuration file for `logstash-forwarder` that sends `stdin` data directly to your ELK server.
    
        {
            "network": {
                    "servers": [ "<elk-hostname>:<elk-port>" ],
                    "timeout": 15,
                    "ssl ca": "elk.crt"
            },
            "files": [
                {
                    "paths": [ "-" ]
                }
            ]
        }
        

2.  Create a `logstash.sh` bash script to pipe DCOS service logs from `journalctl` to `logstash-forwarder`.
    
            #!/bin/bash
        
            journalctl --since="now" -f -u dcos-exhibitor.service -u dcos-marathon.service -u dcos-mesos-dns.service -u dcos-mesos-master.service -u dcos-nginx.service | ./logstash-forwarder -config logstash.conf
        

3.  Run `logstash.sh` as a service.
    
    **Important:** If any of these DCOS services are restarted, you must restart this script.

# <a name="agent"></a>Step 3: Agent Nodes

For each Agent node in your DCOS cluster:

1.  Create a `logstash.conf` configuration file for `logstash-forwarder` that sends `stdin` data directly to your ELK server.
    
        {
            "network": {
                "servers": [ "<elk-hostname>:<elk-port>" ],
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
        

2.  Create a `logstash.sh` bash script to pipe DCOS service logs from `journalctl` to `logstash-forwarder`.
    
            #!/bin/bash
        
            journalctl --since="now" -f -u dcos-mesos-slave-public.service  | ./logstash-forwarder -config logstash.conf
        

3.  Run `logstash.sh` as a service.
    
    **Important:** If any of these DCOS services are restarted, you must restart this script.

### Known Issue

*   The agent node logstash configuration expects tasks to write logs to `stdout` and `stderr`. Some DCOS services, including Cassandra and Kafka, do not write logs to `stdout` and `stderr`. If you want to log these services, you must customize your agent node logstash configuration.

# What's Next

For details on how to filter your logs with ELK, see [Filtering DCOS logs with ELK][3].

 [1]: ../release-notes/community-edition/1-3/
 [2]: https://github.com/elastic/logstash-forwarder
 [3]: ./logging/filter-elk/