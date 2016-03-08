---
ID: 1709
post_title: Customizing a DCOS Service
post_date: 2015-12-29 14:55:43
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/manage-service/service-tutorials/install-service-with-a-custom-options-file/
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
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