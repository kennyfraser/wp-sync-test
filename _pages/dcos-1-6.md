---
ID: 91
post_title: DCOS 1.6
author: gitsync
post_date: 2016-02-22 18:13:26
post_excerpt: ""
layout: page
permalink: https://gitsync.mmdev2.ca/dcos-1-6/
published: true
header_0_background:
  - 'a:1:{i:0;s:4:"fill";}'
header_0_background_fill_style:
  - 'a:1:{i:0;s:4:"dark";}'
header_0_logo_style:
  - 'a:1:{i:0;s:11:"color-light";}'
header_0_navigation_style:
  - 'a:1:{i:0;s:5:"light";}'
header:
  - 'a:1:{i:0;s:1:"1";}'
page_header_0_show_page_header:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_fill_screen:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_background:
  - 'a:1:{i:0;s:11:"transparent";}'
page_header_0_show_background_image:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_background_video:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_headline:
  - 'a:1:{i:0;s:0:"";}'
page_header_0_headline_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_description:
  - 'a:1:{i:0;s:0:"";}'
page_header_0_description_size:
  - 'a:1:{i:0;s:7:"default";}'
page_header_0_show_image:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_content_alignment:
  - 'a:1:{i:0;s:6:"center";}'
page_header_0_content_style:
  - 'a:1:{i:0;s:4:"dark";}'
page_header_0_actions:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_actions_footnote:
  - 'a:1:{i:0;s:1:"0";}'
page_header_0_show_video:
  - 'a:1:{i:0;s:1:"0";}'
page_header:
  - 'a:1:{i:0;s:1:"1";}'
page_options_require_authentication:
  - 'a:1:{i:0;s:0:"";}'
hide_from_navigation:
  - 'a:1:{i:0;s:1:"0";}'
hide_from_related:
  - 'a:1:{i:0;s:1:"1";}'
---
The release notes provide a list of useful topics and links for Mesosphere DCOS.

# DCOS Access Control Service

You can now enable authorization and authentication in your datacenter.

*   Provides an HTTP API for managing users, groups, and access control lists.
*   Provides an interface for authentication and authorization.
*   Uses a highly available persistence layer.
*   Provides a login endpoint that allows for authentication against a DCOS-internal set of users or against a remote directory service via LDAP 3 (RFC 4510).

For more information, see the [documentation][1].

# <a name="dcos"></a>Improved DCOS installation

**Automated DCOS Installer**

New automated DCOS installer that includes a GUI or command line invocation.

*   Automated GUI installer provides a graphical user interface that guides you through the installation of DCOS Enterprise Edition. With the GUI installer you can interactively install DCOS to a cluster, using a minimal configuration set. This minimal configuration set includes Zookeeper as the Exhibitor shared storage and a static master list (known master IP addresses, not behind a VIP). For more information, see the [documentation][2].
*   Automated command line installer provides a guided installation of DCOS Enterprise Edition with the full configuration set available. For more information, see the [documentation][3].

**Ease of installation**

*   A flattened configuration file that no longer includes nested hashes of `cluster_config` and `ssh_config`. All the parameters for your cluster go in one place, at one level.

**New Configuration Parameters**

*   `superuser_username` and `superuser_password_hash` are new configuration parameters for authentication to the DCOS. For more information, see the [documentation][4].

# DCOS Web Interface

**Integration with DCOS access control service**

*   Access to the DCOS web interface now requires login.
*   Superusers can manage users and groups from a corresponding control panel.
*   Superusers can control user or group access to services by adding them to the corresponding access control lists.
*   Superusers can configure a directory (LDAP) back-end for other users to authenticate against.
*   For more information, see the [documentation][5].

**Mesos UI capabilities now in the DCOS web interface** You can view cluster task information that was previously only available in the Mesos interface.

*   **File browser** You can now browse and download files in Task sandboxes formerly found in the Mesos UI. Files can be downloaded by using the DCOS web interface.
*   **Log Viewer** You can now view live logs for stderr and stdout in the DCOS web interface.

