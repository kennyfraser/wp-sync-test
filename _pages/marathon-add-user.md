---
ID: 580
post_title: Adding a Marathon User Instance
author: Joel Hamill
post_date: 2016-03-08 14:04:16
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/adding-a-marathon-user-instance/
published: true
import_src:
  - >
    mesosphere-docs/tutorials/marathon-add-user.md
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
  - 56df3deebd7cf
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "25"
---
A native Marathon instance is installed as a part of the DCOS installation. This tutorial creates a Marathon instance on top of the native Marathon to create separate user environments.

Prerequisite

:   [Install the DCOS CLI][1]

1.  Create a JSON configuration file, specify `marathon-alice` as the framework name, and save as `newuser.json`:
    
        {"marathon": {"framework-name": "marathon-alice" }}
        
    
    **Tip:** You must create separate JSON configuration files for each Marathon instance.

2.  From the DCOS CLI, enter this command to install the Marathon instance:
    
        $ dcos package install --options=newuser.json marathon
        

3.  From the DCOS web interface **Services** tab, click on the **marathon-alice** service name to navigate to the Marathon web interface.

4.  Optional: You can modify the DCOS CLI configuration to point to the **marathon-alice** instance. This allows you to administer your Marathon instance by using the DCOS CLI.
    
    1.  From the DCOS CLI, set the `marathon.url` property to point to the **marathon-alice** instance, where `<hostname>` is the Marathon web interface hostname:
        
            $ dcos config set marathon.url http://<hostname>/service/marathon-alice/
            
    
    2.  Verify that the the `marathon.url` is set. The `marathon.url` takes precedence over the native Marathon in DCOS.
        
            $ dcos config show
            core.dcos_url=http://nodel-elasticl-1xyz-1940784093.us-west-2.elb.amazonaws.com
            core.email=youremail@email.io
            core.reporting=True
            core.token=a547c734ed81247d0203ce238a5a07ac012b59f8d7f89ed539e5110557548152
            marathon.url=http://alicenodel-elasticl-1xyz-1940784093.us-west-2.elb.amazonaws.com/service/marathon-alice/
            package.cache=/Users/alice/.dcos/cache
            package.sources=['https://github.com/mesosphere/universe/archive/version-1.x.zip']
            
        
        **Tip:** You can switch back to the native Marathon instance by specifying `dcos config unset marathon.url`.

### Next Steps

After you have your Marathon instance up and running, you can try [Deploying a Containerized App on a Public Node][2].

 [1]: /install/cli/
 [2]: ../getting-started/tutorials/deploy-containerized-app/