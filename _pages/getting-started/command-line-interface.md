---
post_title: Command line overview
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---

Welcome to my world.

    Joels-MacBook-Pro-10:dcos joelhamill$ cat dog.txt
    send: 'GET /capabilities HTTP/1.1\r\nHost: pool-2434-elasticl-1wjiys4who8b1-1077936506.us-west-2.elb.amazonaws.com\r\nConnection: keep-alive\r\nContent-Type: application/vnd.dcos.capabilities+json;charset=utf-8;version=v1\r\nAccept-Encoding: gzip, deflate\r\nAccept: application/vnd.dcos.capabilities+json;charset=utf-8;version=v1\r\nUser-Agent: python-requests/2.9.1\r\n\r\n'
    reply: 'HTTP/1.1 200 OK\r\n'
    header: Content-Type: application/vnd.dcos.capabilities+json;charset=utf-8;version=v1
    header: Date: Thu, 17 Mar 2016 22:51:39 GMT
    header: Server: openresty/1.7.10.2
    header: Content-Length: 48
    header: Connection: keep-alive
    send: 'POST /package/list HTTP/1.1\r\nHost: pool-2434-elasticl-1wjiys4who8b1-1077936506.us-west-2.elb.amazonaws.com\r\nContent-Length: 2\r\nAccept-Encoding: gzip, deflate\r\nAccept: application/vnd.dcos.package.list-response+json;charset=utf-8;version=v1\r\nUser-Agent: python-requests/2.9.1\r\nConnection: keep-alive\r\nContent-Type: application/vnd.dcos.package.list-request+json;charset=utf-8;version=v1\r\n\r\n{}'
