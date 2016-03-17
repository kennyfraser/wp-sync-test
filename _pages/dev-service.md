---
UID: 56df3defe274e
post_title: Developing Services
post_excerpt: ""
layout: page
published: true
menu_order: 39
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>This section describes the developer-specific DCOS components, explaining what is necessary to package and provide your own service on DCOS.</p>

<p>The Mesosphere Datacenter Operating System (DCOS) provides the optimal user experience possible for orchestrating and managing a datacenter. If you are an Apache Mesos developer, you are already familiar with developing a framework. DCOS extends Apache Mesos by including a web interface for health checks and monitoring, a command-line, a service packaging description, and a <a href="https://docs.mesosphere.com/overview/universe/">repository</a> that catalogs those packages. &lt;!-- ### 

<a name="integration"></a>DCOS Integration Points --&gt; <!-- 
There are a number of ways to extend the capabilities of the DCOS all of which center around datacenter services.  DCOS defines 2 types of services:

* **Native** - An Apache Mesos framework which requires registration with the `mesos-master`.
* **Non-Native** - A standard process that provides value to the cluster without an integration with Mesos. This seems like a standard user application deployed the distinction is this is an infrastructure service.

All of these services are deployed by Marathon and must be packaged and added to the DCOS package repository for deployment.  For Native applications, it is also possible to extend the CLI and web interface.  Non-Native service integration support is coming soon.  For the rest of this topic a native service is assumed.  All of these services require packaging and a service catalog.

 --></p>

<h1><a name="universe"></a>Package Repositories</h1>

<p>DCOS offers the Universe and Multiverse package repositories.</p>

<p><strong>Universe</strong> DCOS Universe contains all services that have been certified by Mesosphere to be GA. For more information on DCOS Universe, see the <a href="https://github.com/mesosphere/universe">GitHub Universe repository</a>.</p>

<p><strong>Multiverse</strong> DCOS Multiverse contains experimental services that are still being tested and are not guaranteed to work properly with DCOS. Multiverse services are not recommended for production clusters. For more information on DCOS Multiverse, see the <a href="https://github.com/mesosphere/multiverse">GitHub Multiverse repository</a>.</p>

<p>All services in the package repositories are required to meet a certain standard as defined by Mesosphere. For details on submitting a DCOS service, see <a href="https://github.com/mesosphere/universe#contributing-a-package">Contributing a package</a>.</p>

<h1><a name="adminrouter"></a>Admin Router and web interface integration</h1>

<p>By default, a DCOS service is deployed on a <a href="/administration/dcosarchitecture/#scrollNav-2">private agent node</a>. To allow configuration control or monitoring of a service by a user, the admin router proxies calls on the master node to the service in a private node on the cluster. The HTTP service endpoint requires relative paths for artifacts and resources. The service endpoint can provide a web interface, a RESTful endpoint, or both. When creating a DCOS CLI subcommand it is common to have a RESTful endpoint to communicate with the scheduler service.</p>

<p>The integration to the admin router is automatic when a framework scheduler registers a <code>webui_url</code> during the registration process with the Mesos master. There are a couple of limitations:</p>

<ul>
<li>The URL must NOT end with a backslash (/). For example, this is good <code>internal.dcos.host.name:10000</code>, and this is bad <code>internal.dcos.host.name:10000/</code>.</li>
<li>DCOS supports 1 URL and port.</li>
</ul>

<p>When the <code>webui_url</code> is provided, the service is listed on the DCOS web interface as a service with a link. That link is the admin router proxy URL name that is based on a naming convention of: <code>/service/&lt;service_name&gt;</code>. For example, <code>&lt;dcos_host&gt;/service/unicorn</code> is the proxy to the <code>webui_url</code>. If you provide a web interface, it will be integrated with the DCOS web interface and users can click the link for quick access to your service.</p>

<p>Service health check information is provided from the DCOS service tab when:</p>

<ul>
<li>There are service health checks defined in the <code>marathon.json</code> file. For example:</li>
</ul>

