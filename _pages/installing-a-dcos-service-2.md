---
ID: 525
post_title: Installing a DCOS service
post_date: 2016-02-24 14:49:26
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/installing-a-dcos-service-2/
published: true
post_parent: 1444
menu_order: 53
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
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