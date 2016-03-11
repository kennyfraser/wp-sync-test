---
ID: 1316
post_title: Adding a Kafka broker
author: Joel Hamill
post_date: 2016-03-08 14:04:15
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/adding-a-kafka-broker/
published: true
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
  - 56df3deddb0b1
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "100"
---
In this example, a Kafka broker is added by using the Kafka CLI.

**Prerequisites:**

*   The DCOS Kafka service is [installed][1]

1.  From the Kafka CLI add a new broker:
    
        $ dcos kafka broker add 1
        
        broker added:
          id: 1
          active: false
          state: stopped
          resources: cpus:1.00, mem:2048, heap:1024, port:auto
          failover: delay:1m, max-delay:10m
          stickiness: period:10m
        

2.  Start a new broker:
    
        $ dcos kafka broker start 1
        Broker 1 started
        

3.  Verify that the broker is added and started:
    
        $ dcos kafka broker list

 [1]: ../reference/kafka/#kafkainstall