<blockquote>
<pre><code> "healthChecks": [
 {
   "path": "/",
   "portIndex": 1,
   "protocol": "HTTP",
   "gracePeriodSeconds": 5,
   "intervalSeconds": 60,
   "timeoutSeconds": 10,
   "maxConsecutiveFailures": 3
 
</code></pre>
</blockquote>

<ul>
<li><p>The <code>framework-name</code> property in the <code>marathon.json</code> file is valid. For example:</p>

<pre><code>  "id": "{{kafka.framework-name}}"
</code></pre></li>
<li><p>The framework property in the <code>package.json</code> file is set to true. For example:</p>

<pre><code>  "framework": true
</code></pre></li>
</ul>

<p>You can provide public access to your service through the admin router or by deploying your own proxy or router to the public agent node. It is recommend to use the admin router for scheduler configuration and control allowing integration with the DCOS web interface. It is also recommended to provide a CLI subcommand for command-line control of a RESTful service endpoint for the scheduler.</p>

<h1>DCOS Service structure</h1>

<p>Each DCOS service contains <code>package.json</code>, <code>config.json</code>, and <code>marathon.json</code> files. The contents of these files are described in the DCOS Service specification.</p>

<!-- This information should be replaced with link to service spec. JSH 11/23/15 -->

<ul>
<li><p><strong>package.json</strong></p>

<ul>
<li>The <code>"name": "cassandra",</code> parameter specified here defines the DCOS service name in the package repository. The must be the first parameter in the file. </li>
<li>Focus the description on your service. Assume that all users are familiar with DCOS and Mesos.</li>
<li><p>The <code>tags</code> parameter is used for user searches (<code>dcos package search &lt;criteria&gt;</code>). Add tags that distinguish your service in some way. Avoid the following terms: Mesos, Mesosphere, DCOS, and datacenter. For example, the unicorns service could have:</p>

<pre><code>  "tags": ["rainbows", "mythical"]
</code></pre></li>
<li><p>The <code>preInstallNotes</code> parameter gives the user information they'll need before starting the installation process. For example, you could explain what the resource requirements are for your service.</p>

<pre><code>  "preInstallNotes":"Unicorns take 7 nodes with 1 core each and 1TB of ram."
</code></pre></li>
<li><p>The <code>postInstallNotes</code> parameter gives the user information they'll need after the installation. Focus on providing a documentation URL, a tutorial, or both. For example:</p>

<pre><code>  "postInstallNotes": "Thank you for installing the Unicorn service.nntDocumentation: http://&lt;your-url&gt;ntIssues: https://github.com/",
</code></pre></li>
<li><p>The <code>postUninstallNotes</code> parameter gives the user information they'll need after an uninstall. For example, further cleanup before reinstalling again and a link to the details. A common issue is cleaning up ZooKeeper entries. For example:</p>

<pre><code>  postUninstallNotes": "The Unicorn DCOS Service has been uninstalled and will no longer run.nPlease follow the instructions at http://&lt;your-URL&gt; to clean up any persisted state" }
</code></pre></li>
</ul></li>
<li><p><strong>config.json</strong></p>

<ul>
<li>The requirement block is for all properties that are required by the marathon.json file without a condition block (it is NOT properties that are not provided and thus must be supplied by the user)</li>
</ul></li>
<li><p><strong>marathon.json</strong></p>

<ul>
<li><p>A second-level (nested) property must be the framework-name with a value of the service name. For example:</p>

<pre><code>  "framework-name" : "{{unicorn.framework-name}}"
</code></pre></li>
<li><p>Use the same value for the id parameter. For example:</p>

<pre><code>  "id" : "{{unicorn.framework-name}}"
</code></pre></li>
<li><p>All URLs used by the service must be passed to the service by using command line or environment variable</p></li>
</ul></li>
</ul>

<p><strong>NOTE</strong>: All services submitted to the DCOS package repositories are required to use versioned artifacts that do not change.</p>

<h1>Creating a DCOS Service</h1>

<p>Here is a detailed developer workflow for creating a DCOS service:</p>

<ol>
<li><p>Fork the Universe repository.</p></li>
<li><p>Create a DCOS service in your local repository.</p>

<ol>
<li><p>Name your service. For example, <code>unicorn</code>.</p>

<p>The DCOS package repository directory structure is:</p>

<pre><code>  repo/packages/&lt;initial-letter&gt;/&lt;service-name&gt;/&lt;version&gt;
</code></pre>

<p>Where:</p></li>
</ol>

<ul>
<li><code>&lt;initial-letter&gt;</code> is the uppercase first letter of your service name. </li>
<li><code>&lt;service-name&gt;</code> is the lowercase service name. Do not use keywords such as Apache, Mesos or DCOS in your service name.</li>
<li><code>&lt;version&gt;</code> is the service version number. </li>
</ul>

<ol>
<li>Create a directory under <code>repo/packages</code> for your service. For example, <code>repo/packages/U/unicorn</code>.</li>
<li>Create a version index directory. For example, <code>repo/packages/U/unicorn/0</code>.</li>
<li>Add <code>package.json</code>, <code>config.json</code>, and <code>marathon.json</code> to your index directory.</li>
<li>If you have a CLI subcommand, create a <code>command.json</code> file and add to your index directory.</li>
</ol></li>
<li><p>Test your service on DCOS:</p>

<ol>
<li><p>Configure DCOS to point to your local repository. For example, if your forked repository at <code>https://github.com/mesosphere/altuniverse</code> and using the <code>version-1.x</code> branch, add it to your DCOS configuration with this command:</p>

<pre><code>$ dcos config prepend package.sources https://github.com/mesosphere/altuniverse/archive/version-1.x.zip
</code></pre></li>
<li><p>Update your DCOS package repository with this command:</p>

<pre><code>$ dcos package update
</code></pre></li>
</ol></li>
</ol>

<h1>Naming and directory structure</h1>

<p>After you add the JSON files to the index folder, there are scripts under the <code>&lt;universe&gt;/scripts</code> directory.</p>

<p>Run the package repository scripts in numerical order. If a script passes you can move on to the next script.</p>

<ol>
<li><p>Run the <code>0-validate-version.sh</code> script to validate the versioning.</p></li>
<li><p>Run the <code>1-validate-packages.sh</code> script to validate the <code>command.json</code>, <code>config.json</code>, and <code>package.json</code> files against the schema.</p></li>
<li><p>Run the <code>2-build-index.sh</code> script to add your DCOS service to the <code>index.json</code> file.</p></li>
<li><p>Run the <code>3-validate-index.sh</code> script to validate the <code>index.json</code> file.</p></li>
</ol>

<p>For more information about the JSON files, see the <a href="https://github.com/mesosphere/universe">Universe Readme</a> page. &lt;!-- ### 

<a name="dcoscli"></a>DCOS CLI The command.json schema access the service endpoint Developer notes: There currently is no support for service dependencies over riding the framework-name (service endpoint)? --&gt;</p>