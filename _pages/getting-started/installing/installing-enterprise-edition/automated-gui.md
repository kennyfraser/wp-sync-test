---
UID: 56df3deb71a93
post_title: Automated GUI installation
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The automated GUI installation method provides a simple graphical interface that guides you through the installation of DCOS Enterprise Edition.</p>

<p>This installation method uses a bootstrap node to administer the DCOS installation across your cluster. The bootstrap node uses an SSH key to connect to each node in your cluster to automate the DCOS installation.</p>

<p><strong>Important:</strong> This installation method supports a minimal DCOS configuration set that includes ZooKeeper for shared storage and a static master list, and publicly accessible master IP addresses.</p>

<p>To use the automated GUI installation method:</p>

<ul>
<li>Cluster nodes must be network accessible from the bootstrap node </li>
<li>Cluster nodes must have SSH enabled and ports open from the bootstrap node</li>
<li>The bootstrap node must have an unencrypted SSH key that can be used to authenticate with the cluster nodes over SSH</li>
</ul>

<p>[installing-enterprise-edition-hardware] [installing-enterprise-edition-software-ssh]</p>

<h1>Install DCOS</h1>

<p><strong>Important:</strong> Encrypted SSH keys are not supported.</p>

<ol>
<li><p>From your terminal, start the DCOS installer with this command.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --web
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
16:36:09 dcos_installer.action_lib.prettyprint:: ====&gt; Starting DCOS installer in web mode
16:36:09 root:: Starting server ('0.0.0.0', 9000)
</code></pre>

<p><strong>Tip:</strong> You can add the verbose (<code>-v</code>) flag to see the debug output:</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --web -v
</code></pre></li>
<li><p>Launch the DCOS web installer in your browser at: <code>http://&lt;bootstrap-node-public-ip&gt;:9000</code>.</p></li>
<li><p>Click <strong>Begin Installation</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin.png" rel="attachment wp-att-3190"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin-800x510.png" alt="ui-installer-begin" width="800" height="510" class="alignnone size-large wp-image-3190" /></a></p></li>
<li><p>Specify your Deployment and DCOS Environment settings:</p>

<h3>Deployment Settings</h3>

<dl>
<dt><strong>Master Private IP List</strong></dt>
<dd>Specify a comma-separated list of your internal static master IP addresses.</dd>

<dt><strong>Agent Private IP List</strong></dt>
<dd>Specify a comma-separated list of your internal static agent IP addresses.</dd>

<dt><strong>Master Public IP</strong></dt>
<dd>Specify a publicly accessible proxy IP address to one of your master nodes. If you don't have a proxy or already have access to the network where you are deploying this cluster, you can use one of the master IP's that you specified in the master list. This proxy IP address is used to access the DCOS web interface on the master node after DCOS is installed.</dd>

<dt><strong>SSH Username</strong></dt>
<dd>Specify the SSH username, for example <code>centos</code>. SSH Listening Port</dd>

<dt><strong>SSH Listening Port</strong></dt>
<dd>Specify the port to SSH to, for example <code>22</code>. SSH Key</dd>

<dt><strong>SSH Key</strong></dt>
<dd>Specify the private SSH key with access to your master IPs.</dd>
</dl>

<h3>DCOS Environment Settings</h3>

<dl>
<dt><strong>Username</strong></dt>
<dd>Specify the administrator username. This username is required for using DCOS.</dd>

<dt><strong>Password</strong></dt>
<dd>Specify the administrator password. This password is required for using DCOS.</dd>

<dt><strong>ZooKeeper for Exhibitor Private IP</strong></dt>
<dd>
<p>Specify a comma-separated list of one or more ZooKeeper host IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this ZooKeeper cluster to orchestrate its configuration.</p>

<p><strong>Important:</strong> Multiple ZooKeeper instances are recommended for failover in production environments.</p>
</dd>

<dt><strong>ZooKeeper for Exhibitor Port</strong></dt>
<dd>Specify the ZooKeeper port. For example, <code>2181</code>.</dd>

<dt><strong>Upstream DNS Servers</strong></dt>
<dd>
<p>Specify a comma-separated list of DNS resolvers for your DCOS cluster nodes. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them. If you have no internal hostnames to resolve, you can set this to a public nameserver like Google or AWS. In the example file above, the <a href="https://developers.google.com/speed/public-dns/docs/using" target="_blank">Google Public DNS IP addresses (IPv4)</a> are specified (<code>8.8.8.8</code> and <code>8.8.4.4</code>).</p>

<p><em>Caution:</em> If you set this parameter incorrectly you will have to reinstall DCOS. For more information about service discovery, see this <a href="../administration/service-discovery/">documentation</a>.</p>
</dd>

<dt><strong>IP Detect Script</strong></dt>
<dd>
<p>Choose an IP detect script from the dropdown to broadcast the IP address of each node across the cluster. Each node in a DCOS cluster has a unique IP address that is used to communicate between nodes in the cluster. The IP detect script prints the unique IPv4 address of a node to STDOUT each time DCOS is started on the node.</p>

<p><strong>Important:</strong> The IP address of a node must not change after DCOS is installed on the node. For example, the IP address must not change when a node is rebooted or if the DHCP lease is renewed. If the IP address of a node does change, the node must be wiped and reinstalled.</p>
</dd>
</dl></li>
<li><p>Click <strong>Run Pre-Flight</strong>. The preflight script installs the cluster <a href="../step-2-cluster-prerequisites/">prerequisites</a> and validates that your cluster is installable. This step can take up to 15 minutes to complete. If errors any errors are found, fix and then click <strong>Retry</strong>.</p>

<p><strong>Important:</strong> If you exit your GUI installation before launching DCOS, you must do this before reinstalling:</p>

<ul>
<li>SSH to each node in your cluster and run <code>rm -rf /opt/mesosphere</code>.</li>
<li>SSH to your bootstrap master node and run <code>rm -rf /var/lib/zookeeper</code></li>
</ul>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" rel="attachment wp-att-3197"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" alt="ui-installer-pre-flight1" width="626" height="405" class="alignnone size-full wp-image-3197" /></a></p></li>
<li><p>Click <strong>Deploy</strong> to install DCOS on your cluster. If errors any errors are found, fix and then click <strong>Retry</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" rel="attachment wp-att-3195"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" alt="ui-installer-deploy1" width="628" height="406" class="alignnone size-full wp-image-3195" /></a></p>

<p><strong>Tip:</strong> This step might take a few minutes, depending on the size of your cluster.</p></li>
<li><p>Click <strong>Run Post-Flight</strong>. If errors any errors are found, fix and then click <strong>Retry</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" rel="attachment wp-att-3196"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" alt="ui-installer-post-flight1" width="623" height="366" class="alignnone size-full wp-image-3196" /></a></p>

<p><strong>Tip:</strong> You can click <strong>Download Logs</strong> to view your logs locally.</p></li>
<li><p>Click <strong>Log In To DCOS</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a></p></li>
<li><p>Enter your administrator username and password.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a></p>

<p>You are done!</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a></p></li>
</ol>

<h3>Next Steps</h3>

<p>Now you can <a href="../security-and-authentication/managing-authorization/">assign user roles</a>.</p>