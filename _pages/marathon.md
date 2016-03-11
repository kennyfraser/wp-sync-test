---
ID: 560
post_title: Marathon
author: Joel Hamill
post_date: 2016-03-08 14:04:14
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/marathon/
published: true
import_src:
  - mesosphere-docs/services/marathon.md
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
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3deeed82d
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "10"
---
The DCOS uses Marathon to manage the processes and services and is the "init system" for the DCOS. Marathon starts and monitors your applications and services, automatically healing failures.

A native Marathon instance is installed as a part of the DCOS installation. After DCOS has been started, you can manage the native Marathon instance through the web interface at `<hostname>/marathon` or from the DCOS CLI with the `dcos marathon` command.

You can create additional Marathon instances for specific users by using the instructions below.

# <a name="install"></a>Installing a Marathon instance on DCOS

**Prerequisite**

*   The DCOS CLI must be [installed][1].

1.  Install a Marathon instance:
    
    *   To install a single Marathon instance, enter this command from the DCOS CLI:
        
            $ dcos package install marathon
            
        
        By default the DCOS Service name is `marathon-user`. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathontask.png" rel="attachment wp-att-1410"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/marathontask.png" alt="marathontask" width="709" height="44" class="alignnone size-full wp-image-1410" /></a>
    
    *   To install multiple Marathon instances:
        
        1.  Create a custom JSON configuration file that includes this entry where `<name>` is the unique Marathon instance name:
            
                 {"marathon": {"framework-name": "marathon-<name>" }}
                
            
            **Tip:** You must create a separate JSON configuration file for each Marathon instance.
        
        2.  From the DCOS CLI, enter this command:
            
                 $ dcos package install --options=<config-file>.json marathon
                
        
        3.  From the DCOS web interface **Services** tab, click on your Marathon service name to navigate to the web interface. For more information, see [Deploying Multiple Marathon Instances][2].

# <a name="uninstall"></a>Uninstalling a Marathon Instance

1.  From the DCOS CLI, enter this command:
    
        $ dcos package uninstall marathon
        

2.  Open the Zookeeper Exhibitor web interface at `<hostname>/exhibitor`, where `<hostname>` is the [Mesos Master hostname][3].
    
    1.  Click on the **Explorer** tab and navigate to the `universe/<marathon-user>` folder.
        
        **Important:** Do not delete the `marathon` folder. This is the native DCOS Marathon instance.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathon.png" rel="attachment wp-att-1407"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathon-600x482.png" alt="zkmarathon" width="300" height="241" class="alignnone size-medium wp-image-1407" /></a>
    
    2.  Choose Type **Delete**, enter the required **Username**, **Ticket/Code**, and **Reason** fields, and click **Next**.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathondelete.png" rel="attachment wp-att-1409"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkmarathondelete-600x331.png" alt="zkmarathondelete" width="300" height="166" class="alignnone size-medium wp-image-1409" /></a>
    
    3.  Click **OK** to confirm your deletion.

For more information:

*   <a href="http://mesosphere.github.io/marathon/docs/" target="_blank">Marathon documentation</a>
*   [Deploying a Web App][4]

 [1]: /install/cli/
 [2]: /tutorials/marathon-add-user/
 [3]: /install/awscluster#launchdcos
 [4]: /tutorials/deploywebapp/