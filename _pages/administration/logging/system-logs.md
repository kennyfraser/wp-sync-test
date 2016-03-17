---
UID: 56df3def6cd0a
post_title: System Logging
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>To access the DCOS core component logs, you must have SSH access to your DCOS cluster. The DCOS core components use the host’s systemd journal for logging; they do not use the standard Mesos sandbox logs.</p>

<ol>
<li><p><a href="/install/sshcluster/">SSH into your master node</a>.</p></li>
<li><p>Enter the command for the component whose logs you wish to view:</p>

<p><strong>Admin Router</strong></p>

<pre><code>   journalctl -u dcos-nginx -b
</code></pre>

<p><strong>DCOS Marathon</strong></p>

<pre><code>   journalctl -u dcos-marathon -b
</code></pre>

<p><strong>gen-resolvconf</strong></p>

<pre><code>   journalctl -u dcos-gen-resolvconf -b
</code></pre>

<p><strong>Mesos master node</strong></p>

<pre><code>   journalctl -u dcos-mesos-master -b
</code></pre>

<p><strong>Mesos agent node</strong></p>

<pre><code>   journalctl -u dcos-mesos-slave -b
</code></pre>

<p><strong>Mesos DNS</strong></p>

<pre><code>   journalctl -u dcos-mesos-dns -b
</code></pre>

<p><strong>ZooKeeper</strong></p>

<pre><code>   journalctl -u dcos-exhibitor -b
</code></pre></li>
</ol>