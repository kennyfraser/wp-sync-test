---
ID: 3283
post_title: Enabling LDAP
author: Joel Hamill
post_date: 2016-02-11 13:17:09
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/security-and-authentication/enabling-ldap/
published: true
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
header:
  - "1"
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
page_header:
  - "1"
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "0"
---
To provide external user access to DCOS, you can authenticate users that are defined in an LDAP (Lightweight Directory Access Protocol) registry. You can then provide users with a single sign-on (SSO), granting them access the authorized services in your DCOS datacenter.

When a user logs in and provides an LDAP user name and password, that information is used to connect to an LDAP server and authenticate a user. If successful, the DCOS authentication issues a session credential with the user's distinguished name.

LDAP users are uniquely identified by [distinguished names (DN)][1]. To authenticate a user, a full DN and password are required. However, you can use a distinguished name template to allows users to enter only the part of their DN name that is unique to their accounts. The distinguished name template converts the partial DN name to a full DN.

**Prerequisite:**

*   DCOS Enterprise Edition

1.  Launch the DCOS web interface and login with the Admin username and password. The Admin username and password are configured during installation.
    
    1.  Click on the **Settings** -> **Organization** -> **External Directory** tab.
    
    2.  Click **Add Directory**.
        
        <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-ldap.png" rel="attachment wp-att-3223"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/auth-ldap.png" alt="auth-ldap" width="513" height="515" class="alignnone size-full wp-image-3223" /></a>
        
        **Host** Specify the hostname or IP address.
        
        **Port** Specify the port to use.
        
        **Distinguished Name Template** Specify the template to use for translating a username to a distinguished name. The distinguished name template is a static string that contains the `%(username)s` substring, which is replaced with the name the user provides when logging in. The template structure depends on your directory setup. Here's an example:
        
            cn=%(username)s,dc=los-pollos,dc=io
            uid=%(username)s,cn=users,cn=accounts,dc=demo1,dc=freeipa,dc=org
            uid=%(username)s,ou=users,dc=example,dc=com
            
        
        **Connection options:** By default, a plain text connection is opened and then [STARTTLS][2] attempts to upgrade this plain text connection to an encrypted SSL/TLS connection. If this upgrade to encryption fails, the plain text connection continues. You can control this by choosing one of these options:
        
        *   **Use SSL/TLS socket** Use SSL/TLS socket for opening a connection. <!-- in 1.7 "Use SSL/TLS socket for opening a connection" -->
        
        *   **Enforce startTLS** Abort connection if STARTTLS fails. If the upgrade to an encrypted SSL/TLS connection fails, the authentication operation is terminated. <!-- In 1.7 "Abort connection if STARTTLS fails" -->

2.  Click **Add**.

 [1]: https://www.ldap.com/ldap-dns-and-rdns
 [2]: https://en.wikipedia.org/wiki/STARTTLS