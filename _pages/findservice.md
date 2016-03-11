---
ID: 554
post_title: Using the Package Repository
author: Joel Hamill
post_date: 2016-03-08 14:03:51
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/using-the-package-repository/
published: true
import_src:
  - mesosphere-docs/services/findservice.md
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
  - 56df3def1ec46
page_options_show_link_unauthenticated:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
menu_order:
  - "21"
---
# Search the package repository

You can use the DCOS CLI to find DCOS services to install.

1.  First, update the local cache of the package repository to make sure you have the latest available package version:
    
        $ dcos package update
        Updating source [https://github.com/mesosphere/universe/archive/version-1.x.zip]
        

2.  Then, use the `dcos package search` command to find available big data packages:
    
        $ dcos package search "big data"
        NAME VERSION FRAMEWORK SOURCE DESCRIPTION  
        spark 1.4.0-SNAPSHOT True https://github.com/mesosphere/universe/archive/version-1.x.zip Spark is a fast and general cluster computing system for Big Data
        

# Add a repository to the package source list

In this example, a package repository is added to the beginning of the `packages.sources` list:

    $ dcos config prepend package.sources https://yourcompany/archive/stuff.zip
    $ dcos config show
    dcos config show
    core.dcos_url=<hostname>
    core.email=youremail@email.com
    core.reporting=True
    core.token=375b30cd25c7c1f7952066098a8860c6d9d52e04e87d4dbff3ee1ea8b8fdac80
    package.cache=/Users/username/.dcos/cache
    package.sources=['https://yourcompany/archive/stuff.zip', 'https://github.com/mesosphere/universe/archive/master.zip']
    

# Remove an item from the package source

In this example, the first item is removed from the `package.sources` parameter:

    $ dcos config unset package.sources --index=0