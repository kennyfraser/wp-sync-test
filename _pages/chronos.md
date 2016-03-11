---
ID: 553
post_title: Chronos
author: Joel Hamill
post_date: 2016-03-08 14:04:12
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/chronos/
published: true
import_src:
  - mesosphere-docs/services/chronos.md
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
  - 56df3def24ea7
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "4"
---
Chronos is the "cron" for your Mesosphere DCOS. It is a highly-available distributed job scheduler, providing the most robust way to run batch jobs in your datacenter. Chronos schedules jobs across the Mesos cluster and manages dependencies between jobs in an intelligent way.

*   [Installing Chronos on DCOS][1]
*   [Uninstalling Chronos][2]

### <a name="chronosinstall"></a>Installing Chronos on DCOS

**Prerequisite**

*   The DCOS CLI must be [installed][3].

1.  From the DCOS CLI, enter this command:
    
        $ dcos package install chronos
        
    
    **Tip:** You can specify a JSON configuration file along with the Chronos installation command: `dcos package install chronos --option <config_file>`. For more information, see the [dcos package section of the CLI command reference][4].

2.  Verify that Chronos is installed:
    
    *   From the DCOS CLI: `dcos package list`
    *   From the DCOS web interface, go to the Services tab and confirm that Chronos is running. You can click on the Chronos service to go the web interface. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chronostask.png" rel="attachment wp-att-1512"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chronostask.png" alt="chronostask" width="710" height="41" class="alignnone size-full wp-image-1512" /></a>

### <a name="uninstall"></a>Uninstalling Chronos

1.  From the DCOS CLI, enter this command:
    
        $ dcos package uninstall chronos
        

2.  Open the Zookeeper Exhibitor web interface at `<hostname>/exhibitor`, where `<hostname>` is the [Mesos Master hostname][5].
    
    1.  Click on the **Explorer** tab and navigate to the `chronos` folder.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a>
    
    2.  Choose Type **Delete**, enter the required **Username**, **Ticket/Code**, and **Reason** fields, and click **Next**.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkchronosdelete.png" rel="attachment wp-att-1617"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkchronosdelete.png" alt="zkchronosdelete" width="613" height="339" class="alignnone size-full wp-image-1617" /></a>
    
    3.  Click **OK** to confirm your deletion.

For more information:

*   <a href="http://mesos.github.io/chronos/docs/" target="_blank">Chronos documentation</a>

 [1]: #chronosinstall
 [2]: #uninstall
 [3]: /install/cli/
 [4]: ../administration/introcli/command-reference/
 [5]: /install/awscluster#launchdcos