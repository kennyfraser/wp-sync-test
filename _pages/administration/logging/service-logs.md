---
ID: 528
post_title: Service Logging
author: Joel Hamill
post_date: 2016-03-08 14:03:43
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/administration/logging/service-logging/
published: true
import_src:
  - mesosphere-docs/logging/service-logs.md
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
  - 56df3def7af97
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "2"
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