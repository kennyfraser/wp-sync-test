---
ID: 3315
post_title: DCOS 1.5
post_date: 2016-02-11 16:56:25
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheon.io/administration/release-notes/community-edition/dcos-1-5/
published: true
post_parent: 959
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The release notes provide a list of useful topics and links for Mesosphere DCOS.

# <a name="mesos"></a>Mesos upgrade

*   The Apache Mesos kernel is now at [version 0.26.0][1]. 
    *   Mesos is now more stable than ever, with more than 80 bug fixes added!

# <a name="known-issues"></a>Known Issues and Limitations

*   See additional known issues at <a href="https://support.mesosphere.com" target="_blank">support.mesosphere.com</a>.

*   The Service and Agent panels of the DCOS Web Interface won't render over 5,000 tasks. If you have a service or agent that has over 5,000 your browser may experience slowness. In this case you can close said browser tab and reopen the DCOS web interface.

 [1]: https://git-wip-us.apache.org/repos/asf?p=mesos.git;a=blob_plain;f=CHANGELOG;hb=0.26.0