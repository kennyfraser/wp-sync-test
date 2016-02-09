---
ID: 2932
post_title: Managing Authorization
author: Joel Hamill
post_date: 2016-02-05 16:45:41
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/enabling-authorization/
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
You can enable authorization and authentication in your datacenter with DCOS Enterprise Edition.

# Manage users

The available users roles are Administrator and User. The **Administrator** has complete control of the DCOS and installed services. The **User** role has limited access to DCOS and installed services, as defined by the Administrator.

1.  Launch the DCOS web interface and login with your Admin username and password. The Admin username and password are configured during installation. By default, the Admin username is given the Full Name of **Bootstrap superuser**.

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

# Manage groups

You can create Groups and assign users to it. Groups are comprised of Users and installed DCOS services and components. By default, when you launch DCOS the Administrator is added to the **Access Control Service super user group**.

1.  Launch the DCOS web interface and login with the Admin username and password. The Admin username and password are configured during installation.

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

# Managing LDAP

To provide external user access to DCOS, you can authenticate users defined in an LDAP (Lightweight Directory Access Protocol) registry. You can then provide users with a single sign-on (SSO), granting them access the authorized services in your DCOS datacenter.

1.  Launch the DCOS web interface and login with the Admin username and password. The Admin username and password are configured during installation.
    
    ### Add LDAP directory
    
    1.  Click on the **Settings** -> **Organization** -> **External Directory** tab.
    
    2.  Click **Add Directory**.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-ldap.png" rel="attachment wp-att-3223"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-ldap.png" alt="auth-ldap" width="513" height="515" class="alignnone size-full wp-image-3223" /></a>
        
        **Host**
        
        **Port**
        
        **Distinguished Name Template**

<!-- *   add LDAP -->

<!-- *   delete LDAP -->