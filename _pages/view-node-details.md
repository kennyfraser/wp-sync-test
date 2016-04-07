---
UID: 56f9a5595fa87
post_title: Viewing DCOS Registered Apps
post_excerpt: ""
layout: page
published: true
menu_order: 42
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
You can get a detailed view of which nodes in your cluster are running which service.

[tutorial-prereq]

1.  SSH into your [master node][1].

2.  Run this command from your master node to view the node details:
    
          $ curl http://master.mesos:8123/v1/enumerate
        
    
    In this example Kafka and Chronos are installed:
    
          $ curl http://master.mesos:8123/v1/enumerate
          {
            "frameworks": [
             {
              "tasks": null,
              "name": "chronos"
             },
             {
              "tasks": null,
              "name": "kafka"
             },
             {
              "tasks": [
               {
                "name": "kafka",
                "id": "kafka.443d5d63-f527-11e5-81a5-2a8c0aaf83b5",
                "records": [
                 {
                  "name": "kafka.marathon.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "kafka-7fdws-s0.marathon.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "kafka.marathon.slave.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "kafka-7fdws-s0.marathon.slave.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "_kafka._tcp.marathon.slave.mesos.",
                  "host": "kafka-7fdws-s0.marathon.slave.mesos.:14799",
                  "rtype": "SRV"
                 },
                 {
                  "name": "_kafka._udp.marathon.slave.mesos.",
                  "host": "kafka-7fdws-s0.marathon.slave.mesos.:14799",
                  "rtype": "SRV"
                 },
                 {
                  "name": "_kafka._tcp.marathon.mesos.",
                  "host": "kafka-7fdws-s0.marathon.mesos.:14799",
                  "rtype": "SRV"
                 }
                ]
               },
               {
                "name": "chronos",
                "id": "chronos.40a4f462-f527-11e5-81a5-2a8c0aaf83b5",
                "records": [
                 {
                  "name": "chronos.marathon.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "chronos-4dj75-s0.marathon.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "chronos.marathon.slave.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "chronos-4dj75-s0.marathon.slave.mesos.",
                  "host": "10.0.2.162",
                  "rtype": "A"
                 },
                 {
                  "name": "_chronos._tcp.marathon.slave.mesos.",
                  "host": "chronos-4dj75-s0.marathon.slave.mesos.:9106",
                  "rtype": "SRV"
                 },
                 {
                  "name": "_chronos._udp.marathon.slave.mesos.",
                  "host": "chronos-4dj75-s0.marathon.slave.mesos.:9106",
                  "rtype": "SRV"
                 },
                 {
                  "name": "_chronos._tcp.marathon.mesos.",
                  "host": "chronos-4dj75-s0.marathon.mesos.:9106",
                  "rtype": "SRV"
                 }
                ]
               }
              ],
              "name": "marathon"
             }
            ]

 [1]: /administration/sshcluster/