---
ID: 3386
post_title: DCOS 1.6
author: Joel Hamill
post_date: 2016-02-17 10:25:59
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/administration/release-notes/enterprise-edition/dcos-1-6/
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
  - "1"
---
The release notes provide a list of useful topics and links for Mesosphere DCOS.

# <a name="dcos"></a>Simplified DCOS installation

*   

# DCOS Web Interface

**Integration with DCOS access control service**

*   Access to the DCOS web interface now requires login.
*   Superusers can manage users and groups from a corresponding control panel.
*   Superusers can control user or group access to services by adding them to the corresponding access control lists.
*   Superusers can configure a directory (LDAP) back-end for other users to authenticate against.

**Mesos UI capabilities now in the DCOS web interface** You can now view cluster task information that was previously only available in the Mesos interface.

*   **File browser** You can now browse and download files in Task sandboxes formerly found in the Mesos UI. Files can be downloaded via DCOS UI.
*   **Log Viewer** You can now view live logs for stderr and stdout in the DCOS web interface.

**Fixes and improvements**

*   Table scrolling issue is fixed.
*   Modal sizing, resizing, and scrolling issues are fixed.
*   The Intercom button and DCOS Tour buttons are now optional in web interface.
*   Issue with graphs showing NaN is fixed.
*   Issue with no stroke on graphs when at 0% is fixed.

# DCOS Marathon Upgrade

DCOS 1.6 now includes Marathon 0.15.1 with many UI enhancements.

<a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/mara-relnotes-1-6.png" rel="attachment wp-att-3392"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/mara-relnotes-1-6-800x443.png" alt="mara-relnotes-1-6" width="800" height="443" class="alignnone size-large wp-image-3392" /></a>

*   **Perform actions directly from the Applications list** You can now perform common Marathon functions, including scale, destroy, and suspend, directly from a contextual dropdown menu in the Applications list. You no longer have to click through to the application detail view. Additionally, you can perform scale and delete operations on entire Groups!

*   **Better feedback** The feedback dialogs have been completely redesigned for clarity and usability. Color-coded severity levels are now shown: info, warning and error. The action button labels have been rephrased for improved usability. Buttons that can lead to dangerous actions, such as "force scale" are no longer preselected by default.

*   **Application Health** The health status breakdown is now shown in the application details page.

# <a name="mesos"></a>DCOS Mesos Upgrade

The Apache Mesos kernel is now at [version 0.27][1].

*   [MESOS-1791][2]: Support for resource quota that provides non-revocable resource guarantees without tying reservations to particular Mesos agents. Please refer to the quota documentation for more information. 
*   [MESOS-191][3]: Multiple disk support to allow for disk IO intensive applications to achieve reliable, high performance. 
*   [MESOS-4085][4]: Flexible roles with the introduction of implicit roles. It deprecates the whitelist functionality that was implemented by specifying --roles during master startup to provide a static list of roles. 
*   [MESOS-2353][5]: Performance improvement of the state endpoint for large clusters. 
*   Furthermore, 167+ bugfixes and improvements made it into this release. For full release notes with all features and bug fixes, please refer to the [CHANGELOG][6].

# <a name="known-issues"></a>Known Issues and Limitations

*   See additional known issues at <a href="https://support.mesosphere.com" target="_blank">support.mesosphere.com</a>.

*   The Service and Agent panels of the DCOS Web Interface won't render over 5,000 tasks. If you have a service or agent that has over 5,000 your browser may experience slowness. In this case you can close said browser tab and reopen the DCOS web interface.

 [1]: http://mesos.apache.org/blog/mesos-0-27-0-released/
 [2]: https://issues.apache.org/jira/browse/MESOS-1791
 [3]: https://issues.apache.org/jira/browse/MESOS-191
 [4]: https://issues.apache.org/jira/browse/MESOS-4085
 [5]: https://issues.apache.org/jira/browse/MESOS-2353
 [6]: https://git-wip-us.apache.org/repos/asf?p=mesos.git;a=blob_plain;f=CHANGELOG;hb=0.27.0