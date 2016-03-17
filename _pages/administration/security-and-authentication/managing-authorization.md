---
UID: 56df3dec171d6
post_title: >
  Managing Authorization and
  Authentication
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: true
---
<p>Authorization and authentication is managed in the DCOS web interface.</p>

<p>You can authorize individual users and groups of users.</p>

<ul>
<li><strong>Users</strong> are granted access to DCOS and installed services by an administrator. During installation a default administrator user named <code>Bootstrap superuser</code> is created and added to the group named <code>Superuser group</code>. The Superuser group has access to all DCOS components and is where DCOS administrator access is defined. Only members of the Superuser group have access to the DCOS web interface. </li>
<li><strong>Groups</strong> are comprised of a list of installed DCOS services that users are granted access to. </li>
</ul>

<p><strong>Prerequisites:</strong></p>

<ul>
<li>DCOS Enterprise Edition</li>
<li>You must be an Administrator to manage users or groups.</li>
</ul>

<h1>User Management</h1>

<ol>
<li><p>Launch the DCOS web interface and login with your Admin username and password.</p></li>
<li><p>Click on the <strong>Settings</strong> -&gt; <strong>Users</strong> tab and choose your action.</p>

<h3>Add users</h3>

<ul>
<li>From the <strong>Users</strong> tab, click <strong>New User</strong> and fill in the name, username, and password. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user2-1.gif" rel="attachment wp-att-3494"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user2-1-800x507.gif" alt="auth-enable-add-user2" width="800" height="507" class="alignnone size-large wp-image-3494" /></a> </li>
</ul>

<h3>Modify users</h3>

<p>You can modify permissions, group membership, and passwords. You cannot modify the username.</p>

<ul>
<li>From the <strong>Users</strong> tab, click the user name and select your action. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-modify.gif" rel="attachment wp-att-3495"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-modify-800x509.gif" alt="auth-enable-modify" width="800" height="509" class="alignnone size-large wp-image-3495" /></a></li>
</ul>

<h3>Delete users</h3>

<ol>
<li>From the <strong>Users</strong> tab, select the user name and click <strong>Actions -&gt; Delete</strong>. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-delete-user.gif" rel="attachment wp-att-3498"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-delete-user-800x510.gif" alt="auth-enable-delete-user" width="800" height="510" class="alignnone size-large wp-image-3498" /></a></li>
<li>Click <strong>Delete</strong> to confirm the action.</li>
</ol>

<h3>Switch users</h3>

<p>To switch users, you must log out of the current user and then back in as the new user.</p>

<p>To log out of the DCOS web interface:</p>

<ol>
<li><p>Click on your username in the lower left corner and select <strong>Sign Out</strong>. <!-- insert graphic --></p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-logout-user.gif" rel="attachment wp-att-3533"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-logout-user-800x430.gif" alt="auth-enable-logout-user" width="800" height="430" class="alignnone size-large wp-image-3533" /></a></p>

<p>You can now log in as another user.</p></li>
</ol>

<p>To log out of the DCOS CLI:</p>

<ol>
<li><p>Enter the this command:</p>

<pre><code>$ dcos config unset core.dcos_acs_token
Removed [core.dcos_acs_token]
</code></pre>

<p>You can now log in as another user.</p></li>
</ol></li>
</ol>

<h1>Group management</h1>

<ol>
<li><p>Launch the DCOS web interface and login with your Admin username and password. The Administrator username and password are configured during DCOS installation. The Admin username is <strong>Bootstrap superuser</strong> and cannot be changed.</p></li>
<li><p>Click on the <strong>Settings</strong> -&gt; <strong>Organization</strong> tab and choose your action.</p>

<h3>Add a group</h3>

<ol>
<li>From the <strong>Groups</strong> tab, click <strong>New Group</strong>.</li>
<li>Type in the group name and click <strong>Create</strong>.</li>
<li>Select the group name and add Members and Permissions and click <strong>Close</strong>. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-define-group.gif" rel="attachment wp-att-3501"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-define-group-800x509.gif" alt="auth-enable-define-group" width="800" height="509" class="alignnone size-large wp-image-3501" /></a> 

<ul>
<li><strong>Members</strong> Defines the users that are in the group.</li>
<li><strong>Permissions</strong> Defines the installed DCOS services and components.</li>
</ul></li>
</ol>

<h3>Add users to a group</h3>

<ol>
<li>From the <strong>Users</strong> tab, select the user name and click <strong>Actions -&gt; Add to Group</strong>. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user-group.gif" rel="attachment wp-att-3499"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user-group-800x509.gif" alt="auth-enable-add-user-group" width="800" height="509" class="alignnone size-large wp-image-3499" /></a></li>
<li>Choose the group and click <strong>Add</strong>. </li>
</ol>

<h3>Delete users from group</h3>

<ol>
<li>From the <strong>Users</strong> tab, select the user name and click <strong>Actions -&gt; Delete from Group</strong>.</li>
<li>Choose the group and click <strong>Remove</strong>.</li>
</ol>

<h3>Delete a group</h3>

<ol>
<li>From the <strong>Groups</strong> tab, select the group to open.</li>
<li>Click <strong>Delete</strong>.</li>
</ol>

<h3>Modify a group</h3>

<ol>
<li>From the <strong>Groups</strong> tab, select the group to open.</li>
<li>Modify and click <strong>Close</strong>.</li>
</ol></li>
</ol>