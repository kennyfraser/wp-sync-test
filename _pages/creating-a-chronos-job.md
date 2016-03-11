---
ID: 1304
post_title: Creating a Chronos job
author: Joel Hamill
post_date: 2016-03-08 14:04:15
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/creating-a-chronos-job/
published: true
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
  - 56df3dede196e
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "100"
---
In this example, a Chronos job that sleeps for 10 seconds is created.

**Prerequisite:**

*   The DCOS Chronos service is [installed][1]

1.  From the DCOS Services page, click on the **chronos** link to go to the Chronos web interface.

2.  From the Chronos web interface, click **New Job**.

3.  Add the NAME “Test Chronos Sleep”, COMMAND `sleep 10`, your email for OWNER(S), and click **Create**.
    
    <a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosnewjob.png" rel="attachment wp-att-1308"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosnewjob.png" alt="chronosnewjob" width="424" height="432" class="alignnone size-full wp-image-1308" /></a>

4.  After the job is created, you can click on **Force Run** (the ‘play’ button) to run it. You should see it as a running task on your Mesos master and the Sandbox. Once it’s finished running, you should see status “SUCCESS.”
    
    <a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosforcerun.png" rel="attachment wp-att-1310"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosforcerun.png" alt="chronosforcerun" width="315" height="99" class="alignnone size-full wp-image-1310" /></a>

 [1]: ../reference/chronos/#chronosinstall