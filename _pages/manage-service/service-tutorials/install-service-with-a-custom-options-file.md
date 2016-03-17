---
UID: 56df3ded97f67
post_title: Customizing a DCOS Service
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can customize your DCOS service during installation with a JSON configuration file. Otherwise, the services are installed by using default values.</p>

<p>The general process is as follows:</p>

<ol>
<li><p>View the available configuration options with the <code>dcos package describe --config &lt;package-name&gt;</code> command:</p>

<pre><code>$ dcos package describe --config marathon
{
 "properties": {
    "marathon": {
      "cpus": {
        "default": 2.0,
        "description": "CPU shares to allocate to each Marathon instance.",
        "minimum": 0.0,
        "type": "number"
     },
    ...        
    "mem": {
      "default": 1024.0,
      "description": "Memory (MB) to allocate to each Marathon task.",
      "minimum": 512.0,
      "type": "number"
     },
     ...
}
</code></pre></li>
<li><p>Create a JSON configuration file. You can choose an arbitrary name, but you might want to choose a pattern like <code>&lt;package-name&gt;-config.json</code>. For example, <code>marathon-config.json</code>.</p>

<pre><code>$ nano marathon-config.json
</code></pre></li>
<li><p>Add an entry to a JSON options file. For example, to change the number of Marathon CPU shares to 3 and memory allocation to 2048:</p>

<pre><code>{
  "marathon": { 
    "cpus": 3.0, "mem": 2048.0 
   } 
}
</code></pre></li>
<li><p>From the DCOS CLI, install the DCOS service with the custom options file specified:</p>

<pre><code>$ dcos package install --options=marathon-config.json marathon
</code></pre></li>
</ol>

<p>For more information, see the <a href="../administration/introcli/command-reference/#scrollNav-4">dcos package</a> documentation.</p>