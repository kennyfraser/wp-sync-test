---
post_title: Spark
post_excerpt: ""
layout: page
published: true
menu_order: 9
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
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