---
ID: 2932
post_title: >
  Managing Authorization and
  Authentication
post_date: 2016-02-05 16:45:41
post_excerpt: ""
layout: page
permalink: >
  https://gitsync.mmdev2.ca/managing-authorization/
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: true
---
Authorization and authentication is managed in the DCOS web interface.

You can authorize individual users and groups of users.

*   **Users** are granted access to DCOS and installed services by an administrator. During installation a default administrator user named `Bootstrap superuser` is created and added to the group named `Superuser group`. The Superuser group has access to all DCOS components and is where DCOS administrator access is defined. Only members of the Superuser group have access to the DCOS web interface. 
*   **Groups** are comprised of a list of installed DCOS services that users are granted access to. 

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
    ### Switch users
    
    To switch users, you must log out of the current user and then back in as the new user.
    
    To log out of the DCOS web interface:
    
    1.  Click on your username in the lower left corner and select **Sign Out**. <!-- insert graphic -->
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-logout-user.gif" rel="attachment wp-att-3533"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-logout-user-800x430.gif" alt="auth-enable-logout-user" width="800" height="430" class="alignnone size-large wp-image-3533" /></a>
        
        You can now log in as another user.
    
    To log out of the DCOS CLI:
    
    1.  Enter the this command:
        
            $ dcos config unset core.dcos_acs_token
            Removed [core.dcos_acs_token]
            
        
        You can now log in as another user.

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