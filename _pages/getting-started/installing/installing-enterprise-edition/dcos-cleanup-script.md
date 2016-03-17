---
UID: 56df3ded44bb6
post_title: DCOS cleanup script
post_excerpt: ""
layout: page
published: true
menu_order: 204
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Sometimes, you can get your hosts into a bad spot and want to start over. Instead of creating brand new instances, you can run this cleanup script and get rid of all the DCOS specific things.</p>

<p>To run the DCOS cleanup script:</p>

<pre><code>$ sudo -i /opt/mesosphere/bin/pkgpanda uninstall
$ sudo rm -rf /opt/mesosphere
</code></pre>