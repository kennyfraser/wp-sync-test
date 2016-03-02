---
ID: 522
post_title: Updating the CLI
author: Joel Hamill
post_date: 2015-12-08 08:56:36
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/administration/introcli/updatecli/
published: true
import_src:
  - mesosphere-docs/install/updatecli.md
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: false
page_options_show_link_unauthenticated: false
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
        

2.  Install the legacy version of the DCOS CLI, where <public-master-ip> is the public IP of your master node:
    
        mkdir -p dcos && cd dcos && 
          curl -O https://downloads.mesosphere.com/dcos-cli/install-legacy.sh && 
          bash ./install-legacy.sh . <public-master-ip> && 
          source ./bin/env-setup