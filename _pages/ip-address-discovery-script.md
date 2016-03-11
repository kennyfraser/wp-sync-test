---
ID: 3145
post_title: 'Step 1: Create a script for IP address discovery'
author: Joel Hamill
post_date: 2016-03-08 14:04:05
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/step-1-create-a-script-for-ip-address-discovery/
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
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3dec0f535
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "3"
---
In this step you create an IP detect script to broadcast the IP address of each node across the cluster. Each node in a DCOS cluster has a unique IP address that is used to communicate between nodes in the cluster. The IP detect script prints the unique IPv4 address of a node to STDOUT each time DCOS is started on the node.

**Important:** The IP address of a node must not change after DCOS is installed on the node. For example, the IP address must not change when a node is rebooted or if the DHCP lease is renewed. If the IP address of a node does change, the node must be [wiped and reinstalled][1].

1.  Create a directory named `genconf` on your bootstrap node and navigate to it.
    
        $ mkdir -p genconf && cd genconf
        

2.  Create an IP detection script for your environment and save as `genconf/ip-detect`. You can use the examples below.
    
    *   #### Use the AWS Metadata Server
        
        This method uses the AWS Metadata service to get the IP address:
        
            #!/bin/sh
            # Example ip-detect script using an external authority
            # Uses the AWS Metadata Service to get the node's internal
            # ipv4 address
            curl -fsSL http://169.254.169.254/latest/meta-data/local-ipv4
            
    
    *   #### Use the GCE Metadata Server
        
        This method uses the GCE Metadata Server to get the IP address:
        
            #!/bin/sh
            # Example ip-detect script using an external authority
            # Uses the GCE metadata server to get the node's internal
            # ipv4 address
            
            curl -fsSl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/network-interfaces/0/ip
            
    
    *   #### Use the IP address of an existing interface
        
        This method discovers the IP address of a particular interface of the node.
        
        If you have multiple generations of hardware with different internals, the interface names can change between hosts. The IP detection script must account for the interface name changes. The example script could also be confused if you attach multiple IP addresses to a single interface, or do complex Linux networking, etc.
        
            #!/usr/bin/env bash
            set -o nounset -o errexit
            export PATH=/usr/sbin:/usr/bin:$PATH
            echo $(ip addr show eth0 | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | head -1)
            
    
    *   #### Use the network route to the Mesos master
        
        This method uses the route to a Mesos master to find the source IP address to then communicate with that node.
        
        In this example, we assume that the Mesos master has an IP address of `172.28.128.3`. You can use any language for this script. Your Shebang line must be pointed at the correct environment for the language used and the output must be the correct IP address.
        
            #!/usr/bin/env bash
            set -o nounset -o errexit
            
            MASTER_IP=172.28.128.3
            
            echo $(/usr/sbin/ip route show to match 172.28.128.3 | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | tail -1)
            

## Next step

[Step 2: Configure and install DCOS][2]

 [1]: ../getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/
 [2]: ../configure-and-install-dcos/