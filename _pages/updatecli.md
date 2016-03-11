---
ID: 522
post_title: Updating the CLI
author: Joel Hamill
post_date: 2016-03-08 14:03:41
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/updating-the-cli/
published: true
import_src:
  - mesosphere-docs/install/updatecli.md
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
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3defa49fb
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "25"
---
You can update the DCOS CLI to the latest version or downgrade to an older version.

# Upgrade the CLI

You can upgrade an existing DCOS CLI installation to the latest build.

1.  From your DCOS CLI installation directory, enter this command to update the DCOS CLI:
    
        $ sudo pip install -U dcoscli
        

# Downgrade the CLI

You can downgrade an existing DCOS CLI installation to an older version.

**Tip:** Downgrading is necessary if you are running an older version of DCOS and want to reinstall the DCOS CLI.

1.  Delete your DCOS CLI installation directories:
    
        $ sudo rm -rf dcos && rm -rf ~/.dcos
        

2.  Install the legacy version of the DCOS CLI, where is the public IP of your master node:
    
        mkdir -p dcos && cd dcos && 
          curl -O https://downloads.mesosphere.com/dcos-cli/install-legacy.sh && 
          bash ./install-legacy.sh . <public-master-ip> && 
          source ./bin/env-setup