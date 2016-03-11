---
ID: 1669
post_title: Changing the DCOS master node
author: Joel Hamill
post_date: 2016-03-08 14:03:34
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/changing-the-dcos-master-node/
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
  - 56df3dedb1f76
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "100"
---
In this example, the DCOS cluster Mesos master hostname is changed.

1.  Show the current configuration, where `core.dcos_url` is the hostname of your DCOS installation:
    
        $ dcos config show
        core.dcos_url=http://ab-test-2.us-west-2.elb.cloudhost.com
        core.email=youremail@email.com
        core.reporting=True
        core.token=375b30cd25c7c1f7952066098a8860c6d9d52e04e87d4dbff3ee1ea8b8fdac80
        package.cache=/Users/username/.dcos/cache
        package.sources=['https://github.com/mesosphere/universe/archive/version-1.x.zip']
        

2.  Set the hostname (`core.dcos_url`) to the new DCOS master `http://ab-test-3.us-west-2.elb.cloudhost.com`:
    
        $ dcos config set core.dcos_url http://ab-test-2.us-west-2.elb.cloudhost.com
        

3.  Verify that the hostname value has been updated:
    
        $ dcos config show core.dcos_url
        http://ab-test-3.us-west-2.elb.cloudhost.com