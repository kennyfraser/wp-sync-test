---
ID: 583
post_title: Uninstalling the CLI
post_date: 2016-02-24 14:49:28
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/installing-enterprise-edition-1-4-2/uninstalling-the-cli-2/
published: true
post_parent: 533
menu_order: 101
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
1.  Delete your DCOS CLI installation directory.

2.  Delete the hidden `.dcos` directory. This will delete the configuration files for your DCOS services. By default, this directory is located in your home directory. For example, `/Users/<your username>/.dcos/` on OS X or `C:Users<your username>.dcos` on Windows.