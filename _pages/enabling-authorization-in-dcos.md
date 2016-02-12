---
ID: 2932
post_title: >
  Managing Authorization and
  Authentication
author: Joel Hamill
post_date: 2016-01-28 12:11:01
post_excerpt: ""
layout: page
permalink: >
  http://local.mesodocs.com/getting-started/installing/installing-enterprise-edition/enabling-authorization-in-dcos/
published: true
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: false
page_options_show_link_unauthenticated: false
---
Authorization and authentication is managed in the DCOS web interface.

The available users roles are Administrator and User. **Administrator** has complete control of DCOS and installed services. **User** has limited access to DCOS and installed services, as defined by the Administrator.

You can create groups and assign users to them. Groups are a collection of installed DCOS services and components that users are granted access to. By default, when you launch DCOS the Administrator is added to the Superuser group.

# Users

You must be an Administrator to manage users or groups.

**Prerequisite:**

*   DCOS Enterprise Edition

1.  Launch the DCOS web interface and login with your Admin username and password. The Administrator username and password are configured during DCOS installation. The Admin username is **Bootstrap superuser** and cannot be changed.

2.  Click on the **Settings** -> **Users** tab and choose your action.
    
    ### Add users
    
    *   From the **Users** tab, click **New User** and fill in the name, username, and password. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user.png" rel="attachment wp-att-3107"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-add-user-600x445.png" alt="auth-enable-add-user" width="300" height="223" class="alignnone size-medium wp-image-3107" /></a>
    ### Modify users
    
    You can modify permissions, group membership, and passwords. You cannot modify the username.
    
    *   From the **Users** tab, click the user name and select your action. 
    ### Delete users
    
    *   From the **Users** tab, select the user name and click **Actions -> Delete**.
    ### Add users to a group
    
    1.  From the **Users** tab, select the user name and click **Actions -> Add to Group**. <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-modify.png" rel="attachment wp-att-3111"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-enable-modify-800x304.png" alt="auth-enable-modify" width="800" height="304" class="alignnone size-large wp-image-3111" /></a>
    2.  Choose the group and click **Add**. 
    ### Delete users from group
    
    1.  From the **Users** tab, select the user name and click **Actions -> Delete from Group**.
    2.  Choose the group and click **Remove**.

# Groups

You must be an Administrator to manage users or groups.

**Prerequisite:**

*   DCOS Enterprise Edition

1.  Launch the DCOS web interface and login with your Admin username and password. The Administrator username and password are configured during DCOS installation. The Admin username is **Bootstrap superuser** and cannot be changed.

2.  Click on the **Settings** -> **Organization** tab and choose your action.
    
    ### Add a group
    
    1.  From the **Groups** tab, click **New Group**.
    2.  Type in the group name and click **Create**.
    3.  Add Members and Permissions and click **Close**.
        
        *   **Members** Defines the users that are in the group.
        *   **Permissions** Defines the installed DCOS services and components.
    ### Delete a group
    
    1.  From the **Groups** tab, select the group to open.
    2.  Click **Delete**.
    ### Modify a group
    
    1.  From the **Groups** tab, select the group to open.
    2.  Modify and click **Close**.