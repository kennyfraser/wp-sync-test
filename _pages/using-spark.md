---
ID: 1702
post_title: Customizing Your Spark Installation
author: Joel Hamill
post_date: 2016-03-08 14:04:16
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/customizing-your-spark-installation/
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
  - 56df3ded9de0b
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "100"
---
# Installing the Spark app only, not CLI

In this example, the Spark app is installed without the Spark CLI:

    $ dcos package install —app spark
    

# Installing Spark with A Custom Name

In this example, Spark is installed with the explicit application ID `alice-spark` specified:

    $ dcos package install --app-id=alices-spark spark
    Installing package [spark] version [1.4.0-SNAPSHOT] with app id [alices-spark]
    Installing CLI subcommand for package [spark]
    Spark cluster mode on Mesos is ready!