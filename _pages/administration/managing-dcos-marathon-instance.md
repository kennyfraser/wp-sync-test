---
UID: 56df3dedab11d
post_title: Managing DCOS Marathon
post_excerpt: ""
layout: page
published: true
menu_order: 11
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<h1>Specifying the Marathon environment variable as a CLI command</h1>

<p>In this example, the <a href="https://mesosphere.github.io/marathon/docs/task-environment-vars.html">Marathon <code>env</code> variable</a> is updated by specifying a JSON string in the command.</p>

<ol>
<li><p>Specify this CLI command with the JSON string included:</p>

<pre><code>dcos marathon app update test-app env='{"APISERVER_PORT":"25502"}'
</code></pre></li>
<li><p>Run the command below to see the result of your update:</p>

<pre><code>dcos marathon app show test-app | jq '.env'
</code></pre></li>
</ol>

<h1>Specifying the Marathon environment variable in a JSON file</h1>

<p>In this example, the <a href="https://mesosphere.github.io/marathon/docs/task-environment-vars.html">Marathon <code>env</code> variable</a> is updated by specifying a JSON file in the command.</p>

<ol>
<li><p>Save the existing environment variables to a file:</p>

<pre><code>dcos marathon app show test-app | jq .env &gt;env_vars.json
</code></pre>

<p>The file will look like this:</p>

<pre><code>    { "SCHEDULER_DRIVER_PORT": "25501", }
</code></pre></li>
<li><p>Edit the <code>env_vars.json</code> file. Make the JSON a valid object by enclosing the file contents with <code>{ "env" :}</code> and add your update:</p>

<pre><code>{ "env" : { "APISERVER_PORT" : "25502", "SCHEDULER_DRIVER_PORT" : "25501" } }
</code></pre></li>
<li><p>Specify this CLI command with the JSON file specified:</p>

<pre><code>dcos marathon app update test-app &lt; env_vars.json
</code></pre>

<p>View the results of your update:</p>

<pre><code>   dcos marathon app show test-app | jq '.env'
</code></pre></li>
</ol>

<h1>Displaying completed Marathon tasks</h1>

<p>In this example, all completed HDFS tasks are shown:</p>

<pre><code>$ dcos task --completed hdfs
 NAME  USER  STATE                      ID                    
 hdfs  root    F    hdfs.6b18a882-00a5-11e5-9926-56847afe9799 
 hdfs  root    K    hdfs.6eacf2d3-00a5-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.71165c45-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.74125e36-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.770e6027-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.7b3bdd38-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.7f698159-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.809b71aa-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.82fe19cb-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.86928b2c-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.8ac02f4d-00a6-11e5-9926-56847afe9799 
 hdfs  root    F    hdfs.8e54528e-00a6-11e5-9926-56847afe9799
</code></pre>