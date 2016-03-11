---
ID: 2302
post_title: DCOS cleanup script
author: Joel Hamill
post_date: 2016-03-08 14:04:04
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/
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
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3ded44bb6
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "204"
---
Sometimes, you can get your hosts into a bad spot and want to start over. Instead of creating brand new instances, you can run this cleanup script and get rid of all the DCOS specific things.

To run the DCOS cleanup script:

    $ sudo -i /opt/mesosphere/bin/pkgpanda uninstall
    $ sudo rm -rf /opt/mesosphere