---
UID: 56df3debf2e74
post_title: 'Step 2: Configure and install DCOS'
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: true
---
<p>The DCOS installation creates these folders:</p>

<ul>
<li><dl>
<dt><code>/opt/mesosphere</code></dt>
<dd>Contains all the DCOS binaries, libraries, cluster configuration. Do not modify.</dd>
</dl></li>
<li><dl>
<dt><code>/etc/systemd/system/dcos.target.wants</code></dt>
<dd>Contains the systemd services which start the things that make up systemd. They must live outside of <code>/opt/mesosphere</code> because of systemd constraints.</dd>
</dl></li>
<li><dl>
<dt>Various units prefixed with <code>dcos</code> in <code>/etc/systemd/system</code></dt>
<dd>Copies of the units in <code>/etc/systemd/system/dcos.target.wants</code>. They must be at the top folder as well as inside <code>dcos.target.wants</code>.</dd>
</dl></li>
</ul>

<h1><a name="config-json"></a>Configure your cluster</h1>

<p>In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.</p>

<ol>
<li><p>From the bootstrap node, run this command to create a hashed password for superuser authentication, where <code>&lt;superuser_password&gt;</code> is the superuser password. Use the hashed password key for the <code>superuser_password_hash</code> parameter in your <code>config.yaml</code> file.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --hash-password &lt;superuser_password&gt;
</code></pre>

<p>Here is an example of a hashed password output.</p>

<pre><code>Extracting image from this script and loading into docker daemon, this step can take a few minutes
dcos-genconf.9eda4ae45de5488c0c-c40556fa73a00235f1.tar
Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
00:42:10 dcos_installer.action_lib.prettyprint:: ====&gt; HASHING PASSWORD TO SHA512
00:42:11 root:: Hashed password for 'password' key:
$6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1
</code></pre></li>
<li><p>Create a configuration file and save as <code>genconf/config.yaml</code>. You can use this example as a template.</p>

<pre><code>---
bootstrap_url: http://&lt;bootstrap_public_ip&gt;:&lt;your_port&gt;       
cluster_name: '&lt;cluster-name&gt;'
exhibitor_storage_backend: zookeeper
exhibitor_zk_hosts: &lt;host1&gt;:2181,&lt;host2&gt;:2181,&lt;host3&gt;:2181
exhibitor_zk_path: /dcos
master_discovery: static 
master_list:
- &lt;master-private-ip-1&gt;
- &lt;master-private-ip-2&gt;
- &lt;master-private-ip-3&gt;
superuser_username: &lt;username&gt;
superuser_password_hash: &lt;hashed-password&gt;
resolvers:
- 8.8.8.8
- 8.8.4.4
</code></pre>

<dl>
<dt><strong>bootstrap_url</strong></dt>
<dd>Specify the URI path for the DCOS installer to store the customized DCOS build files. For example, <code>http://&lt;bootstrap_ip&gt;:&lt;your_port&gt;</code>. By default this is set to <code>file:///opt/dcos_install_tmp</code> in the <code>config.yaml</code> template file, but for manual installations you must change this value.</dd>

<dt><strong>cluster_name</strong></dt>
<dd>Specify the name of your cluster.</dd>

<dt><strong>exhibitor_storage_backend</strong></dt>
<dd>This parameter specifies the type of storage backend for Exhibitor. In the example file above, a ZooKeeper instance is used for external storage. During DCOS installation, a storage system is required for configuring and orchestrating ZooKeeper with Exhibitor on the master nodes. Exhibitor automatically configures your ZooKeeper installation on the master nodes during your DCOS installation. Multiple ZooKeeper instances are recommended for failover in production environments.</dd>

<dt><strong>exhibitor_zk_hosts</strong></dt>
<dd>Specify a comma-separated list of one or more ZooKeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this ZooKeeper cluster to orchestrate it's configuration. Where <code>&lt;host1&gt;</code> is your internal bootstrap node IP and port <code>2181</code> is the default ZooKeeper port.</dd>

<dt><strong>master_discovery</strong></dt>
<dd>This parameter specifies the Mesos master discovery method. By default this is set to <code>static</code> in the <code>config.yaml</code> template file. The <code>static</code> method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address</dd>

<dt><strong>master_list</strong></dt>
<dd>Specify a list of your static master IP addresses as a YAML nested series (<code>-</code>).</dd>

<dt><strong>resolvers</strong></dt>
<dd>
<p>Specify a YAML-formatted nested series (<code>-</code>) of DNS resolvers for your DCOS host nodes, or accept the default <a href="https://developers.google.com/speed/public-dns/docs/using" target="_blank">Google Public DNS IP addresses (IPv4)</a> of <code>8.8.8.8</code> and <code>8.8.4.4</code>. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them. If you have no internal hostnames to resolve, it</p>

<p><em>Caution:</em> If you set the <code>resolvers</code> parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.</p>
</dd>

<dt><strong>superuser_password_hash</strong></dt>
<dd>This parameter specifies the hashed Admin password. This password is required for using DCOS. See step 1 for more information on how to create.</dd>

