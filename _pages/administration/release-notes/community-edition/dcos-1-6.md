---
UID: 56df3deb543d8
post_title: DCOS 1.6
post_excerpt: ""
layout: page
published: true
menu_order: 110
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The release notes provide a list of useful topics and links for Mesosphere DCOS.</p>

<h1>DCOS Web Interface</h1>

<p><strong>Mesos UI capabilities now in the DCOS web interface</strong> You can now view cluster task information that was previously only available in the Mesos interface.</p>

<ul>
<li><strong>File browser</strong> You can now browse and download files in Task sandboxes formerly found in the Mesos UI. Files can be downloaded by using the DCOS web interface.</li>
<li><strong>Log Viewer</strong> You can now view live logs for stderr and stdout in the DCOS web interface.</li>
</ul>

<p><strong>Fixes and improvements</strong></p>

<ul>
<li>Table scrolling issue is fixed.</li>
<li>Modal sizing, resizing, and scrolling issues are fixed.</li>
<li>The Intercom button and DCOS Tour buttons are now optional in web interface.</li>
<li>Issue with graphs showing NaN is fixed.</li>
<li>Issue with no stroke on graphs when at 0% is fixed.</li>
</ul>

<h1>DCOS Marathon Upgrade</h1>

<p>DCOS 1.6 now includes Marathon 0.15.1 with many UI enhancements.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/mara-relnotes-1-6.png" rel="attachment wp-att-3392"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/mara-relnotes-1-6-800x443.png" alt="mara-relnotes-1-6" width="800" height="443" class="alignnone size-large wp-image-3392" /></a></p>

<ul>
<li><p><strong>Perform actions directly from the Applications list</strong> You can now perform common Marathon functions, including scale, destroy, and suspend, directly from a contextual dropdown menu in the Applications list. You no longer have to click through to the application detail view. Additionally, you can perform scale and delete operations on entire Groups!</p></li>
<li><p><strong>Better feedback</strong> The feedback dialogs have been completely redesigned for clarity and usability. Color-coded severity levels are now shown: info, warning and error. The action button labels have been rephrased for improved usability. Buttons that can lead to dangerous actions, such as "force scale" are no longer preselected by default.</p></li>
<li><p><strong>Application Health</strong> The health status breakdown is now shown in the application details page.</p></li>
</ul>

<h1>Advanced AWS Templates</h1>

<p>The advanced DCOS AWS CloudFormation Templates extend the basic DCOS template functionality by adding the ability to instantiate a complete DCOS cluster on an existing VPC/Subnet combination, extend/update an existing DCOS cluster by adding more agent nodes, or create a new DCOS cluster that consists of 1-7 masters and unlimited agent nodes.</p>

<ul>
<li>New AWS Cloudformation template types available: Infra, Master, Private Agent, and Public Agent</li>
<li>Supports deployment to customer provided VPC</li>
<li>Instance Type can be specified for Master and Agent templates</li>
<li>Composable: template deployments can be deployed and combined with each other in a pluggable fashion, for multiple agent auto-scale groups, multiple DCOS clusters within the same VPC, etc.</li>
</ul>

<h1><a name="mesos"></a>DCOS Mesos Upgrade</h1>

<p>The Apache Mesos kernel is now at <a href="http://mesos.apache.org/blog/mesos-0-27-0-released/">version 0.27</a>.</p>

<ul>
<li><a href="https://issues.apache.org/jira/browse/MESOS-1791">MESOS-1791</a>: Support for resource quota that provides non-revocable resource guarantees without tying reservations to particular Mesos agents. Please refer to the quota documentation for more information. </li>
<li><a href="https://issues.apache.org/jira/browse/MESOS-191">MESOS-191</a>: Multiple disk support to allow for disk IO intensive applications to achieve reliable, high performance. </li>
<li><a href="https://issues.apache.org/jira/browse/MESOS-4085">MESOS-4085</a>: Flexible roles with the introduction of implicit roles. It deprecates the whitelist functionality that was implemented by specifying --roles during master startup to provide a static list of roles. </li>
<li><a href="https://issues.apache.org/jira/browse/MESOS-2353">MESOS-2353</a>: Performance improvement of the state endpoint for large clusters. </li>
<li>Furthermore, 167+ bugfixes and improvements made it into this release. For full release notes with all features and bug fixes, please refer to the <a href="https://git-wip-us.apache.org/repos/asf?p=mesos.git;a=blob_plain;f=CHANGELOG;hb=0.27.0">CHANGELOG</a>.</li>
</ul>

<h1><a name="known-issues"></a>Known Issues and Limitations</h1>

<ul>
<li>The Service and Agent panels of the DCOS Web Interface won't render over 5,000 tasks. If you have a service or agent that has over 5,000 your browser may experience slowness. In this case you can close said browser tab and reopen the DCOS web interface.</li>
<li>See additional known issues at <a href="https://support.mesosphere.com" target="_blank">support.mesosphere.com</a>.</li>
</ul>