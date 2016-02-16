---
ID: 3361
post_title: Using the DCOS Installer
author: Joel Hamill
post_date: 2016-02-16 15:46:35
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/basic-installation/
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
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "0"
---
You can quickly get up and running with DCOS Enterprise Edition by using the GUI installer.

**Prerequisite:** If your SSH key has a passphrase, you must decrypt your SSH key before installing DCOS. For information on how to decrypt your SSH key, see [these instructions][1].

1.  Download and save the DCOS setup file, `dcos_generate_config.ee.sh`, to the `dcos` directory on your bootstrap node. This file is used to create your customized DCOS build file.
    
    **Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

2.  From your terminal, start the DCOS installer with this command.
    
        $ sudo bash dcos_generate_config.ee.sh --web
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        16:36:09 dcos_installer.action_lib.prettyprint:: ====> Starting DCOS installer in web mode
        16:36:09 root:: Starting server ('0.0.0.0', 9000)
        
    
    You can add the verbose (`-v`) flag to see the debug output:
    
        $ sudo bash dcos_generate_config.ee.sh --web -v
        

3.  Launch the DCOS web installer in your browser at: `http://<public-ip>:9000`.

4.  Click **Begin Installation**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin.png" rel="attachment wp-att-3190"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin-800x510.png" alt="ui-installer-begin" width="800" height="510" class="alignnone size-large wp-image-3190" /></a>

5.  Specify your Deployment and DCOS Environment settings:
    
    ### Deployment Settings
    
    **Master IP Address List**
    :   Specify a comma-separated list of your static master IP addresses.
    
    **Agent IP Address List**
    :   Specify a comma-separated list of your static agent IP addresses.
    
    **Public IP Address**
    :   Specify the IP address to a publicly accessible proxy to one of your master nodes. If you either don't have a proxy or you already have access to the network where you are deploying this cluster, you can use one of the master IP's that you specified in the master list. This proxy IP address is used to access the DCOS web interface after the deployment succeeds.
    
    **SSH Username**
    :   Specify the SSH username, for example `centos`. SSH Listening Port
    
    **SSH Listening Port**
    :   Specify the port to SSH to, for example `22`. SSH Key
    
    **SSH Key**
    :   Specify the SSH key with access to your master IPs.
    
    ### DCOS Environment Settings
    
    **Username**
    :   Specify the administrator username. This username is required for using DCOS.
    
    **Password**
    :   Specify the administrator password. This password is required for using DCOS.
    
    **Bootstrapping Zookeeper IP Address(es)**
    :   Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it’s configuration.
    
    **Bootstrapping Zookeeper Port**
    :   Specify the Zookeeper port. For example, `2181`.
    
    **Upstream DNS Servers**
    :   Specify the DNS servers, which can be on your private network or the public internet. Caution: If you set this parameter incorrectly you will have to reinstall DCOS. For more information about service discovery, see this [documentation][2].
    
    **IP Detect Script**
    :   Specify an IP detect script to broadcast the IP address of each node across the cluster. For more information, see the [documentation][3].

6.  Click **Run Pre-Flight**. The preflight script validates that your cluster is installable. This step can take up to 15 minutes to complete. If errors any errors are found, fix and then click **Retry**.
    
    **Important:** If you exit your GUI installation before launching DCOS, you must do this before reinstalling:
    
    *   SSH to each node in your cluster and run `rm -rf /opt/mesosphere`.
    *   SSH to your bootstrap master node and run `rm -rf /var/lib/zookeeper`
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" rel="attachment wp-att-3197"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" alt="ui-installer-pre-flight1" width="626" height="405" class="alignnone size-full wp-image-3197" /></a>

7.  Click **Deploy** to install DCOS on your cluster. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" rel="attachment wp-att-3195"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" alt="ui-installer-deploy1" width="628" height="406" class="alignnone size-full wp-image-3195" /></a>

8.  Click **Run Post-Flight**. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" rel="attachment wp-att-3196"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" alt="ui-installer-post-flight1" width="623" height="366" class="alignnone size-full wp-image-3196" /></a>
    
    **Tip:** You can click **Download Logs** to view your logs locally.

9.  Click **Log In To DCOS**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a>

10. Enter your administrator username and password.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a>
    
    You are done!
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a>

### Next Steps

Now you can [assign user roles][4].

 [1]: https://techjourney.net/how-to-decrypt-an-enrypted-ssl-rsa-private-key-pem-key/
 [2]: ../installing-enterprise-edition-1-6/#scrollNav-3
 [3]: ../configuration-parameters-1-6/
 [4]: ../security-and-authentication/managing-authorization/