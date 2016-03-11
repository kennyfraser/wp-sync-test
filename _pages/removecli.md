---
ID: 520
post_title: Uninstalling the CLI
author: Joel Hamill
post_date: 2016-03-08 14:03:41
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/uninstalling-the-cli/
published: true
import_src:
  - mesosphere-docs/install/removecli.md
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
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3defaa4af
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "101"
---
1.  Delete your DCOS CLI installation directory.

2.  Delete the hidden `.dcos` directory. This will delete the configuration files for your DCOS services. By default, this directory is located in your home directory. For example, `/Users/<your username>/.dcos/` on OS X or `C:Users<your username>.dcos` on Windows.