---
ID: 107
post_title: 'Step 3: Create a script for IP address discovery'
author: gitsync
post_date: 2016-02-22 18:13:27
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/step-3-create-a-script-for-ip-address-discovery-2/
published: true
header_0_background:
  - 'a:1:{i:0;s:4:"fill";}'
header_0_background_fill_style:
  - 'a:1:{i:0;s:4:"dark";}'
header_0_logo_style:
  - 'a:1:{i:0;s:11:"color-light";}'
header_0_navigation_style:
  - 'a:1:{i:0;s:5:"light";}'
header:
  - 'a:1:{i:0;s:1:"1";}'
page_header_0_show_page_header:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_fill_screen:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_background:
  - 'a:1:{i:0;s:11:"transparent";}'
page_header_0_show_background_image:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_background_video:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_headline:
  - 'a:1:{i:0;s:0:"";}'
page_header_0_headline_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_description:
  - 'a:1:{i:0;s:0:"";}'
page_header_0_description_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_show_image:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_content_alignment:
  - 'a:1:{i:0;s:6:"center";}'
page_header_0_content_style:
  - 'a:1:{i:0;s:4:"dark";}'
page_header_0_actions:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_actions_footnote:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_video:
  - 'a:1:{i:0;s:1:"0";}'
page_header:
  - 'a:1:{i:0;s:1:"1";}'
page_options_require_authentication:
  - 'a:1:{i:0;s:0:"";}'
hide_from_navigation:
  - 'a:1:{i:0;s:1:"0";}'
hide_from_related:
  - 'a:1:{i:0;s:1:"1";}'
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

[Step 4: Configure and install DCOS][2]

 [1]: ../getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/
 [2]: ../step-4-configure-and-install-dcos/