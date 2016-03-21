---
UID: 56ec1d00939f4
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
    send: 'GET /capabilities HTTP/1.1rnHost: pool-2434-elasticl-1wjiys4who8b1-1077936506.us-west-2.elb.amazonaws.comrnConnection: keep-alivernContent-Type: application/vnd.dcos.capabilities+json;charset=utf-8;version=v1rnAccept-Encoding: gzip, deflaternAccept: application/vnd.dcos.capabilities+json;charset=utf-8;version=v1rnUser-Agent: python-requests/2.9.1rnrn'
    reply: 'HTTP/1.1 200 OKrn'
    header: Content-Type: application/vnd.dcos.capabilities+json;charset=utf-8;version=v1
    header: Date: Thu, 17 Mar 2016 22:51:39 GMT
    header: Server: openresty/1.7.10.2
    header: Content-Length: 48
    header: Connection: keep-alive
    send: 'POST /package/list HTTP/1.1rnHost: pool-2434-elasticl-1wjiys4who8b1-1077936506.us-west-2.elb.amazonaws.comrnContent-Length: 2rnAccept-Encoding: gzip, deflaternAccept: application/vnd.dcos.package.list-response+json;charset=utf-8;version=v1rnUser-Agent: python-requests/2.9.1rnConnection: keep-alivernContent-Type: application/vnd.dcos.package.list-request+json;charset=utf-8;version=v1rnrn{}'