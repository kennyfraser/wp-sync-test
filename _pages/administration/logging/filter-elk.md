---
UID: 56df3def9306e
post_title: Filtering Task Logs with ELK
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The file system paths of DCOS task logs contain information such as the agent ID, framework ID, and executor ID. You can use this information to filter the log output for specific tasks, applications, or agents.</p>

<p><strong>Prerequisite</strong></p>

<ul>
<li><a href="/logging/elk/">An ELK stack that aggregates DCOS logs</a></li>
</ul>

<h1><a name="configuration"></a>Configuration</h1>

<ol>
<li><p>Create the following <code>dcos</code> pattern file in your custom patterns directory located at <code>$PATTERNS_DIR</code>:</p>

<pre><code>PATHELEM [^/]+
TASKPATH ^/var/lib/mesos/slave/slaves/%{PATHELEM:agent}/frameworks/%{PATHELEM:framework}/executors/%{PATHELEM:executor}/runs/%{PATHELEM:run}
</code></pre></li>
<li><p>Update the configuration file for your Logstash instance to include the following <code>grok</code> filter, where <code>$PATTERNS_DIR</code> is replaced with your custom patterns directory:</p>

<pre><code>filter {
    grok {
        patterns_dir =&gt; "$PATTERNS_DIR"
        match =&gt; { "file" =&gt; "%{TASKPATH}" }
    }
}
</code></pre></li>
<li><p>Restart Logstash.</p>

<p>Logstash will extract the <code>agent</code>, <code>framework</code>, <code>executor</code>, and <code>run</code> fields. These fields are shown in the metadata of all Mesos task log events. Elasticsearch queries will also show results from those fields.</p></li>
</ol>

<h1><a name="usage"></a>Usage Example</h1>

<p>In the screenshots below, we are using Kibana hosted by <a href="http://logz.io">logz.io</a>, but your Kibana interface will look similar.</p>

<p>For example, you can type <code>framework:*</code> into the Search field. This will show all of the events where the <code>framework</code> field is defined:</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-exists.png" rel="attachment wp-att-1530"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-exists.png" alt="logstash-framework-exists" width="2839" height="1246" class="alignnone size-full wp-image-1530" /></a></p>

<p>Click the disclosure triangle next to one of these events to view the details. This will show all of the fields extracted from the task log file path:</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-fields.png" rel="attachment wp-att-1529"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-fields.png" alt="logstash-fields" width="2318" height="922" class="alignnone size-full wp-image-1529" /></a></p>

<p>Finally, let's search for all of the events that reference the framework ID of the event shown in the screenshot above, but that do not contain the chosen <code>framework</code> field. This will show only non-task results:</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-search.png" rel="attachment wp-att-1531"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-search.png" alt="logstash-framework-search" width="2844" height="1361" class="alignnone size-full wp-image-1531" /></a></p>

<h1><a name="templates"></a>Template Examples</h1>

<p>Here are some example query templates. Replace the template parameters <code>$executor1</code>, <code>$framework2</code>, and any others with the actual values from your cluster.</p>

<p><strong>Important:</strong> Do not change the quotation marks in these examples or the queries will not work. If you create custom queries, be careful with the placement of quotation marks.</p>

<ul>
<li><p>Logs related to a specific executor <code>$executor1</code>, including logs for tasks run from that executor:</p>

<pre><code>"$executor1"
</code></pre></li>
<li><p>Non-task logs related to a specific executor <code>$executor1</code>:</p>

<pre><code>"$executor1" AND NOT executor:$executor1
</code></pre></li>
<li><p>Logs (including task logs) for a framework <code>$framework1</code>, if <code>$executor1</code> and <code>$executor2</code> are that framework's executors:</p>

<pre><code>"$framework1" OR "$executor1" OR "$executor2"
</code></pre></li>
<li><p>Non-task logs for a framework <code>$framework1</code>, if <code>$executor1</code> and <code>$executor2</code> are that framework's executors:</p>

<pre><code>("$framework1" OR "$executor1" OR "$executor2") AND NOT (framework:$framework1 OR executor:$executor1 OR executor:$executor2)
</code></pre></li>
<li><p>Logs for a framework <code>$framework1</code> on a specific agent host <code>$agent_host1</code>:</p>

<pre><code>host:$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2")
</code></pre></li>
<li><p>Non-task logs for a framework <code>$framework1</code> on a specific agent <code>$agent1</code> with host <code>$agent_host1</code>:</p>

<pre><code>host:$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2") AND NOT agent:$agent
</code></pre></li>
</ul>