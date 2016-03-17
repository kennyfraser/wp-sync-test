---
UID: 56df3def49109
post_title: Package Repository
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>DCOS services are Mesosphere-certified applications that are packaged and available from the public DCOS package repositories that are hosted on GitHub. Available DCOS services include Mesosphere-certified <a href="https://github.com/mesosphere/universe">Mesos frameworks</a> and other applications. A Mesos framework is the combination of a Mesos scheduler and an optional custom executor.</p>

<p>DCOS services can be created by the community and by Mesosphere. The Mesosphere-certified services are <a href="../manage-service/cassandra/">Cassandra</a>, <a href="../manage-service/chronos/">Chronos</a>, <a href="../manage-service/hdfs/">HDFS</a>, <a href="../manage-service/kafka/">Kafka</a>, <a href="../manage-service/kubernetes/">Kubernetes</a>, <a href="../manage-service/marathon/">Marathon</a>, and <a href="../manage-service/spark/">Spark</a>.</p>

<p>You can install DCOS services on your cluster with a single command <code>dcos package install</code> after <a href="https://docs.mesosphere.com/administration/introcli/cli/">installing the DCOS Command-Line Interface</a>. DCOS offers the Universe and Multiverse package repositories.</p>

<h1>Universe</h1>

<p>DCOS Universe contains all services that have been certified by Mesosphere. For more information on DCOS Universe, see the <a href="https://github.com/mesosphere/universe">GitHub Universe repository</a>.</p>

<h1>Multiverse</h1>

<p>DCOS Multiverse contains experimental services that are still being tested and are not guaranteed to work properly with DCOS. Multiverse services are not recommended for production clusters. For more information on DCOS Multiverse, see the <a href="https://github.com/mesosphere/multiverse">GitHub Multiverse repository</a>.</p>

<p>For more information on installing services, see <a href="../manage-service/">Managing DCOS services</a>.</p>

<h1>Next steps</h1>

<p>See the <a href="../overview/tutorials/installdatacenter/">installing a DCOS service</a> tutorial.</p>