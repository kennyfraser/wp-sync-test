---
ID: 1064
post_title: Configuring the CLI to use an HTTP Proxy
author: Joel Hamill
post_date: 2016-03-08 14:03:39
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/configuring-the-cli-to-use-an-http-proxy/
published: true
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
  - 56df3dee295e7
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "15"
---
If you use a proxy server to connect to the Internet, you can configure the CLI to use your proxy server.

**Prerequisites**

*   pip version 7.1.0 or greater 
*   The `http_proxy` and `https_proxy` environment variables are defined to use pip.

To configure a proxy for the CLI:

1.  From the CLI terminal, define the environment variables `http_proxy` and `https_proxy`:
    
        $ export http_proxy=’<http://your/proxy/here/>’
        $ export https_proxy=’<http://your/proxy/here/>’
        

2.  Define `no_proxy` for domains that you don’t want to use the proxy for:
    
        $ export no_proxy="127.0.0.1, localhost”