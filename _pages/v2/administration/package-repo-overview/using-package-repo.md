---
post_title: Using the Package Repository
post_excerpt: ""
layout: page
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

In this example, a package repository named `your-repo` is added with a URL of `https://yourcompany/archive/stuff.zip`:

    $ dcos package repo add your-repo https://yourcompany/archive/stuff.zip


Here is what the repository list now looks like:

    $ dcos package repo list
    Universe: https://universe.mesosphere.com/repo
    your-repo: https://yourcompany/archive/stuff.zip


# Remove an item from the package source

In this example, a repository is removed:

    $ dcos package repo remove your-repo


Here is what the repository list now looks like:

    $ dcos package repo list
    Universe: https://universe.mesosphere.com/repo