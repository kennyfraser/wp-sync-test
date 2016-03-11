---
ID: 530
post_title: System Logging
author: Joel Hamill
post_date: 2016-03-08 14:03:43
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/administration/logging/system-logging/
published: true
import_src:
  - mesosphere-docs/logging/system-logs.md
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
  - 56df3def6cd0a
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "1"
---
To access the DCOS core component logs, you must have SSH access to your DCOS cluster. The DCOS core components use the host’s systemd journal for logging; they do not use the standard Mesos sandbox logs.

1.  [SSH into your master node][1].

2.  Enter the command for the component whose logs you wish to view:
    
    **Admin Router**
    
           journalctl -u dcos-nginx -b
        
    
    **DCOS Marathon**
    
           journalctl -u dcos-marathon -b
        
    
    **gen-resolvconf**
    
           journalctl -u dcos-gen-resolvconf -b
        
    
    **Mesos master node**
    
           journalctl -u dcos-mesos-master -b
        
    
    **Mesos agent node**
    
           journalctl -u dcos-mesos-slave -b
        
    
    **Mesos DNS**
    
           journalctl -u dcos-mesos-dns -b
        
    
    **ZooKeeper**
    
           journalctl -u dcos-exhibitor -b

 [1]: /install/sshcluster/