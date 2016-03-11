---
ID: 2558
post_title: 'FAQ &#038; Troubleshooting'
author: Joel Hamill
post_date: 2016-03-08 14:03:49
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/faq-troubleshooting/
published: true
header:
  - "1"
page_header:
  - "1"
page_options_topic_page:
  - ""
page_options_require_authentication:
  - ""
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
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
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3decad6eb
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "4"
---
# How can I check the Mesos-DNS version?

You can check the Mesos-DNS version with `mesos-dns -version`.

**Note:** We do not recommend upgrading Mesos-DNS independently of DCOS. Use the version of Mesos-DNS that shipped with your version of DCOS.

# What if Mesos-DNS fails to launch?

Check that port 53 and port 8123 are available and not in use by other processes.

# What if my Agent nodes cannot connect to Mesos-DNS?

*   Make sure that port 53 is not blocked by a firewall rule on your cluster.

*   It is possible that the Master nodes are not running. Run `sudo systemctl status dcos-mesos-dns` and `sudo journalctl -u dcos-gen-resolvconf.service -n 200 -f` for more information about Mesos-DNS errors.

# How do I configure my DCOS cluster to communicate with external hosts and services?

For DNS requests for hostnames or services outside the DCOS cluster, Mesos-DNS will query an external nameserver. By default, Google's nameserver with IP address 8.8.8.8 will be used. If you need to configure a custom external name server, use the [`resolvers` parameter][1] when you first install DCOS.

**Important:** External nameservers can only be set when you install DCOS. They cannot be changed after installation.

# <a name="leader"></a>What is the difference between leader.mesos and master.mesos?

To query the leading master node, always query `leader.mesos`.

If you try to connect to `master.mesos` using HTTP, you will be automatically redirected to the leading master node.

However, if you try to query or connect to `master.mesos` using any method other than HTTP, the results will be unpredictable because the name will resolve to a random master node. For example, a service that attempts to register with `master.mesos` may communicate with a non-leading master node and will be unable to register as a service on the cluster.

 [1]: https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/#config-json