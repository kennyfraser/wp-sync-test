---
ID: 554
post_title: Using the Package Repository
post_date: 2015-12-08 08:57:29
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheon.io/administration/universe/findservice/
published: true
menu_order: 21
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
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