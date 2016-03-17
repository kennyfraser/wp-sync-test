---
UID: 56df3dec9919f
post_title: Service Naming
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Mesos-DNS defines the DNS top-level domain <code>.mesos</code> for Mesos tasks that are running on DCOS. Tasks and services are discovered by looking up A and, optionally, SRV records within this Mesos domain.</p>

<h1><a name="a-records"></a>A Records</h1>

<p>An A record associates a hostname to an IP address.</p>

<p>When a task is launched by a DCOS service, Mesos-DNS generates an A record for a hostname in the format <code>&lt;task&gt;.&lt;service&gt;.mesos</code> that provides one of the following:</p>

<ul>
<li>The IP address of the <a href="../../terminology/">agent node</a> that is running the task</li>
<li>The IP address of the task's network container (provided by a Mesos containerizer)</li>
</ul>

<p>For example, other DCOS tasks can discover the IP address for a task named <code>search</code> launched by the <code>marathon</code> service with a lookup for <code>search.marathon.mesos</code>:</p>

<pre><code>$ dig search.marathon.mesos

; &lt;&lt;&gt;&gt; DiG 9.8.4-rpz2+rl005.12-P1 &lt;&lt;&gt;&gt; search.marathon.mesos
;; global options: +cmd
;; Got answer:
;; -&gt;&gt;HEADER&lt;&lt;- opcode: QUERY, status: NOERROR, id: 24471
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 0

;; QUESTION SECTION:
;search.marathon.mesos.         IN  A

;; ANSWER SECTION:
search.marathon.mesos.      60  IN  A   10.9.87.94
</code></pre>

<p>If the Mesos containerizer that launches the task provides a container IP <code>10.0.4.1</code> for the task <code>search.marathon.mesos</code>, then the lookup result is:</p>

<pre><code>$ dig search.marathon.mesos

; &lt;&lt;&gt;&gt; DiG 9.8.4-rpz2+rl005.12-P1 &lt;&lt;&gt;&gt; search.marathon.mesos
;; global options: +cmd
;; Got answer:
;; -&gt;&gt;HEADER&lt;&lt;- opcode: QUERY, status: NOERROR, id: 24471
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 0

;; QUESTION SECTION:
;search.marathon.mesos.         IN  A

;; ANSWER SECTION:
search.marathon.mesos.      60  IN  A   10.0.4.1
</code></pre>

<p>In addition to the <code>&lt;task&gt;.&lt;service&gt;.mesos</code> syntax shown above, Mesos-DNS also generates A records that contain the IP addresses of the agent nodes that are running the task: <code>&lt;task&gt;.&lt;service&gt;.slave.mesos</code>.</p>

<p>For example, a query of the A records for <code>search.marathon.slave.mesos</code> shows the IP address of each agent node running one or more instances of the <code>search</code> application on the <code>marathon</code> service.</p>

<h1><a name="srv-records"></a>SRV Records</h1>

<p>An SRV record specifies the hostname and port of a service.</p>

<p>For a task named <code>mytask</code> launched by a service named <code>myservice</code>, Mesos-DNS generates an SRV record <code>_mytask._protocol.myservice.mesos</code>, where <code>protocol</code> is <code>udp</code> or <code>tcp</code>.</p>

<p>For example, other Mesos tasks can discover a task named <code>search</code> launched by the <code>marathon</code> service with a query for <code>_search._tcp.marathon.mesos</code>:</p>

<pre><code>$ dig _search._tcp.marathon.mesos SRV

;  DiG 9.8.4-rpz2+rl005.12-P1 &amp;lt;&amp;lt;&amp;gt;&amp;gt; _search._tcp.marathon.mesos SRV
;; global options: +cmd
;; Got answer:
;; -&amp;gt;&amp;gt;HEADER&amp;lt;&amp;lt;- opcode: QUERY, status: NOERROR, id: 33793
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;_search._tcp.marathon.mesos.   IN SRV

;; ANSWER SECTION:
_search._tcp.marathon.mesos.    60 IN SRV 0 0 31302 10.254.132.41.
</code></pre>

<p>Mesos-DNS supports the use of a task's DiscoveryInfo for SRV record generation.</p>

<p>On a DCOS cluster, ports are offered by agent nodes in the same way as other resources such as CPU and memory. If DiscoveryInfo is not available, Mesos-DNS uses the ports that were allocated for the task.</p>

<p>The following table shows the rules that govern SRV generation:</p>

