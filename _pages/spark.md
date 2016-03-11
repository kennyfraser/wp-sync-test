---
ID: 564
post_title: Spark
author: Joel Hamill
post_date: 2016-03-08 14:04:17
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/spark/
published: true
import_src:
  - mesosphere-docs/services/spark.md
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
  - 56df3deee69d0
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "9"
---
Spark is a fast, general-purpose cluster computing system that makes parallel jobs easy to write.

# <a name="sparkinstall"></a>Installing Spark on DCOS

**Prerequisites:**

*   The DCOS CLI must be [installed][1]. 
*   It is recommend that you have minimum of two nodes with at least 2 CPUs and 2 GB of RAM available for the Spark Service and running a Spark job.

1.  From the DCOS CLI, enter this command to install the Spark DCOS service and CLI:
    
        $ dcos package install spark
        
    
    **Tip:** It can take up to 15 minutes to download and complete deployment, depending on your internet connection.

2.  Verify that Spark is running:
    
    *   From the DCOS web interface, go to the Services tab and confirm that Spark is running. Click the **spark** row item to view the Spark web interface. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/sparktask.png" rel="attachment wp-att-1236"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/sparktask.png" alt="sparktask" width="717" height="41" class="alignnone size-full wp-image-1236" /></a>
    *   From the DCOS CLI: `dcos package list`
    *   From the Mesos web interface at `http://<hostname>/mesos`, verify that the Spark framework has registered and is starting tasks. There should be several journalnodes, namenodes, and datanodes running as tasks. Wait for all of these to show the RUNNING state.

# <a name="uninstall"></a>Uninstalling Spark

1.  From the DCOS CLI, enter this command:
    
        $ dcos package uninstall spark
        

2.  Open the Zookeeper Exhibitor web interface at `<hostname>/exhibitor`, where `<hostname>` is the [Mesos Master hostname][2].
    
    1.  Click on the **Explorer** tab and navigate to the `spark_mesos_dispatcher` folder.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkspark.png" rel="attachment wp-att-1413"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkspark-600x473.png" alt="zkspark" width="300" height="237" class="alignnone size-medium wp-image-1413" /></a>
    
    2.  Choose Type **Delete**, enter the required **Username**, **Ticket/Code**, and **Reason** fields, and click **Next**.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zksparkdelete.png" rel="attachment wp-att-1412"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zksparkdelete-600x336.png" alt="zksparkdelete" width="300" height="168" class="alignnone size-medium wp-image-1412" /></a>
    
    3.  Click **OK** to confirm your deletion.

For more information, see the <a href="https://github.com/mesosphere/dcos-spark" target="_blank">DCOS Spark CLI</a> documentation on GitHub.

 [1]: /install/cli/
 [2]: /install/awscluster#launchdcos