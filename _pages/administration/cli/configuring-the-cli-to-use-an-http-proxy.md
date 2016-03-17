---
UID: 56df3dee295e7
post_title: Configuring the CLI to use an HTTP Proxy
post_excerpt: ""
layout: page
published: true
menu_order: 15
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>If you use a proxy server to connect to the Internet, you can configure the CLI to use your proxy server.</p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>pip version 7.1.0 or greater </li>
<li>The <code>http_proxy</code> and <code>https_proxy</code> environment variables are defined to use pip.</li>
</ul>

<p>To configure a proxy for the CLI:</p>

<ol>
<li><p>From the CLI terminal, define the environment variables <code>http_proxy</code> and <code>https_proxy</code>:</p>

<pre><code>$ export http_proxy=’&lt;http://your/proxy/here/&gt;’
$ export https_proxy=’&lt;http://your/proxy/here/&gt;’
</code></pre></li>
<li><p>Define <code>no_proxy</code> for domains that you don’t want to use the proxy for:</p>

<pre><code>$ export no_proxy="127.0.0.1, localhost”
</code></pre></li>
</ol>