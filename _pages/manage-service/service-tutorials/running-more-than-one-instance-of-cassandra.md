---
UID: 56df37499552b
post_title: Running multiple Cassandra instances
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
In this tutorial, an additional instance of the DCOS Cassandra service is installed.

For each new instance, you must provide a unique Cassandra instance name in an `options.json` file and specify this file during Cassandra installation.

By default, the DCOS Cassandra service creates an instance named `cassandra`. Additional instance names are created as `cassandra.<new-user>`.

**Prerequisites:**

*   [DCOS is installed][1]
*   Sufficient resources to add more Cassandra hosts

1.  Create an `options.json` file and specify the new Cassandra instance as `user3`. This new Cassandra instance is named: `cassandra.user3`.
    
        {
          "cassandra": {
            "cluster-name": "user3"
          }
        }
        

2.  From the DCOS CLI, run this command to install Cassandra with the options file specified:
    
        dcos package install cassandra --options=options.json
        

3.  Verify that Cassandra is successfully installed:
    
    *   From the DCOS CLI: `dcos package list`
    *   From the Mesosphere DCOS web interface, go to the Services tab and confirm that the new Cassandra instance is running. <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandrauser3.png" rel="attachment wp-att-1282"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/cassandrauser3.png" alt="cassandrauser3" width="669" height="48" class="alignnone size-full wp-image-1282" /></a> 
    *   From the Mesos web interface at `<hostname>/mesos`, verify that the Cassandra framework has registered and is starting tasks.
        
        **Tip:** It can take up to 6 minutes for the Cassandra ring to bootstrap - during this time, the DCOS web interface may show the service as Unhealthy. There should be 6 tasks running with the following IDs:
    
    cassandra.dcos.node.0.executor cassandra.dcos.node.0.executor.server cassandra.dcos.node.1.executor cassandra.dcos.node.1.executor.server cassandra.dcos.node.2.executor cassandra.dcos.node.2.executor.server
    
    *   To run advanced Cassandra jobs, you must SSH into your cluster. For more information on SSHing into your cluster, see \[Establishing an SSH Connection\]\[5\].
        
        For example, to view several system tables in the Cassandra servers, from any node in your cluster, run the following command. Because this example downloads a Docker file, it may take a while.
        
        $ docker run -i -t --net=host --entrypoint=/usr/bin/cqlsh spotify/cassandra -e " SELECT * FROM system.schema_keyspaces ; SELECT * FROM system.schema_columns ; SELECT * FROM system.schema_columnfamilies ;" cassandra-dcos-node.cassandra.dcos.mesos 9160

4.  Access the Cassandra cluster by using the DNS name created by Mesos-DNS: `cassandra-user3-node.cassandra.user3.mesos`.
    
    For more information on Mesos-DNS naming, see [Service Naming][2].

 [1]: ../administering/installing/
 [2]: https://docs.mesosphere.com/administration/service-discovery/service-naming/