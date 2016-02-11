---
ID: 2932
post_title: >
  Managing Authorization and
  Authentication
author: Joel Hamill
post_date: 2016-02-05 16:45:41
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/security-and-authentication/managing-authorization/
published: true
header:
  - "1"
page_header:
  - "1"
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "1"
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
page_header_0_show_page_header:
  - "0"
page_header_0_size:
  - default
page_header_0_fill_screen:
  - "0"
page_header_0_background:
  - transparent
page_header_0_show_background_image:
  - "0"
page_header_0_show_background_video:
  - "0"
page_header_0_headline:
  - ""
page_header_0_headline_size:
  - default
page_header_0_description:
  - ""
page_header_0_description_size:
  - default
page_header_0_show_image:
  - "0"
page_header_0_content_alignment:
  - center
page_header_0_content_style:
  - dark
page_header_0_actions:
  - "0"
page_header_0_show_actions_footnote:
  - "0"
page_header_0_show_video:
  - "0"
---
Authorization and authentication is managed in the DCOS web interface.

The available users roles are Administrator and User. **Administrator** has complete control of DCOS and installed services. **User** has limited access to DCOS and installed services, as defined by the Administrator.

You can create groups and assign users to them. Groups are a collection of installed DCOS services and components that users are granted access to. By default, when you launch DCOS the Administrator is added to the Superuser group.

# Users

You must be an Administrator to manage users or groups.

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