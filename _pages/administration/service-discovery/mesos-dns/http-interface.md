---
UID: 56df3deca5b21
post_title: HTTP Interface
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Mesos-DNS implements a simple REST API for service discovery over HTTP:</p>

<h1><a name="get-version"></a><code>GET /v1/version</code></h1>

<p>Lists in JSON format the Mesos-DNS version and source code URL.</p>

<pre><code>$ curl http://10.190.238.173:8123/v1/version
{
    "Service":"Mesos-DNS",
    "URL":"https://github.com/mesosphere/mesos-dns","Version":"0.1.1"
}
</code></pre>

<h1><a name="get-config"></a><code>GET /v1/config</code></h1>

<p>Lists in JSON format the Mesos-DNS configuration parameters.</p>

<pre><code>$ curl http://10.190.238.173:8123/v1/config
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
</code></pre>

<h1><a name="get-hosts"></a><code>GET /v1/hosts/{host}</code></h1>

<p>Lists in JSON format the IP addresses that correspond to a hostname. It is the equivalent of a DNS A record lookup.</p>

<p><strong>Note:</strong> The HTTP interface only resolves hostnames in the Mesos domain.</p>

<pre><code>$ curl http://10.190.238.173:8123/v1/hosts/nginx.marathon.mesos
[
    {"host":"nginx.marathon.mesos.","ip":"10.249.219.155"},
    {"host":"nginx.marathon.mesos.","ip":"10.190.238.173"},
    {"host":"nginx.marathon.mesos.","ip":"10.156.230.230"}
]
</code></pre>

<h1><a name="get-service"></a><code>GET /v1/services/{service}</code></h1>

<p>Lists in JSON format the hostname, IP address, and ports that correspond to a hostname. It is the equivalent of a DNS SRV record lookup.</p>

<p><strong>Note:</strong> The HTTP interface only resolves service names in the Mesos domain.</p>

<pre><code>curl http://10.190.238.173:8123/v1/services/_nginx._tcp.marathon.mesos.
[
    {"host":"nginx-s2.marathon.mesos.","ip":"10.249.219.155","port":"31644","service":"_nginx._tcp.marathon.mesos."},
    {"host":"nginx-s1.marathon.mesos.","ip":"10.190.238.173","port":"31667","service":"_nginx._tcp.marathon.mesos."},
    {"host":"nginx-s0.marathon.mesos.","ip":"10.156.230.230","port":"31880","service":"_nginx._tcp.marathon.mesos."}
]
</code></pre>