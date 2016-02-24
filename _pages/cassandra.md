---
ID: 552
post_title: Cassandra
post_date: 2015-12-08 08:57:28
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheon.io/manage-service/cassandra/
published: true
post_parent: 69
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
**Disclaimer:** This service is available at the alpha level and not recommended for Mesosphere DCOS production systems.

Cassandra is an open source distributed database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure. Cassandra is well suited to run on Mesos due to its peer-to-peer architecture. Cassandra has great features that back today’s web applications with its horizontal scalability, no single point of failure, and a simple query language (CQL).

For information about how Mesos DNS is implemented for the Cassandra DCOS service, see the <a href="http://mesosphere.github.io/cassandra-mesos/docs/mesos-dns.html" target="_blank">Cassandra-Mesos documentation</a>.

# <a name="cassandrainstall"></a>Installing Cassandra on DCOS

**Prerequisite**

*   The DCOS CLI must be [installed][1].
*   A DCOS cluster with at least 3 private nodes, each with 0.3 CPU shares, 1184MB of memory and 272MB of disk.

1.  From the DCOS CLI, enter this command:
    
        $ dcos package install cassandra
        

2.  Verify that Cassandra is successfully installed:
    
    *   From the DCOS CLI: `dcos package list`
    *   From the Mesosphere DCOS web interface, go to the Services tab and confirm that Cassandra running. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandratask.png" rel="attachment wp-att-1326"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandratask.png" alt="cassandratask" width="713" height="45" class="alignnone size-full wp-image-1326" /></a> 
    *   From the Mesos web interface at `<hostname>/mesos`, verify that the Cassandra service has registered and is starting tasks. 
    
    **Tip:** It can take up to 6 minutes for the Cassandra ring to bootstrap - during this time, the DCOS web interface may show the service as Unhealthy. There should be 6 tasks running with the following IDs:
    
        cassandra.dcos.node.0.executor
        cassandra.dcos.node.0.executor.server
        cassandra.dcos.node.1.executor
        cassandra.dcos.node.1.executor.server
        cassandra.dcos.node.2.executor
        cassandra.dcos.node.2.executor.server
        

3.  To run advanced Cassandra jobs, you must SSH into your cluster. For more information on SSHing into your cluster, see [Establishing an SSH Connection][2].
    
    For example, to view several system tables in the Cassandra servers, from any node in your cluster, run the following command. Because this example downloads a Docker file, it may take a while.
    
        $ docker run -i -t --net=host 
            --entrypoint=/usr/bin/cqlsh 
            spotify/cassandra -e "
               SELECT * FROM system.schema_keyspaces ;
               SELECT * FROM system.schema_columns ;
               SELECT * FROM system.schema_columnfamilies ;" 
            cassandra-dcos-node.cassandra.dcos.mesos 9160
        

# <a name="uninstall"></a>Uninstalling Cassandra

1.  From the DCOS CLI, enter this command:
    
        $ dcos package uninstall cassandra
        

2.  Open the Zookeeper Exhibitor web interface at `<hostname>/exhibitor`, where `<hostname>` is the [Mesos Master hostname][3].
    
    1.  Click on the **Explorer** tab and navigate to the `cassandra-mesos` folder.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandra.png" rel="attachment wp-att-1329"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandra-150x150.png" alt="zkcassandra" width="150" height="150" class="alignnone size-thumbnail wp-image-1329" /></a>
    
    2.  Click the **Modify** button on the lower-left side of browser.
    
    3.  Choose Type **Delete**, enter the required **Username**, **Ticket/Code**, and **Reason** fields, and click **Next**.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandradelete.png" rel="attachment wp-att-1332"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/zkcassandradelete-600x334.png" alt="zkcassandradelete" width="300" height="167" class="alignnone size-medium wp-image-1332" /></a>
    
    4.  Click **OK** to confirm your deletion.

3.  Clear your data directories. By default the Cassandra DCOS Service data and log directories are written into the Mesos task sandbox. You can change this by setting the `cassandra.data-directory` option when you install the Cassandra DCOS Service.

For more information:

*   <a href="http://mesosphere.github.io/cassandra-mesos/" target="_blank">Cassandra Mesos documentation</a>
*   [Deploying a Containerized App on a Public Node][4]

 [1]: /install/cli/
 [2]: /administration/sshcluster/
 [3]: /install/awscluster#launchdcos
 [4]: ../getting-started/tutorials/deploy-containerized-app/