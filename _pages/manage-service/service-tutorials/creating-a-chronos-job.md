---
UID: 56df3dede196e
post_title: Creating a Chronos job
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>In this example, a Chronos job that sleeps for 10 seconds is created.</p>

<p><strong>Prerequisite:</strong></p>

<ul>
<li>The DCOS Chronos service is <a href="../reference/chronos/#chronosinstall">installed</a></li>
</ul>

<ol>
<li><p>From the DCOS Services page, click on the <strong>chronos</strong> link to go to the Chronos web interface.</p></li>
<li><p>From the Chronos web interface, click <strong>New Job</strong>.</p></li>
<li><p>Add the NAME “Test Chronos Sleep”, COMMAND <code>sleep 10</code>, your email for OWNER(S), and click <strong>Create</strong>.</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosnewjob.png" rel="attachment wp-att-1308"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosnewjob.png" alt="chronosnewjob" width="424" height="432" class="alignnone size-full wp-image-1308" /></a></p></li>
<li><p>After the job is created, you can click on <strong>Force Run</strong> (the ‘play’ button) to run it. You should see it as a running task on your Mesos master and the Sandbox. Once it’s finished running, you should see status “SUCCESS.”</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosforcerun.png" rel="attachment wp-att-1310"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/chronosforcerun.png" alt="chronosforcerun" width="315" height="99" class="alignnone size-full wp-image-1310" /></a></p></li>
</ol>