<dt><strong>superuser_username</strong></dt>
<dd>Specify a new username for the <code>Bootstrap superuser</code>. This username is required for using DCOS. For more information, see <a href="../security-and-authentication/managing-authorization/" target="_blank">Managing Authentication</a>.</dd>
</dl>

<p>For the complete list of available configuration options and parameters, see the <a href="../configuration-parameters/">Configuration Parameters</a> documentation.</p></li>
</ol>

<h1><a name="install-bash"></a>Install DCOS</h1>

<p>In this step you create a custom DCOS build file on your bootstrap node and then install DCOS onto your cluster. With this method you package the DCOS distribution yourself and connect to every server manually and run the commands.</p>

<p><strong>Tip:</strong> If something goes wrong and you want to rerun your setup, use these cluster <a href="https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/">cleanup instructions</a>.</p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>A <code>genconf/config.yaml</code> file that is optimized for <a href="../administration/introcli/">manual distribution of DCOS across your nodes</a>.</li>
<li>A <code>genconf/ip-detect</code> <a href="../step-3-ip-address-discovery-script/">script</a>.</li>
</ul>

<!-- Early access URL: https://downloads.mesosphere.com/dcos/EarlyAccess/dcos_generate_config.sh -->

<p><!-- Stable URL: https://downloads.mesosphere.com/dcos/stable/dcos_generate_config.sh --> To install DCOS:</p>

<ol>
<li><p>From the bootstrap node, run the DCOS installer shell script to generate a customized DCOS build file. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to <code>./genconf/serve/</code>.</p>

<p>At this point your directory structure should resemble:</p>

<pre><code>├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
├── dcos_generate_config.ee.sh
├── genconf
│   ├── config.yaml
│   ├── ip-detect
</code></pre></li>
<li><p>Run this command to generate your customized DCOS build file:</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh
</code></pre>

<p><strong>Tip:</strong> For the install script to work, you must have created <a href="../administration/introcli/">genconf/config.yaml</a> and <a href="../step-3-ip-address-discovery-script/">genconf/ip-detect</a>.</p></li>
<li><p>From your home directory, run this command to host the DCOS install package through an nginx Docker container. For <code>&lt;your-port&gt;</code>, specify the port value that is used in the <code>bootstrap_url</code>.</p>

<pre><code>$ sudo docker run -d -p &lt;your-port&gt;:80 -v $PWD/genconf/serve:/usr/share/nginx/html:ro nginx
</code></pre></li>
<li><p>Run these commands on each of your master nodes in succession to install DCOS using your custom build file.</p>

<p><strong>Tip:</strong> Although there is no actual harm to your cluster, DCOS may issue error messages until all of your master nodes are configured.</p>

<ol>
<li><p>SSH to your master nodes:</p>

<pre><code>$ ssh &lt;master-ip&gt;
</code></pre></li>
<li><p>Make a new directory and navigate to it:</p>

<pre><code>$ mkdir /tmp/dcos &amp;&amp; cd /tmp/dcos
</code></pre></li>
<li><p>Download the DCOS installer from the nginx Docker container, where <code>&lt;bootstrap-ip&gt;</code> and <code>&lt;your_port&gt;</code> are specified in <code>bootstrap_url</code>:</p>

<pre><code>$ curl -O http://&lt;bootstrap-ip&gt;:&lt;your_port&gt;/dcos_install.sh
</code></pre></li>
<li><p>Run this command to install DCOS on your master nodes:</p>

<pre><code>$ sudo bash dcos_install.sh master
</code></pre></li>
</ol></li>
<li><p>Run these commands on each of your agent nodes to install DCOS using your custom build file.</p>

<ol>
<li><p>SSH to your agent nodes:</p>

<pre><code>$ ssh &lt;agent-ip&gt;
</code></pre></li>
<li><p>Make a new directory and navigate to it:</p>

<pre><code>$ mkdir /tmp/dcos &amp;&amp; cd /tmp/dcos
</code></pre></li>
<li><p>Download the DCOS installer from the nginx Docker container, where <code>&lt;bootstrap-ip&gt;</code> and <code>&lt;your_port&gt;</code> are specified in <code>bootstrap_url</code>:</p>

<pre><code>$ curl -O http://&lt;bootstrap-ip&gt;:&lt;your_port&gt;/dcos_install.sh
</code></pre></li>
<li><p>Run this command to install DCOS on your agent nodes:</p>

<pre><code>$ sudo bash dcos_install.sh slave
</code></pre></li>
</ol></li>
<li><p>Monitor Exhibitor and wait for it to converge at <code>http://&lt;master-ip&gt;:8181/exhibitor/v1/ui/index.html</code>.</p>

<p><strong>Tip:</strong> This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a></p>

<p>When the status icons are green, you can access the DCOS web interface.</p></li>
<li><p>Launch the DCOS web interface at: <code>http://&lt;master-node-public-ip&gt;/</code>.</p></li>
<li><p>Click <strong>Log In To DCOS</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a></p></li>
<li><p>Enter your administrator username and password.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a></p>

<p>You are done!</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a></p></li>
</ol>

<h3>Next Steps</h3>

<p>Now you can <a href="../security-and-authentication/">assign user roles</a>.</p>