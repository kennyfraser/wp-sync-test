---
UID: 56df3deed04a9
post_title: Deploying Crate as a DCOS Service
post_excerpt: ""
layout: page
published: true
menu_order: 27
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p><strong>Disclaimer:</strong> The Crate service is available at the alpha level and not recommended for Mesosphere DCOS production systems.</p>

<p>Crate is a distributed NoSQL database that you can use to query and compute data with SQL in real-time. It provides a distributed aggregation engine, native search, and super simple scalability. Crate's shared-nothing-architecture means it is optimized for distributed environments that allow horizontal scaling of applications.</p>

<p>The Crate DCOS service is <a href="https://crate.io/docs/support/" target="_blank">supported</a> by a Crate Technology Gmbh.</p>

<ul>
<li><a href="#install">Installing Crate on DCOS</a></li>
<li><a href="#usage">Usage Examples</a></li>
<li><a href="#uninstall">Uninstalling Crate</a></li>
</ul>

<h2><a name="install"></a>Installing Crate on DCOS</h2>

<dl>
<dt>Prerequisite</dt>
<dd>The DCOS CLI must be <a href="/install/cli/">installed</a>.</dd>
</dl>

<p>To install Crate using the DCOS CLI:</p>

<ol>
<li><p>Change the DCOS package repository source to <a href="/overview/universe/">Multiverse</a> and make sure the repository content is current:</p>

<pre><code>$ dcos config prepend package.sources https://github.com/mesosphere/multiverse/archive/version-2.x.zip
$ dcos package update
</code></pre></li>
<li><p>Install Crate with this command:</p>

<pre><code>$ dcos package install crate
</code></pre></li>
<li><p>Verify that Crate is successfully installed and running:</p>

<ul>
<li><p>From the DCOS CLI:</p>

<p>$ dcos package list NAME VERSION APP COMMAND DESCRIPTION crate 0.1.0 /crate --- A Mesos Framework that allows running and resizing one or multiple Crate database clusters.</p></li>
<li><p>From the DCOS web interface, go to the <strong>Services</strong> tab and confirm that Crate is running:</p></li>
</ul></li>
</ol>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/cratetask.png" rel="attachment wp-att-1515"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/cratetask-800x47.png" alt="cratetask" width="800" height="47" class="alignnone size-large wp-image-1515" /></a></p>

<h2><a name="usage"></a>Usage Examples</h2>

<p>After installing, only a single Crate task is running on the DCOS cluster. To launch the Crate cluster you must use the Framework API and have enough resources to add Crate instances.</p>

<ul>
<li><a href="#launch">Launching/Resizing the Cluster</a></li>
<li><a href="#shutdown">Shutting Down the Cluster</a></li>
<li><a href="#multiple">Installing Multiple Crate Clusters</a></li>
</ul>

<h3><a name="launch"></a>Launching and Resizing the Cluster</h3>

<p><strong>Prerequisite:</strong> You must <a href="../administration/sshcluster/">SSH into the agent node</a> that is running the Crate service.</p>

<p>To launch instances, POST the number of desired instances to the <code>/resize</code> endpoint:</p>

<pre><code>$ curl -X POST localhost:4040/cluster/resize 
      -H "Content-Type: application/json" 
      -d '{"instances": 3}'
</code></pre>

<h3><a name="shutdown"></a>Shutting Down the Cluster</h3>

<p><strong>Prerequisite:</strong> You must <a href="../administration/sshcluster/">SSH into the agent node</a> that is running the Crate service.</p>

<ol>
<li><p>To shut down the cluster, POST to the <code>/shutdown</code> endpoint:</p>

<pre><code>$ curl -X POST localhost:4040/cluster/shutdown
</code></pre>

<p>This immediately shuts down all running Crate nodes of the cluster.</p></li>
</ol>

<h3><a name="multiple"></a>Installing Multiple Crate Clusters</h3>

<p>A single instance of the Crate framework can only run a single Crate cluster. To install multiple Crate clusters, specify unique framework names for each cluster in your options file.</p>

<p><strong>Prerequisite:</strong> You must <a href="../administration/sshcluster/">SSH into the agent node</a> that is running the Crate service.</p>

<ol>
<li><p>Add <code>crate.framework-name</code> and <code>crate.cluster-name</code> to your options file. You must use a unique name for each cluster and set each framwork API port to a unique and available port (default is 4040). This is not exposed but is required to route the DCOS endpoints to the correct framework instance.</p>

<pre><code>{
  "crate": {
    "version": "0.50.2",
    "framework-name": "crate-2",
    "cluster-name": "my-cluster"
    "framework": {
      "api-port": 4041
    }
  }
}
</code></pre></li>
<li><p>Run the install command with the DCOS CLI:</p>

<pre><code>$ dcos package install crate --options=crate-options.json
</code></pre></li>
</ol>

<h2><a name="uninstall"></a>Uninstalling Crate</h2>

<ol>
<li><p>From the DCOS CLI, enter this command::</p>

<pre><code>$ dcos package uninstall crate
</code></pre>

<p>If you have installed multiple Crate frameworks you will need to provide the <code>--app-id</code> option. For example:</p>

<pre><code>$ dcos package uninstall crate --app-id /crate-2
</code></pre></li>
<li><p>Open the Zookeeper Exhibitor web interface at <code>&lt;hostname&gt;/exhibitor</code>, where <code>&lt;hostname&gt;</code> is the <a href="/install/awscluster#launchdcos">Mesos Master hostname</a>.</p>

<ol>
<li><p>Click on <strong>Explorer</strong> tab and select the desired app folder (<code>crate</code>) and click <strong>Modify</strong> at the bottom of the explorer.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/screenshot-delete-zookeeper.png" rel="attachment wp-att-1581"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/screenshot-delete-zookeeper-800x675.png" alt="screenshot-delete-zookeeper" width="800" height="675" class="alignnone size-large wp-image-1581" /></a></p></li>
<li><p>Choose Type <strong>Delete</strong>, enter the required <strong>Username</strong>, <strong>Ticket/Code</strong>, and <strong>Reason</strong> fields, and click <strong>Next</strong>.</p></li>
<li><p>Click <strong>OK</strong> to confirm your deletion.</p></li>
</ol></li>
<li><p>Optional: Clear your data directories. By default the Crate DCOS Service data and log directories are written into the Mesos task sandbox. The Mesos task sandbox is automatically purged periodically. You can change this by setting the <code>crate.data-directory</code> option when you install the Crate DCOS Service. You can find more information about how to use custom, persistent data paths in the <a href="https://github.com/crate/crate-mesos-framework#persistent-data-paths">Crate framework documentation</a>.</p></li>
</ol>

<h2>Links</h2>

<p>Crate is <a href="https://crate.io/docs/support/" target="_blank">supported</a> by a Crate Technology Gmbh. For information about Crate enterprise edition, see <a href="https://crate.io/enterprise/" target="_blank">https://crate.io/enterprise/</a> or contact <a href="mailto:&#115;&#x61;&#x6c;&#101;&#x73;&#064;&#x63;&#x72;&#097;&#x74;&#101;&#x2e;&#x69;&#111;">&#115;&#x61;&#x6c;&#101;&#x73;&#064;&#x63;&#x72;&#097;&#x74;&#101;&#x2e;&#x69;&#111;</a>.</p>

<ul>
<li><a href="https://crate.io">https://crate.io</a></li>
<li><a href="https://github.com/crate/crate-mesos-framework/blob/master/README.rst">Framework Documentation</a></li>
<li><a href="https://crate.io/docs/en/stable/">Crate Documentation</a></li>
</ul>