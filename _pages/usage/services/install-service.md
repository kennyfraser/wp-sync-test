---
UID: 56f9844a6d586
post_title: Installing a DCOS service
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
You can install datacenter application packages directly from the DCOS package [repository][1].

**Prerequisite:**

*   DCOS must be [installed][2].

To install a DCOS service:

1.  Install the datacenter service with this command:
    
         dcos package install <servicename>
        
    
    For example, to install Chronos:
    
         dcos package install chronos
        

2.  Verify that the service is successfully installed:
    
    *   From the DCOS CLI: `dcos package list`
    *   From the Mesosphere DCOS web interface: Go to the Services tab and confirm that the datacenter services are running. <a href="/wp-content/uploads/2015/12/services.png" rel="attachment wp-att-1126"><img src="/wp-content/uploads/2015/12/services-800x486.png" alt="Services page" width="800" height="486" class="alignnone size-large wp-image-1126" /></a>
    *   From the Mesos web interface at `<hostname>/mesos`, verify that the service has registered and is starting tasks.

 [1]: /usage/services/package-repo/
 [2]: /administration/installing/