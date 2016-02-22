---
ID: 99
post_title: Running a Spark Job
author: gitsync
post_date: 2016-02-22 18:13:26
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/running-a-spark-job/
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
page_options_topic_page:
  - 'a:1:{i:0;s:0:"";}'
page_options_require_authentication:
  - 'a:1:{i:0;s:0:"";}'
hide_from_navigation:
  - 'a:1:{i:0;s:1:"0";}'
hide_from_related:
  - 'a:1:{i:0;s:1:"0";}'
---
In this tutorial a Spark job is run by using DCOS.

**Prerequisites:**

*   [Running DCOS cluster][1] with at least 3 private agent nodes.
*   [Configured DCOS CLI][2].
*   Upload your Spark application .jar to an external file server that is reachable by the DCOS cluster. For example: `http://external.website/mysparkapp.jar`.

1.  From the DCOS CLI, enter this command to install the Spark DCOS service and CLI:
    
        $ dcos package install spark
        
    
    **Tip:** It can take up to 15 minutes to download and complete deployment, depending on your internet connection.

2.  Verify that Spark is running:
    
    *   From the DCOS web interface, go to the Services tab and confirm that Spark is running. Click the **spark** row item to view the Spark web interface. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/sparktask.png" rel="attachment wp-att-1236"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/sparktask.png" alt="sparktask" width="717" height="41" class="alignnone size-full wp-image-1236" /></a>
    *   From the DCOS CLI: `dcos package list`
    *   From the Mesos web interface at `http://<hostname>/mesos`, verify that the Spark framework has registered and is starting tasks. There should be several journalnodes, namenodes, and datanodes running as tasks. Wait for all of these to show the RUNNING state.

3.  To run a job with the main method in a class called `MySampleClass` and arguments 10:
    
        $ dcos spark run --submit-args=’--class MySampleClass http://external.website/mysparkapp.jar 10’
        
    
    You can also upload jar files and reference those jars in your job. For example:
    
        $ dcos spark run --submit-args='-Dspark.mesos.coarse=true --driver-cores 1 --driver-memory 1024M --class org.apache.spark.examples.SparkPi https://downloads.mesosphere.com/spark/assets/spark-examples_2.10-1.4.0-SNAPSHOT.jar 30'
        

4.  View the Spark scheduler progress by navigating to the Spark web interface as shown with this command:
    
        $ dcos spark webui
        

5.  Set the `--supervise` flag when running the job to allow the scheduler to restart the job anytime the job finishes.
    
        $ dcos spark run --submit-args=’--supervise --class MySampleClass http://external.website/mysparkapp.jar 10’
        

6.  You can set the amount of cores and memory that the driver will require to be scheduled by passing in `--driver-cores` and `--driver-memory` flags. Any options that the usual spark-submit script accepts are also accepted by DCOS Spark CLI.

7.  To set any Spark properties (e.g. coarse grain mode or fine grain mode), you can also provide a custom `spark.properties` file and set the environment variable `SPARK_CONF_DIR` to that directory.

 [1]: ../install/awscluster/
 [2]: ../install/cli/