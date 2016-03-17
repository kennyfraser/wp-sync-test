---
UID: 56df3def870b0
post_title: Filtering Task Logs with Splunk
post_excerpt: ""
layout: page
published: true
menu_order: 6
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The file system paths of DCOS task logs contain information such as the agent ID, framework ID, and executor ID. You can use this information to filter the log output for specific tasks, applications, or agents.</p>

<p><strong>Prerequisite</strong></p>

<ul>
<li><a href="/logging/splunk/">A Splunk installation that aggregates DCOS logs</a></li>
</ul>

<h1><a name="configuration"></a>Configuration</h1>

<p>You can configure Splunk either by using the Splunk <a href="#splunkui">Web UI</a> or by editing the <a href="#propsconf">props.conf file</a>.</p>

<h4><a name="splunkui"></a>Splunk Web UI</h4>

<ol>
<li>Navigate to Settings &gt; Fields &gt; Field Extractions &gt; New.</li>
<li><p>Fill out the form with the following:</p>

<ul>
<li>Destination app: <code>search</code></li>
<li>Name: <code>dcos_task</code> (or any unique, meaningful name for the extraction)</li>
<li>Apply to <code>source</code> named <code>/var/lib/mesos/slave/...</code></li>
<li>Type: <code>Inline</code></li>
<li><p>Extraction/Transform:</p>

<pre><code>/var/lib/mesos/slave/slaves/(?&lt;agent&gt;[^/]+)/frameworks/(?&lt;framework&gt;[^/]+)/executors/(?&lt;executor&gt;[^/]+)/runs/(?&lt;run&gt;[^/]+)/.* in source
</code></pre></li>
</ul></li>
<li><p>Click "Save".</p></li>
<li><p>In the "Field Extractions" view, find the extraction you just created and set the permissions appropriately.</p></li>
</ol>

<p>The <code>agent</code>, <code>framework</code>, <code>executor</code>, and <code>run</code> fields should now be available to use in search queries and appear in the fields associated with Mesos task log events.</p>

<h4><a name="propsconf"></a>props.conf</h4>

<ol>
<li><p>Add the following entry to <code>props.conf</code> (see the <a href="http://docs.splunk.com/Documentation/Splunk/latest/admin/Propsconf">Splunk documentation</a> for details):</p>

<pre><code>[source::/var/lib/mesos/slave/...]
EXTRACT = /var/lib/mesos/slave/slaves/(?&lt;agent&gt;[^/]+)/frameworks/(?&lt;framework&gt;[^/]+)/executors/(?&lt;executor&gt;[^/]+)/runs/(?&lt;run&gt;[^/]+)/.* in source
</code></pre></li>
<li><p>Run the following search in the Splunk Web UI to ensure the changes take effect:</p>

<pre><code>extract reload=true
</code></pre></li>
</ol>

<p>The <code>agent</code>, <code>framework</code>, <code>executor</code>, and <code>run</code> fields should now be available to use in search queries and appear in the fields associated with Mesos task log events.</p>

<h1><a name="usage"></a>Usage Example</h1>

<p>For example, in the Splunk web UI, you can type <code>framework=*</code> into the Search field. This will show all of the events where the <code>framework</code> field is defined:</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-exists.png" rel="attachment wp-att-1592"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-exists.png" alt="splunk-framework-exists" width="2826" height="1246" class="alignnone size-full wp-image-1592" /></a></p>

<p>Click the disclosure triangle next to one of these events to view the details. This will show all of the fields extracted from the task log file path:</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-fields.png" rel="attachment wp-att-1591"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-fields.png" alt="splunk-fields" width="2297" height="727" class="alignnone size-full wp-image-1591" /></a></p>

<p>Finally, let's search for all of the events that reference the framework ID of the event shown in the screenshot above, but that do not contain the chosen <code>framework</code> field. This will show us only non-task results:</p>

<p><a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-search.png" rel="attachment wp-att-1593"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-search.png" alt="splunk-framework-search" width="2832" height="1327" class="alignnone size-full wp-image-1593" /></a></p>

<h1><a name="templates"></a>Template Examples</h1>

<p>Here are example query templates for aggregating the DCOS logs with Splunk. Replace the template parameters <code>$executor1</code>, <code>$framework2</code>, and any others with actual values from your cluster.</p>

<p><strong>Important:</strong> Do not change the quotation marks in these examples or the queries will not work. If you create custom queries, be careful with the placement of quotation marks.</p>

<ul>
<li><p>Logs related to a specific executor <code>$executor1</code>, including logs for tasks run from that executor:</p>

<pre><code>"$executor1"
</code></pre></li>
<li><p>Non-task logs related to a specific executor <code>$executor1</code>:</p>

<pre><code>"$executor1" AND NOT executor=$executor1
</code></pre></li>
<li><p>Logs (including task logs) for a framework <code>$framework1</code>, if <code>$executor1</code> and <code>$executor2</code> are that framework's executors:</p>

<pre><code>"$framework1" OR "$executor1" OR "$executor2"
</code></pre></li>
<li><p>Non-task logs for a framework <code>$framework1</code>, if <code>$executor1</code> and <code>$executor2</code> are that framework's executors:</p>

<pre><code>("$framework1" OR "$executor1" OR "$executor2") AND NOT (framework=$framework1 OR executor=$executor1 OR executor=$executor2)
</code></pre></li>
<li><p>Logs for a framework <code>$framework1</code> on a specific agent host <code>$agent_host1</code>:</p>

<pre><code>host=$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2")
</code></pre></li>
<li><p>Non-task logs for a framework <code>$framework1</code> on a specific agent <code>$agent1</code> with host <code>$agent_host1</code>:</p>

<pre><code>host=$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2") AND NOT agent=$agent
</code></pre></li>
</ul>