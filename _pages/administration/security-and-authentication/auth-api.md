---
UID: 56eaafe07143c
post_title: Authentication HTTP API Endpoint
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
You can make external calls to HTTP API endpoints in your DCOS cluster.

You must first obtain an authorization token and then include the authorization token in your HTTP request.

# Generate the authentication token

### Request

Log in with a POST request against `acs/api/v1/auth/login`.

### Data

Specify the user ID (`<user-id>`), password (`<password>`), and external hostname (`<master-host-name>`):

    $ curl --data '{"uid":"<user-id>", "password":"<your-password>"}' \
        --header "Content-Type:application/json" \
        http://<master-host-name>/acs/api/v1/auth/login
    

### Response

The response provides an authentication token that you can provide in the HTTP API `Authorization` header for subsequent requests:

    {
    "token": "<authtoken>"
    }
    

# Make HTTP request using the Authorization header

To authenticate an HTTP request against a DCOS component, specify the `<authtoken>` in the request header.

For example, to access the Marathon UI:

    $ curl --header "Authorization: token=<authtoken>" http://<master-host-name>/service/marathon/v2/apps
    

For example, to access the Mesos UI:

    $ curl --header "Authorization: token=<authtoken>" http://<master-host-name>/mesos/master/state.json