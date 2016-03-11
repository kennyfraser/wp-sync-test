---
ID: 2555
post_title: HTTP Interface
author: Joel Hamill
post_date: 2016-03-08 14:03:49
post_excerpt: ""
layout: page
permalink: >
  https://dev-mesosphere-documentation.pantheonsite.io/http-interface/
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
page_options_topic_page:
  - ""
page_options_require_authentication:
  - ""
hide_from_navigation:
  - ""
hide_from_related:
  - ""
UID:
  - 56df3deca5b21
page_options_show_link_unauthenticated:
  - ""
menu_order:
  - "3"
---
Mesos-DNS implements a simple REST API for service discovery over HTTP:

# <a name="get-version"></a>`GET /v1/version`

Lists in JSON format the Mesos-DNS version and source code URL.

    $ curl http://10.190.238.173:8123/v1/version
    {
        "Service":"Mesos-DNS",
        "URL":"https://github.com/mesosphere/mesos-dns","Version":"0.1.1"
    }
    

# <a name="get-config"></a>`GET /v1/config`

Lists in JSON format the Mesos-DNS configuration parameters.

    $ curl http://10.190.238.173:8123/v1/config
    {
        "Masters":null,
        "Zk":"zk://10.207.154.85:2181/mesos",
        "RefreshSeconds":60,
        "TTL":60,
        "Port":53,
        "Domain":"mesos",
        "Resolvers":["169.254.169.254","10.0.0.1"],
        "Timeout":5,
        "Email":"root.mesos-dns.mesos.",
        "Mname":"mesos-dns.mesos.",
        "Listener":"0.0.0.0",
        "HttpPort":8123,
        "DnsOn":true,
        "HttpOn":true
    }
    

# <a name="get-hosts"></a>`GET /v1/hosts/{host}`

Lists in JSON format the IP addresses that correspond to a hostname. It is the equivalent of a DNS A record lookup.

**Note:** The HTTP interface only resolves hostnames in the Mesos domain.

    $ curl http://10.190.238.173:8123/v1/hosts/nginx.marathon.mesos
    [
        {"host":"nginx.marathon.mesos.","ip":"10.249.219.155"},
        {"host":"nginx.marathon.mesos.","ip":"10.190.238.173"},
        {"host":"nginx.marathon.mesos.","ip":"10.156.230.230"}
    ]
    

# <a name="get-service"></a>`GET /v1/services/{service}`

Lists in JSON format the hostname, IP address, and ports that correspond to a hostname. It is the equivalent of a DNS SRV record lookup.

**Note:** The HTTP interface only resolves service names in the Mesos domain.

    curl http://10.190.238.173:8123/v1/services/_nginx._tcp.marathon.mesos.
    [
        {"host":"nginx-s2.marathon.mesos.","ip":"10.249.219.155","port":"31644","service":"_nginx._tcp.marathon.mesos."},
        {"host":"nginx-s1.marathon.mesos.","ip":"10.190.238.173","port":"31667","service":"_nginx._tcp.marathon.mesos."},
        {"host":"nginx-s0.marathon.mesos.","ip":"10.156.230.230","port":"31880","service":"_nginx._tcp.marathon.mesos."}
    ]