**Fixes and improvements**

*   Table scrolling issue is fixed.
*   Modal sizing, resizing, and scrolling issues are fixed.
*   The Intercom button and DCOS Tour buttons are now optional in web interface.
*   Issue with graphs showing NaN is fixed.
*   Issue with no stroke on graphs when at 0% is fixed.

# DCOS Command Line Interface (CLI)

Added support for authentication against DCOS access control service.

# DCOS Marathon Upgrade

DCOS 1.6 now includes Marathon 0.15.1 with many UI enhancements.

<a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/mara-relnotes-1-6.png" rel="attachment wp-att-3392"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/mara-relnotes-1-6-800x443.png" alt="mara-relnotes-1-6" width="800" height="443" class="alignnone size-large wp-image-3392" /></a>

*   **Perform actions directly from the Applications list** You can now perform common Marathon functions, including scale, destroy, and suspend, directly from a contextual dropdown menu in the Applications list. You no longer have to click through to the application detail view. Additionally, you can perform scale and delete operations on entire Groups!

*   **Better feedback** The feedback dialogs have been completely redesigned for clarity and usability. Color-coded severity levels are now shown: info, warning and error. The action button labels have been rephrased for improved usability. Buttons that can lead to dangerous actions, such as "force scale" are no longer preselected by default.

*   **Application Health** The health status details are now shown in the application details page.

# Advanced AWS Templates

The advanced DCOS AWS CloudFormation Templates extend the basic DCOS template functionality by adding the ability to instantiate a complete DCOS cluster on an existing VPC/Subnet combination, extend/update an existing DCOS cluster by adding more agent nodes, or create a new DCOS cluster that consists of 1-7 masters and unlimited agent nodes.

*   New AWS Cloudformation template types available: Infra, Master, Private Agent, and Public Agent
*   Supports deployment to customer provided VPC
*   Instance Type can be specified for Master and Agent templates
*   Composable: template deployments can be deployed and combined with each other in a pluggable fashion, for multiple agent auto-scale groups, multiple DCOS clusters within the same VPC, etc.

# <a name="mesos"></a>DCOS Mesos Upgrade

The Apache Mesos kernel is now at [version 0.27][6].

*   [MESOS-1791][7]: Support for resource quota that provides non-revocable resource guarantees without tying reservations to particular Mesos agents. Please refer to the quota documentation for more information. 
*   [MESOS-191][8]: Multiple disk support to allow for disk IO intensive applications to achieve reliable, high performance. 
*   [MESOS-4085][9]: Flexible roles with the introduction of implicit roles. It deprecates the whitelist functionality that was implemented by specifying --roles during master startup to provide a static list of roles. 
*   [MESOS-2353][10]: Performance improvement of the state endpoint for large clusters. 
*   Furthermore, 167+ bugfixes and improvements made it into this release. For full release notes with all features and bug fixes, please refer to the [CHANGELOG][11].

# <a name="known-issues"></a>Known Issues and Limitations

*   The Service and Agent panels of the DCOS Web Interface won't render over 5,000 tasks. If you have a service or agent that has over 5,000 your browser may experience slowness. In this case you can close said browser tab and reopen the DCOS web interface.
*   See additional known issues at <a href="https://support.mesosphere.com" target="_blank">support.mesosphere.com</a>.

 [1]: ../security-and-authentication/
 [2]: ../automated-gui/
 [3]: ../auto-command-line/
 [4]: ../auto-command-line/#scrollNav-4
 [5]: ../security-and-authentication/managing-authorization/
 [6]: http://mesos.apache.org/blog/mesos-0-27-0-released/
 [7]: https://issues.apache.org/jira/browse/MESOS-1791
 [8]: https://issues.apache.org/jira/browse/MESOS-191
 [9]: https://issues.apache.org/jira/browse/MESOS-4085
 [10]: https://issues.apache.org/jira/browse/MESOS-2353
 [11]: https://git-wip-us.apache.org/repos/asf?p=mesos.git;a=blob_plain;f=CHANGELOG;hb=0.27.0