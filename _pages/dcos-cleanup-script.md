---
ID: 2302
post_title: DCOS cleanup script
author: Suzanna Scala
post_date: 2016-01-12 15:34:43
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/
published: true
page_options_topic_page:
  - ""
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: false
page_options_show_link_unauthenticated: false
---
Sometimes, you can get your hosts into a bad spot and want to start over. Instead of creating brand new instances, you can run this cleanup script and get rid of all the DCOS specific things.

To run the DCOS cleanup script:

    $ sudo -i /opt/mesosphere/bin/pkgpanda uninstall
    $ sudo rm -rf /opt/mesosphere