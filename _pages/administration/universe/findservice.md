---
UID: 56df3def1ec46
post_title: Using the Package Repository
post_excerpt: ""
layout: page
published: true
menu_order: 21
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<h1>Search the package repository</h1>

<p>You can use the DCOS CLI to find DCOS services to install.</p>

<ol>
<li><p>First, update the local cache of the package repository to make sure you have the latest available package version:</p>

<pre><code>$ dcos package update
Updating source [https://github.com/mesosphere/universe/archive/version-1.x.zip]
</code></pre></li>
<li><p>Then, use the <code>dcos package search</code> command to find available big data packages:</p>

<pre><code>$ dcos package search "big data"
NAME VERSION FRAMEWORK SOURCE DESCRIPTION  
spark 1.4.0-SNAPSHOT True https://github.com/mesosphere/universe/archive/version-1.x.zip Spark is a fast and general cluster computing system for Big Data
</code></pre></li>
</ol>

<h1>Add a repository to the package source list</h1>

<p>In this example, a package repository is added to the beginning of the <code>packages.sources</code> list:</p>

<pre><code>$ dcos config prepend package.sources https://yourcompany/archive/stuff.zip
$ dcos config show
dcos config show
core.dcos_url=&lt;hostname&gt;
core.email=youremail@email.com
core.reporting=True
core.token=375b30cd25c7c1f7952066098a8860c6d9d52e04e87d4dbff3ee1ea8b8fdac80
package.cache=/Users/username/.dcos/cache
package.sources=['https://yourcompany/archive/stuff.zip', 'https://github.com/mesosphere/universe/archive/master.zip']
</code></pre>

<h1>Remove an item from the package source</h1>

<p>In this example, the first item is removed from the <code>package.sources</code> parameter:</p>

<pre><code>$ dcos config unset package.sources --index=0
</code></pre>