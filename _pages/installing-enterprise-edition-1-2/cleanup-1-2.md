---
ID: 2332
post_title: DCOS cleanup script (1.2)
post_date: 2016-01-12 16:04:47
post_excerpt: ""
layout: page
permalink: >
  https://test-mesosphere-documentation.pantheon.io/installing-enterprise-edition-1-2/cleanup-1-2/
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
Sometimes, you can get your hosts into a bad spot and want to start over. Instead of creating brand new instances, you can run this cleanup script and get rid of all the <span class="caps">DCOS</span> specific things.

To run the <span class="caps">DCOS</span> cleanup script:

    $ rm -rf /opt/mesosphere /etc/systemd/system/dcos.target.wants