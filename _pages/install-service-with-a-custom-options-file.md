---
ID: 1709
post_title: Customizing a DCOS Service
author: Joel Hamill
post_date: 2016-03-08 14:04:16
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/customizing-a-dcos-service/
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
  - 56df3ded97f67
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "100"
---
You can customize your DCOS service during installation with a JSON configuration file. Otherwise, the services are installed by using default values.

The general process is as follows:

1.  View the available configuration options with the `dcos package describe --config <package-name>` command:
    
        $ dcos package describe --config marathon
        {
         "properties": {
            "marathon": {
              "cpus": {
                "default": 2.0,
                "description": "CPU shares to allocate to each Marathon instance.",
                "minimum": 0.0,
                "type": "number"
             },
            ...        
            "mem": {
              "default": 1024.0,
              "description": "Memory (MB) to allocate to each Marathon task.",
              "minimum": 512.0,
              "type": "number"
             },
             ...
        }
        

2.  Create a JSON configuration file. You can choose an arbitrary name, but you might want to choose a pattern like `<package-name>-config.json`. For example, `marathon-config.json`.
    
        $ nano marathon-config.json
        

3.  Add an entry to a JSON options file. For example, to change the number of Marathon CPU shares to 3 and memory allocation to 2048:
    
        {
          "marathon": { 
            "cpus": 3.0, "mem": 2048.0 
           } 
        }
        

4.  From the DCOS CLI, install the DCOS service with the custom options file specified:
    
        $ dcos package install --options=marathon-config.json marathon
        

For more information, see the [dcos package][1] documentation.

 [1]: ../administration/introcli/command-reference/#scrollNav-4