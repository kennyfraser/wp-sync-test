---
UID: 56df3defa49fb
post_title: Updating the CLI
post_excerpt: ""
layout: page
published: true
menu_order: 25
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can update the DCOS CLI to the latest version or downgrade to an older version.</p>

<h1>Upgrade the CLI</h1>

<p>You can upgrade an existing DCOS CLI installation to the latest build.</p>

<ol>
<li><p>From your DCOS CLI installation directory, enter this command to update the DCOS CLI:</p>

<pre><code>$ sudo pip install -U dcoscli
</code></pre></li>
</ol>

<h1>Downgrade the CLI</h1>

<p>You can downgrade an existing DCOS CLI installation to an older version.</p>

<p><strong>Tip:</strong> Downgrading is necessary if you are running an older version of DCOS and want to reinstall the DCOS CLI.</p>

<ol>
<li><p>Delete your DCOS CLI installation directories:</p>

<pre><code>$ sudo rm -rf dcos &amp;&amp; rm -rf ~/.dcos
</code></pre></li>
<li><p>Install the legacy version of the DCOS CLI, where is the public IP of your master node:</p>

<pre><code>mkdir -p dcos &amp;&amp; cd dcos &amp;&amp; 
  curl -O https://downloads.mesosphere.com/dcos-cli/install-legacy.sh &amp;&amp; 
  bash ./install-legacy.sh . &lt;public-master-ip&gt; &amp;&amp; 
  source ./bin/env-setup
</code></pre></li>
</ol>