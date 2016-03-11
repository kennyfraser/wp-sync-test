---
ID: 516
post_title: Installing a DCOS service
author: Joel Hamill
post_date: 2016-03-08 14:03:33
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/installing-a-dcos-service/
published: true
import_src:
  - >
    mesosphere-docs/install/installdatacenter.md
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
  - 56df3defb6268
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "53"
---
You can install datacenter application packages directly from the DCOS package [repository][1].

**Prerequisite:**

*   DCOS must be [installed][2].

To install a DCOS service:

1.  Check for DCOS CLI package updates:
    
         dcos package update
        

2.  Install the datacenter service:
    
         dcos package install <servicename>
        
    
    For example, to install Chronos:
    
         dcos package install chronos
        

3.  Verify that the service is successfully installed:
    
    *   From the DCOS CLI: `dcos package list`
    *   From the Mesosphere DCOS web interface: Go to the Services tab and confirm that the datacenter services are running. <a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/services.png" rel="attachment wp-att-1126"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/services-800x486.png" alt="Services page" width="800" height="486" class="alignnone size-large wp-image-1126" /></a>
    *   From the Mesos web interface at `<hostname>/mesos`, verify that the service has registered and is starting tasks.

 [1]: ../overview/universe/
 [2]: ../administering/installing/