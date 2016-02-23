---
ID: 93
post_title: >
  Managing Authorization and
  Authentication
post_date: 2016-02-22 18:13:26
post_excerpt: ""
layout: page
permalink: >
  http://local.gitsync.com/managing-authorization-and-authentication/
published: true
post_parent: 0
menu_order: 0
page_options_require_authentication: true
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
Authorization and authentication is managed in the DCOS web interface.

The available users roles are Administrator and User.

*   The administrator has complete control of DCOS and installed services. The Administrator username and password are configured during DCOS installation. The administrator full name is `Bootstrap superuser` and cannot be changed. 
*   The user has limited access to DCOS and installed services, as defined by the Administrator. You can create users and grant them access to individual services.

You can create groups and assign users to them. Groups are a collection of installed DCOS services and components that users are granted access to. By default the Boostrap superuser is added to the `Access Control Service super user group`. Members of the Access Control Service super user group have access to all DCOS components.

**Prerequisites:**

*   DCOS Enterprise Edition
*   You must be an Administrator to manage users or groups.

# User Management

1.  Launch the DCOS web interface and login with your Admin username and password.

2.  Click on the **Settings** -> **Users** tab and choose your action.
    
    ### Add users
    
    *   From the **Users** tab, click **New User** and fill in the name, username, and password. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user2-1.gif" rel="attachment wp-att-3494"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user2-1-800x507.gif" alt="auth-enable-add-user2" width="800" height="507" class="alignnone size-large wp-image-3494" /></a> 
    ### Modify users
    
    You can modify permissions, group membership, and passwords. You cannot modify the username.
    
    *   From the **Users** tab, click the user name and select your action. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-modify.gif" rel="attachment wp-att-3495"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-modify-800x509.gif" alt="auth-enable-modify" width="800" height="509" class="alignnone size-large wp-image-3495" /></a>
    ### Delete users
    
    1.  From the **Users** tab, select the user name and click **Actions -> Delete**. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-delete-user.gif" rel="attachment wp-att-3498"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-delete-user-800x510.gif" alt="auth-enable-delete-user" width="800" height="510" class="alignnone size-large wp-image-3498" /></a>
    2.  Click **Delete** to confirm the action.

# Group management

1.  Launch the DCOS web interface and login with your Admin username and password. The Administrator username and password are configured during DCOS installation. The Admin username is **Bootstrap superuser** and cannot be changed.

2.  Click on the **Settings** -> **Organization** tab and choose your action.
    
    ### Add a group
    
    1.  From the **Groups** tab, click **New Group**.
    2.  Type in the group name and click **Create**.
    3.  Select the group name and add Members and Permissions and click **Close**. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-define-group.gif" rel="attachment wp-att-3501"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-define-group-800x509.gif" alt="auth-enable-define-group" width="800" height="509" class="alignnone size-large wp-image-3501" /></a> 
        *   **Members** Defines the users that are in the group.
        *   **Permissions** Defines the installed DCOS services and components.
    ### Add users to a group
    
    1.  From the **Users** tab, select the user name and click **Actions -> Add to Group**. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user-group.gif" rel="attachment wp-att-3499"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user-group-800x509.gif" alt="auth-enable-add-user-group" width="800" height="509" class="alignnone size-large wp-image-3499" /></a>
    2.  Choose the group and click **Add**. 
    ### Delete users from group
    
    1.  From the **Users** tab, select the user name and click **Actions -> Delete from Group**.
    2.  Choose the group and click **Remove**.
    ### Delete a group
    
    1.  From the **Groups** tab, select the group to open.
    2.  Click **Delete**.
    ### Modify a group
    
    1.  From the **Groups** tab, select the group to open.
    2.  Modify and click **Close**.