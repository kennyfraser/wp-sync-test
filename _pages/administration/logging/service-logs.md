---
ID: 528
post_title: Service Logging
post_date: 2015-12-08 08:56:44
post_excerpt: ""
layout: page
permalink: >
  http://local.mesodocs.com/administration/logging/service-logs/
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The DCOS services run as Mesos tasks and print their logs to `stdout` and `stderr`. The log content varies from service to service, but usually includes task launches, resource accepts, and resource rejects.

You can access these logs in two ways:

*   The `dcos service log` command. For example, enter this command to view the Chronos logs:
    
          dcos service log chronos
        
    
    For more information, see [dcos service log][1].

*   The Mesos web interface:
    
    1.  Go to the Mesos web interface at `<hostname>/mesos`, where `<hostname>` is the [Mesos Master hostname][2].
    
    2.  Find the row for your desired service and click the **Sandbox** link to view or download the logs.
        
        **Tip:** You can use the filters to narrow the scope.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/mesos-sandbox-log.png" rel="attachment wp-att-1559"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/mesos-sandbox-log.png" alt="mesos-sandbox-log" width="898" height="311" class="alignnone size-full wp-image-1559" /></a>

 [1]: ../introcli/command-reference/#scrollNav-5
 [2]: /install/awscluster#launchdcos