---
ID: 3315
post_title: DCOS 1.5
author: Joel Hamill
post_date: 2016-03-08 14:03:46
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/dcos-1-5/
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
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3debadcda
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "100"
---
The release notes provide a list of useful topics and links for Mesosphere DCOS.

# <a name="mesos"></a>Mesos upgrade

*   The Apache Mesos kernel is now at [version 0.26.0][1]. 
    *   Mesos is now more stable than ever, with more than 80 bug fixes added!

# <a name="known-issues"></a>Known Issues and Limitations

*   See additional known issues at <a href="https://support.mesosphere.com" target="_blank">support.mesosphere.com</a>.

*   The Service and Agent panels of the DCOS Web Interface won't render over 5,000 tasks. If you have a service or agent that has over 5,000 your browser may experience slowness. In this case you can close said browser tab and reopen the DCOS web interface.

 [1]: https://git-wip-us.apache.org/repos/asf?p=mesos.git;a=blob_plain;f=CHANGELOG;hb=0.26.0