---
ID: 486
post_title: DCOS cleanup script
post_date: 2016-02-24 14:49:24
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/dcos-cleanup-script-2/
published: true
post_parent: 2343
menu_order: 204
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
Sometimes, you can get your hosts into a bad spot and want to start over. Instead of creating brand new instances, you can run this cleanup script and get rid of all the DCOS specific things.

To run the DCOS cleanup script:

    $ sudo -i /opt/mesosphere/bin/pkgpanda uninstall
    $ sudo rm -rf /opt/mesosphere