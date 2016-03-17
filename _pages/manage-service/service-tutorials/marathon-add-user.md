---
UID: 56df3deebd7cf
post_title: Adding a Marathon User Instance
post_excerpt: ""
layout: page
published: true
menu_order: 25
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>A native Marathon instance is installed as a part of the DCOS installation. This tutorial creates a Marathon instance on top of the native Marathon to create separate user environments.</p>

<dl>
<dt>Prerequisite</dt>
<dd>
<p><a href="/install/cli/">Install the DCOS CLI</a></p>
</dd>
</dl>

<ol>
<li><p>Create a JSON configuration file, specify <code>marathon-alice</code> as the framework name, and save as <code>newuser.json</code>:</p>

<pre><code>{"marathon": {"framework-name": "marathon-alice" }}
</code></pre>

<p><strong>Tip:</strong> You must create separate JSON configuration files for each Marathon instance.</p></li>
<li><p>From the DCOS CLI, enter this command to install the Marathon instance:</p>

<pre><code>$ dcos package install --options=newuser.json marathon
</code></pre></li>
<li><p>From the DCOS web interface <strong>Services</strong> tab, click on the <strong>marathon-alice</strong> service name to navigate to the Marathon web interface.</p></li>
<li><p>Optional: You can modify the DCOS CLI configuration to point to the <strong>marathon-alice</strong> instance. This allows you to administer your Marathon instance by using the DCOS CLI.</p>

<ol>
<li><p>From the DCOS CLI, set the <code>marathon.url</code> property to point to the <strong>marathon-alice</strong> instance, where <code>&lt;hostname&gt;</code> is the Marathon web interface hostname:</p>

<pre><code>$ dcos config set marathon.url http://&lt;hostname&gt;/service/marathon-alice/
</code></pre></li>
<li><p>Verify that the the <code>marathon.url</code> is set. The <code>marathon.url</code> takes precedence over the native Marathon in DCOS.</p>

<pre><code>$ dcos config show
core.dcos_url=http://nodel-elasticl-1xyz-1940784093.us-west-2.elb.amazonaws.com
core.email=youremail@email.io
core.reporting=True
core.token=a547c734ed81247d0203ce238a5a07ac012b59f8d7f89ed539e5110557548152
marathon.url=http://alicenodel-elasticl-1xyz-1940784093.us-west-2.elb.amazonaws.com/service/marathon-alice/
package.cache=/Users/alice/.dcos/cache
package.sources=['https://github.com/mesosphere/universe/archive/version-1.x.zip']
</code></pre>

<p><strong>Tip:</strong> You can switch back to the native Marathon instance by specifying <code>dcos config unset marathon.url</code>.</p></li>
</ol></li>
</ol>

<h3>Next Steps</h3>

<p>After you have your Marathon instance up and running, you can try <a href="../getting-started/tutorials/deploy-containerized-app/">Deploying a Containerized App on a Public Node</a>.</p>