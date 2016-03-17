---
UID: 56df3debb5c64
post_title: Enabling LDAP
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: true
---
<p><strong>Disclaimer:</strong> This is an alpha feature and is meant for testing purposes only.</p>

<p>To provide external user access to DCOS, you can authenticate users that are defined in an LDAP (Lightweight Directory Access Protocol) registry. You can then provide users with a single sign-on (SSO), granting them access the authorized services in your DCOS datacenter.</p>

<p>When a user logs in and provides an LDAP user name and password, that information is used to connect to an LDAP server and authenticate a user. If successful, the DCOS authentication issues a session credential with the user's distinguished name.</p>

<p>LDAP users are uniquely identified by <a href="https://www.ldap.com/ldap-dns-and-rdns">distinguished names (DN)</a>. To authenticate a user, a full DN and password are required. However, you can use a distinguished name template to allows users to enter only the part of their DN name that is unique to their accounts. The distinguished name template converts the partial DN name to a full DN.</p>

<p><strong>Prerequisite:</strong></p>

<ul>
<li>DCOS Enterprise Edition</li>
</ul>

<ol>
<li><p>Launch the DCOS web interface and login with the Admin username and password. The Admin username and password are configured during installation.</p>

<ol>
<li><p>Click on the <strong>Settings</strong> -&gt; <strong>Organization</strong> -&gt; <strong>External Directory</strong> tab.</p></li>
<li><p>Click <strong>Add Directory</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-ldap.gif" rel="attachment wp-att-3790"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-ldap-800x484.gif" alt="auth-ldap" width="800" height="484" class="alignnone size-large wp-image-3790" /></a></p>

<p><strong>Host</strong> Specify the hostname or IP address.</p>

<p><strong>Port</strong> Specify the port to use.</p>

<p><strong>Distinguished Name Template</strong> Specify the template to use for translating a username to a distinguished name. The distinguished name template is a static string that contains the <code>%(username)s</code> substring, which is replaced with the name the user provides when logging in. The template structure depends on your directory setup. Here's an example:</p>

<pre><code>cn=%(username)s,dc=los-pollos,dc=io
uid=%(username)s,cn=users,cn=accounts,dc=demo1,dc=freeipa,dc=org
uid=%(username)s,ou=users,dc=example,dc=com
</code></pre>

<p><strong>Connection options:</strong> By default, a plain text connection is opened and then <a href="https://en.wikipedia.org/wiki/STARTTLS">STARTTLS</a> attempts to upgrade this plain text connection to an encrypted SSL/TLS connection. If this upgrade to encryption fails, the plain text connection continues. You can control this by choosing one of these options:</p>

<ul>
<li><p><strong>Use SSL/TLS socket for all connections</strong> Use SSL/TLS socket for opening a connection.</p></li>
<li><p><strong>Use startTLS for all connections</strong> Abort connection if STARTTLS fails. If the upgrade to an encrypted SSL/TLS connection fails, the authentication operation is terminated.</p></li>
</ul></li>
</ol></li>
<li><p>Click <strong>Add</strong>.</p></li>
</ol>