<table class="table" style="width: 400px !important">
  <thead>
    <tr>
      <th>
        Service
      </th>
      
      <th>
        Container IP Known
      </th>
      
      <th>
        DiscoveryInfo Provided
      </th>
      
      <th>
        Target Host
      </th>
      
      <th>
        Target Port
      </th>
      
      <th>
        A Record Target IP
      </th>
    </tr>
  </thead>
  
  <tbody>
    <tr>
      <td>
        _mytask._protocol.myservice.mesos
      </td>
      
      <td>
        No
      </td>
      
      <td>
        No
      </td>
      
      <td>
        mytask.myservice.slave.mesos
      </td>
      
      <td>
        Host Port
      </td>
      
      <td>
        Agent IP
      </td>
    </tr>
    
    <tr>
      <td>
        _mytask._protocol.myservice.mesos
      </td>
      
      <td>
        Yes
      </td>
      
      <td>
        No
      </td>
      
      <td>
        mytask.myservice.slave.mesos
      </td>
      
      <td>
        Host Port
      </td>
      
      <td>
        Agent IP
      </td>
    </tr>
    
    <tr>
      <td>
        _mytask._protocol.myservice.mesos
      </td>
      
      <td>
        No
      </td>
      
      <td>
        Yes
      </td>
      
      <td>
        mytask.myservice.mesos
      </td>
      
      <td>
        DiscoveryInfo Port
      </td>
      
      <td>
        Agent IP
      </td>
    </tr>
    
    <tr>
      <td>
        _mytask._protocol.myservice.mesos
      </td>
      
      <td>
        Yes
      </td>
      
      <td>
        Yes
      </td>
      
      <td>
        mytask.myservice.mesos
      </td>
      
      <td>
        DiscoveryInfo Port
      </td>
      
      <td>
        Container IP
      </td>
    </tr>
    
    <tr>
      <td>
        mytask.protocol.myservice.slave.mesos
      </td>
      
      <td>
        N/A
      </td>
      
      <td>
        N/A
      </td>
      
      <td>
        mytask.myservice.slave.mesos
      </td>
      
      <td>
        Host Port
      </td>
      
      <td>
        Agent IP
      </td>
    </tr>
  </tbody>
</table>

<h1><a name="other-records"></a>Other Records</h1>

<p>Mesos-DNS generates a few special records:</p>

<ul>
<li>For the leading master: A record (<code>leader.mesos</code>) and SRV records (<code>_leader._tcp.mesos</code> and <code>_leader._udp.mesos</code>)</li>
<li>For all service schedulers: A records (<code>myservice.mesos</code>) and SRV records (<code>_myservice._tcp.myservice.mesos</code>)</li>
<li>For every known DCOS master: A records (<code>master.mesos</code>) and SRV records (<code>_master._tcp.mesos</code> and <code>_master._udp.mesos</code>)</li>
<li>For every known DCOS agent: A records (<code>slave.mesos</code>) and SRV records (<code>_slave._tcp.mesos</code>)</li>
</ul>

<p><strong>Important:</strong> To query the leading master node, always query <code>leader.mesos</code>, not <code>master.mesos</code>. See <a href="https://docs.mesosphere.com/administration/service-discovery/faq-troubleshooting/#leader">this FAQ entry</a> for more information.</p>

<p>There is a delay between the election of a new master and the update of leader/master records in Mesos-DNS.</p>

<p>Mesos-DNS also supports requests for SOA and NS records for the Mesos domain. DNS requests for records of other types in the Mesos domain will return <code>NXDOMAIN</code>. Mesos-DNS does not support PTR records needed for reverse lookups.</p>

<p>Mesos-DNS also generates A records for itself that list all the IP addresses that Mesos-DNS will answer lookup requests on. The hostname for these A records is <code>ns1.mesos</code>.</p>

<h1><a name="naming-conventions"></a>Task and Service Naming Conventions</h1>

<p>Mesos-DNS follows <a href="https://tools.ietf.org/html/rfc952">RFC 952</a> for name formatting. All fields used to construct hostnames for A records and service names for SRV records must be 24 characters or shorter and can include letters of the alphabet (A-Z), numbers (0-9), and a dash (-). Names are not case sensitive. If the task name does not comply with these constraints, Mesos-DNS will shorten the name to 24 characters, remove all invalid characters, and replace periods (.) with a dash (-).</p>

<p>Note that there is a difference in the rules for service names and task names. For service names, periods (.) are allowed, but all other rules apply. For example, a task named <code>apiserver.myservice</code> launched by service <code>marathon.prod</code> will have A records associated with the name <code>apiserver-myservice.marathon.prod.mesos</code> and SRV records associated with the name <code>_apiserver-myservice._tcp.marathon.prod.mesos</code>.</p>

<p>Some services register with default names that are difficult to understand. For example, older versions of Marathon may register with names such as <code>marathon-0.7.5</code>, which will lead to a Mesos-DNS hostname such as <code>search.marathon-0.7.5.mesos</code>. You can avoid this problem by launching services with customized names. For example, launch Marathon with <code>--framework_name marathon</code> to register the service as <code>marathon</code>.</p>

<p>If you are using Marathon groups, the Mesos-DNS hostname is created from the app ID. For example, if you have an app named <code>nginx-router</code> and it is within the <code>mesosphere-tutorial</code> group with an app ID of <code>/mesosphere-tutorial/nginx-router</code>, then the DNS name will be <code>nginx-router-mesosphere-.marathon.mesos</code>. Note that Mesos-DNS truncated the hostname to 24 characters and substituted a dash for the slash between <code>mesosphere-tutorial</code> and <code>nginx-router</code>.</p>

<p>If a service launches multiple tasks with the same name, the DNS lookup will return multiple records, one per task. Mesos-DNS randomly shuffles the order of records to provide rudimentary load balancing between these tasks.</p>

<p><strong>Caution:</strong> It is possible to have a name collision if <em>different</em> services launch tasks that have the same hostname. If different services launch tasks with identical Mesos-DNS hostnames, or if Mesos-DNS truncates app IDs to create identical Mesos-DNS hostnames, applications will communicate with the wrong agent nodes and fail unpredictably.</p>