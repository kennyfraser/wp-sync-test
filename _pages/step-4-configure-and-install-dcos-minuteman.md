---
UID: 56df3debc4269
post_title: 'Step 4: Configure and install DCOS (Minuteman)'
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
<p>Choose your DCOS installation method:</p>

<ul>
<li><a href="#ssh">Using the GUI installer to install DCOS</a></li>
<li><a href="#manual">Using SSH to distribute DCOS across your nodes</a></li>
<li><a href="../configuration-parameters-1-6/">Manually distributing DCOS across your nodes</a></li>
</ul>

<!-- [Step 3: Configure and install DCOS using SSH to distribute DCOS across your nodes][5] -->

<h1><a name="gui"></a>Using the GUI installer to install DCOS</h1>

<p><strong>Prerequisite:</strong></p>

<ul>
<li><a href="http://ipset.netfilter.org/">ipsets</a></li>
</ul>

<ol>
<li><p>Run this command on all of your master IPs:</p>

<pre><code>$ mkdir -p /etc/mesosphere/roles/; touch /etc/mesosphere/roles/minuteman
</code></pre></li>
<li><p>Download and save the DCOS setup file, <code>dcos_generate_config.ee.sh</code>, to the <code>dcos</code> directory on your bootstrap node. This file is used to create your customized DCOS build file.</p>

<p><strong>Important:</strong> Contact your sales representative or <a href="mailto:&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;">&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;</a> to obtain the DCOS setup file.</p></li>
<li><p>From your terminal, start the DCOS installer with this command. Contact your sales representative or sales@mesosphere.io to obtain 1. From your terminal, start the DCOS installer with this command.</p>

<p><strong>Important:</strong> Contact your sales representative or <a href="mailto:&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;">&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;</a> to obtain the DCOS setup file.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --web
Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
16:36:09 dcos_installer.action_lib.prettyprint:: ====&gt; Starting DCOS installer in web mode
16:36:09 root:: Starting server ('0.0.0.0', 9000)
</code></pre>

<p>You can add the verbose (<code>-v</code>) flag to see the debug output:</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --web -v
</code></pre></li>
<li><p>Launch the DCOS web installer in your browser at: <code>http://&lt;public-ip&gt;:9000</code>.</p></li>
<li><p>Click <strong>Begin Installation</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin.png" rel="attachment wp-att-3190"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin-800x510.png" alt="ui-installer-begin" width="800" height="510" class="alignnone size-large wp-image-3190" /></a></p>

<p><strong>Important:</strong> If you exit your GUI installation before launching DCOS, you must do this before reinstalling:</p>

<ul>
<li>SSH to each node in your cluster and run <code>rm -rf /opt/mesosphere</code>.</li>
<li>SSH to your bootstrap master node and run <code>rm -rf /var/lib/zookeeper</code></li>
</ul></li>
<li><p>Specify your Deployment and DCOS Environment settings:</p>

<h3>Deployment Settings</h3>

<dl>
<dt><strong>Master IP Address List</strong></dt>
<dd>Specify a comma-separated list of your static master IP addresses.</dd>

<dt><strong>Agent IP Address List</strong></dt>
<dd>Specify a comma-separated list of your static agent IP addresses.</dd>

<dt><strong>Accessible Master IP Address</strong></dt>
<dd>Specify the IP address to a publicly accessible proxy to one of your master nodes. If you either don't have a proxy or you already have access to the network where you are deploying this cluster, you can use one of the master IP's that you specified in the master list. This proxy IP address is used to access the DCOS web interface after the deployment succeeds.</dd>

<dt><strong>SSH Username</strong></dt>
<dd>Specify the SSH username, for example <code>centos</code>. SSH Listening Port</dd>

<dd>Specify the port to SSH to, for example <code>22</code>. SSH Key</dd>

<dd>Specify the SSH key with access to your master IPs.</dd>
</dl>

<h3>DCOS Environment Settings</h3>

<dl>
<dt><strong>Username</strong></dt>
<dd>Specify the administrator username. This username is required for using DCOS.</dd>

<dt><strong>Password</strong></dt>
<dd>Specify the administrator password. This password is required for using DCOS.</dd>

<dt><strong>Bootstrapping Zookeeper IP Address(es)</strong></dt>
<dd>Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it’s configuration.</dd>

<dt><strong>Bootstrapping Zookeeper Port</strong></dt>
<dd>Specify the Zookeeper port. For example, <code>2181</code>.</dd>

<dt><strong>Upstream DNS Servers</strong></dt>
<dd>Specify the DNS servers, which can be on your private network or the public internet. Caution: If you set this parameter incorrectly you will have to reinstall DCOS. For more information about service discovery, see this <a href="http://ipset.netfilter.org/">documentation</a>.</dd>

<dt><strong>IP Detect Script</strong></dt>
<dd>Specify an IP detect script to broadcast the IP address of each node across the cluster. For more information, see the <a href="http://mesos.apache.org/documentation/latest/containerizer/">documentation</a>.</dd>
</dl></li>
<li><p>Click <strong>Run Pre-Flight</strong>. The preflight script validates that your cluster is installable. This step can take up to 15 minutes to complete. If errors any errors are found, fix and then click <strong>Retry</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" rel="attachment wp-att-3197"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" alt="ui-installer-pre-flight1" width="626" height="405" class="alignnone size-full wp-image-3197" /></a></p></li>
<li><p>Click <strong>Deploy</strong> to install DCOS on your cluster. If errors any errors are found, fix and then click <strong>Retry</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" rel="attachment wp-att-3195"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" alt="ui-installer-deploy1" width="628" height="406" class="alignnone size-full wp-image-3195" /></a></p></li>
<li><p>Click <strong>Run Post-Flight</strong>. If errors any errors are found, fix and then click <strong>Retry</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" rel="attachment wp-att-3196"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" alt="ui-installer-post-flight1" width="623" height="366" class="alignnone size-full wp-image-3196" /></a></p>

<p><strong>Tip:</strong> You can click <strong>Download Logs</strong> to view your logs locally.</p></li>
<li><p>Click <strong>Log In To DCOS</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a></p></li>
<li><p>Enter your administrator username and password.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" rel="attachment wp-att-3182"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" alt="ui-installer-5" width="368" height="388" class="alignnone size-full wp-image-3182" /></a></p>

<p>You are done!</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" alt="dashboardsmall" width="1338" height="828" class="alignnone size-full wp-image-1120" /></a></p></li>
</ol>

<h3>Next Steps</h3>

<p>Now you can <a href="../security-and-authentication/managing-authorization/">assign user roles</a>.</p>

<h1><a name="ssh"></a>Using SSH to distribute DCOS across your nodes</h1>

<p><strong>Prerequisite:</strong></p>

<ul>
<li><a href="http://ipset.netfilter.org/">ipsets</a></li>
</ul>

<h3><a name="config-json"></a>4&#046;1 Configure your cluster</h3>

<p>In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.</p>

<ol>
<li><p>Customize this <code>config.yaml</code> template file for your environment. <!-- do not change bootstrap_url --></p>

<pre><code>  cluster_name: '&lt;cluster-name&gt;'
  ##########################################
  # DO NOT CHANGE the bootstrap_url value, # 
  # unless you have moved installer assets.# 
  ##########################################
  bootstrap_url: file:///opt/dcos_install_tmp
  cluster_name: &lt;cluster-name&gt;
  exhibitor_storage_backend: zookeeper
  exhibitor_zk_hosts: &lt;host1&gt;:&lt;port1&gt;
  exhibitor_zk_path: /dcos
  log_directory: /genconf/logs
  master_discovery: static 
  master_list:
  - &lt;master-ip-1&gt;
  - &lt;master-ip-2&gt;
  - &lt;master-ip-3&gt;
  resolvers:
  - &lt;dns-resolver-1&gt;
  - &lt;dns-resolver-2&gt;
  ssh_port: '&lt;port-number&gt;'
  ssh_user: &lt;username&gt;
  superuser_password: &lt;admin-username&gt;
  superuser_username: &lt;admin-password&gt;
  agent_list:
  - &lt;target-host-1&gt;
  - &lt;target-host-2&gt;
  - &lt;target-host-3&gt;
  - &lt;target-host-4&gt;
  - &lt;target-host-5&gt;
</code></pre>

<p>Specify these configuration parameters. <!-- log_directory: /genconf/logs, process_timeout: 120, ssh_key_path: /genconf/ssh-key --></p>

<dl>
<dt><strong>bootstrap_url</strong></dt>
<dd>
<p>This parameter specifies the URI path for the DCOS installer to store the customized DCOS build files. Use the default value of <code>bootstrap_url: file:///opt/dcos_install_tmp</code>, unless you have moved the installer assets from their default location.</p>
</dd>

<dt><strong>cluster_name</strong></dt>
<dd>Specify the name of your cluster.</dd>

<dt><strong>exhibitor_storage_backend</strong></dt>
<dd>This parameter specifies the type of storage backend for Exhibitor. By default this is set to <code>zookeeper</code> in the <code>config.yaml</code> template file. During DCOS installation, a storage system is required for configuring and orchestrating Zookeeper with Exhibitor on the master nodes. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.</dd>

<dt><strong>exhibitor_zk_hosts</strong></dt>
<dd>Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it's configuration.</dd>

<dt><strong>log_directory</strong></dt>
<dd>This parameter specifies the path to the installer host logs from the SSH processes. By default this is set to <code>/genconf/logs</code>. This should not be changed because <code>/genconf</code> is local to the container that is running the installer, and is a mounted volume.</dd>

<dt><strong>master_discovery</strong></dt>
<dd>This parameter specifies the Mesos master discovery method. By default this is set to <code>static</code> in the <code>config.yaml</code> template file. The <code>static</code> method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address</dd>

<dt><strong>master_list</strong></dt>
<dd>Specify a list of your static master IP addresses as a YAML nested series (<code>-</code>).</dd>

<dt><strong>resolvers</strong></dt>
<dd>
<p>Specify a JSON-formatted list of DNS servers for your DCOS host nodes. You must include the escape characters (<code></code>) as shown in the template. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them.</p>

<p><em>Caution:</em> If you set the <code>resolvers</code> parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.</p>
</dd>

<dt><strong>ssh_port</strong></dt>
<dd>This parameter specifies the port to SSH to, for example <code>22</code>.</dd>

<dt><strong>ssh_user</strong></dt>
<dd>This parameter specifies the SSH username, for example <code>centos</code>.</dd>

<dt><strong>superuser_password</strong></dt>
<dd>This parameter specifies the Admin password. This password is required for using DCOS.</dd>

<dt><strong>superuser_username</strong></dt>
<dd>This parameter specifies the Admin username. This username is required for using DCOS.</dd>

<dt><strong>agent_list</strong></dt>
<dd>This parameter specifies a complete list of IPv4 addresses to your agent host names. This must be a YAML-formatted nested series (-).</dd>
</dl>

<p>For more configuration examples and all available options, see the <a href="../configuration-parameters-1-6/">configuration file options</a>.</p></li>
<li><p>Save as <code>genconf/config.yaml</code>.</p></li>
<li><p>Move your private SSH key to <code>genconf/ssh_key</code>. For more information, see the <a href="http://mesos.apache.org/documentation/latest/containerizer/">ssh_key_path</a> parameter.</p>

<pre><code>$ cp &lt;path-to-key&gt; genconf/ssh_key &amp;&amp; chmod 0600 genconf/ssh_key
</code></pre></li>
</ol>

<h3><a name="install-bash"></a>4&#046;2 Install DCOS</h3>

<p>In this step you create a custom DCOS build file on your bootstrap node and then install DCOS onto your cluster. With this installation method you create a bootstrap server that uses your SSH key and connects to every node to automate the deployment.</p>

<p><strong>Tip:</strong> If something goes wrong and you want to rerun your setup, use these cluster <a href="../getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/">cleanup instructions</a>.</p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>A <code>genconf/config.yaml</code> file that is optimized for automatic distribution of DCOS across your nodes with SSH.</li>
<li>A <code>genconf/ip-detect</code> <a href="https://docs.mesosphere.com/installing-enterprise-edition-1-6/step-3-ip-address-discovery-script/">script</a>.</li>
</ul>

<!-- Early access URL: https://downloads.mesosphere.com/dcos/EarlyAccess/dcos_generate_config.sh -->

<!-- Stable URL: https://downloads.mesosphere.com/dcos/stable/dcos_generate_config.sh -->

<p>To install DCOS:</p>

<ol>
<li><p>Download and save the DCOS setup file, <code>dcos_generate_config.sh</code>, to the <code>dcos</code> directory on your bootstrap node. This file is used to create your customized DCOS build file.</p>

<p><strong>Important:</strong> Contact your sales representative or <a href="mailto:&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;">&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;</a> to obtain the DCOS setup file.</p></li>
<li><p>From the <code>dcos</code> directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to <code>./genconf/serve/</code>.</p>

<pre><code>$ sudo bash dcos_generate_config.sh --genconf
Extracking docker container from this script
dcos-genconf.4543c7745c7e-2af26a89fa52-cb932597d7b992.tar
Loading container into Docker daemon
...
</code></pre>

<p>At this point your directory structure should resemble:</p>

<pre><code>├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
├── dcos_generate_config.sh
├── genconf
│   ├── config.yaml
│   ├── ip-detect
</code></pre></li>
<li><p>Run this command on all of your master IPs:</p>

<pre><code>$ mkdir -p /etc/mesosphere/roles/; touch /etc/mesosphere/roles/minuteman
</code></pre></li>
<li><p>Run a preflight script to validate that your cluster is installable.</p>

<pre><code>$ sudo bash dcos_generate_config.sh --preflight 
Running mesosphere/dcos-genconf docker BUILD_DIR set to /home/someuser/genconf 
... 
Running preflight checks
</code></pre></li>
<li><p>Install DCOS on your cluster.</p>

<pre><code>$ sudo bash dcos_generate_config.sh --deploy
Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/someuser/genconf
...
Starting DCOS install process
Running preflight checks
Creating directories under /etc/mesosphere
Creating role file for master
Configuring DCOS
Setting and starting DCOS

Cleaning up temp directory /opt/dcos_install_tmp
</code></pre>

<p><strong>Tip:</strong> For a detailed view, you can add log level debug (<code>-v</code>) to your command. For example <code>sudo bash dcos_generate_config.sh --deploy -v</code>.</p></li>
<li><p>Run the DCOS diagnostic script to verify that services are up and running.</p>

<pre><code>$ sudo bash dcos_generate_config.sh --postflight
</code></pre></li>
<li><p>Monitor Exhibitor and wait for it to converge at <code>http://&lt;master-ip&gt;:8181/exhibitor/v1/ui/index.html</code>.</p>

<p><strong>Tip:</strong> This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a></p>

<p>When the status icons are green, you can access the DCOS web interface.</p></li>
<li><p>Launch the DCOS web interface at: <code>http://&lt;public-master-ip&gt;/</code>.</p></li>
<li><p>Click <strong>Log In To DCOS</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a></p></li>
<li><p>Enter your administrator username and password.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" rel="attachment wp-att-3182"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" alt="ui-installer-5" width="368" height="388" class="alignnone size-full wp-image-3182" /></a></p>

<p>You are done!</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" alt="dashboardsmall" width="1338" height="828" class="alignnone size-full wp-image-1120" /></a></p></li>
</ol>

<h3>Next Steps</h3>

<p>Now you can <a href="../security-and-authentication/managing-authorization/">assign user roles</a>.</p>

<h1><a name="manual"></a>Manually distributing DCOS across your nodes</h1>

<p><strong>Prerequisite:</strong></p>

<ul>
<li><a href="http://ipset.netfilter.org/">ipsets</a></li>
</ul>

<h3><a name="config-json"></a>4&#046;1 Configure your cluster</h3>

<p>In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.</p>

<ol>
<li><p>Customize this <code>config.yaml</code> template file for your environment. <!-- bootstrap_url is changeable --></p>

<pre><code>  bootstrap_url: http://&lt;bootstrap_ip&gt;:&lt;your_port&gt;       
  cluster_name: '&lt;cluster-name&gt;'
  exhibitor_storage_backend: zookeeper
  exhibitor_zk_hosts: &lt;host1&gt;:&lt;port1&gt;
  exhibitor_zk_path: /dcos
  master_discovery: static 
  master_list:
  - &lt;master-ip-1&gt;
  - &lt;master-ip-2&gt;
  - &lt;master-ip-3&gt;
  resolvers:
  - &lt;dns-resolver-1&gt;
  - &lt;dns-resolver-2&gt;
  superuser_password: &lt;admin-username&gt;
  superuser_username: &lt;admin-password&gt;
</code></pre>

<p>Specify these configuration parameters. <!-- log_directory: /genconf/logs, process_timeout: 120, ssh_key_path: /genconf/ssh-key --></p>

<dl>
<dt><strong>cluster_name</strong></dt>
<dd>Specify the name of your cluster.</dd>

<dt><strong>bootstrap_url</strong></dt>
<dd>
<p>This parameter specifies the URI path for the DCOS installer to store the customized DCOS build files, which can be local (<code>bootstrap_url:file:///opt/dcos_install_tmp</code>) or hosted (<code>http://&lt;your-web-server&gt;</code>). By default this is set to <code>file:///opt/dcos_install_tmp</code> in the <code>config.yaml</code> template file, which is the location where the DCOS installer puts your install tarball.</p>

<p><strong>Tip:</strong> This parameter is for advanced users. The default value should work for most installations.</p>
</dd>

<dt><strong>exhibitor_storage_backend</strong></dt>
<dd>This parameter specifies the type of storage backend for Exhibitor. By default this is set to <code>zookeeper</code> in the <code>config.yaml</code> template file. During DCOS installation, a storage system is required for configuring and orchestrating Zookeeper with Exhibitor on the master nodes. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.</dd>

<dt><strong>exhibitor_zk_hosts</strong></dt>
<dd>Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it's configuration.</dd>

<dt><strong>master_discovery</strong></dt>
<dd>This parameter specifies the Mesos master discovery method. By default this is set to <code>static</code> in the <code>config.yaml</code> template file. The <code>static</code> method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address</dd>

<dt><strong>master_list</strong></dt>
<dd>Specify a list of your static master IP addresses as a YAML nested series (<code>-</code>).</dd>

<dt><strong>resolvers</strong></dt>
<dd>
<p>Specify a JSON-formatted list of DNS servers for your DCOS host nodes. You must include the escape characters (<code></code>) as shown in the template. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them.</p>

<p><em>Caution:</em> If you set the <code>resolvers</code> parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.</p>
</dd>

<dt><strong>superuser_password</strong></dt>
<dd>This parameter specifies the Admin password. This password is required for using DCOS.</dd>

<dt><strong>superuser_username</strong></dt>
<dd>This parameter specifies the Admin username. This username is required for using DCOS.</dd>
</dl>

<p>For more configuration examples and all available options, see the <a href="../configuration-parameters/">configuration file options</a>.</p></li>
<li><p>Save as <code>genconf/config.yaml</code>.</p></li>
</ol>

<h2><a name="install-bash"></a>4&#046;2 Install DCOS</h2>

<p>In this step you create a custom DCOS build file on your bootstrap node and then install DCOS onto your cluster. With this method you package the DCOS distribution yourself and connect to every server manually and run the commands.</p>

<p><strong>Tip:</strong> If something goes wrong and you want to rerun your setup, use these cluster <a href="http://mesos.apache.org/documentation/latest/containerizer/">cleanup instructions</a>.</p>

<p><strong>Prerequisites</strong></p>

<ul>
<li>A <code>genconf/config.yaml</code> file that is optimized for manual distribution of DCOS across your nodes.</li>
<li>A <code>genconf/ip-detect</code> <a href="https://docs.mesosphere.com/installing-enterprise-edition-1-6/step-3-ip-address-discovery-script/">script</a>. </li>
<li><p>The Docker nginx image must be on your bootstrap node. You can use this command to install the nginx container:</p>

<pre><code> docker pull nginx
</code></pre>

<p>For more information, see <a href="https://hub.docker.com/_/nginx/" target="_blank">DockerHub nginx</a> documentation.</p></li>
</ul>

<!-- Early access URL: https://downloads.mesosphere.com/dcos/EarlyAccess/dcos_generate_config.sh -->

<p><!-- Stable URL: https://downloads.mesosphere.com/dcos/stable/dcos_generate_config.sh --> To install DCOS:</p>

<ol>
<li><p>Download and save the DCOS setup file, <code>dcos_generate_config.sh</code>, to the <code>dcos</code> directory on your bootstrap node. This file is used to create your customized DCOS build file.</p>

<p><strong>Important:</strong> Contact your sales representative or <a href="mailto:&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;">&#x73;&#097;&#108;&#101;&#115;&#064;&#109;&#101;&#115;&#111;&#115;p&#x68;&#x65;&#x72;&#x65;&#x2e;&#x69;&#x6f;</a> to obtain the DCOS setup file.</p></li>
<li><p>From the <code>dcos</code> directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to <code>./genconf/serve/</code>.</p>

<p>At this point your directory structure should resemble:</p>

<pre><code>├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
├── dcos_generate_config.sh
├── genconf
│   ├── config.yaml
│   ├── ip-detect
</code></pre></li>
<li><p>Run this command to generate your customized DCOS build file:</p>

<pre><code>$ sudo bash dcos_generate_config.sh
</code></pre>

<p><strong>Tip:</strong> For the install script to work, you must have created <code>genconf/config.yaml</code> and <a href="https://docs.mesosphere.com/installing-enterprise-edition-1-6/step-3-ip-address-discovery-script/">genconf/ip-detect</a>.</p></li>
<li><p>From the <code>dcos</code> directory, run this command to host the DCOS install package through an nginx Docker container. For <code>&lt;your-port&gt;</code>, specify the port value that is used in the <code>bootstrap_url</code>.</p>

<pre><code>$ docker run -d -p &lt;your-port&gt;:80 -v $PWD/genconf/serve:/usr/share/nginx/html:ro nginx
</code></pre></li>
<li><p>Run these commands on each of your master nodes in succession to install DCOS using your custom build file.</p>

<p><strong>Tip:</strong> Although there is no actual harm to your cluster, DCOS may issue error messages until all of your master nodes are configured.</p></li>
<li><p>SSH to your node:</p>

<pre><code>$ ssh &lt;master-ip&gt;
</code></pre></li>
<li><p>Make a new directory and navigate to it:</p>

<pre><code>$ mkdir /tmp/dcos &amp;&amp; cd /tmp/dcos
</code></pre></li>
<li><p>Download the DCOS installer from the nginx Docker container, where <code>&lt;bootstrap-ip&gt;</code> and <code>&lt;your_port&gt;</code> are specified in <code>bootstrap_url</code>:</p>

<pre><code>$ curl -O http://&lt;bootstrap-ip&gt;:&lt;your_port&gt;/dcos_install.sh
</code></pre></li>
<li><p>Run this command to install DCOS on your master node:</p>

<pre><code>$ sudo bash dcos_install.sh master,minuteman
</code></pre></li>
<li><p>Run these commands on each of your agent nodes to install DCOS using your custom build file.</p></li>
<li><p>SSH to your node:</p>

<pre><code>$ ssh &lt;master-ip&gt;
</code></pre></li>
<li><p>Make a new directory and navigate to it:</p>

<pre><code>$ mkdir /tmp/dcos &amp;&amp; cd /tmp/dcos
</code></pre></li>
<li><p>Download the DCOS installer from the nginx Docker container, where <code>&lt;bootstrap-ip&gt;</code> and <code>&lt;your_port&gt;</code> are specified in <code>bootstrap_url</code>:</p>

<pre><code>$ curl http://&lt;bootstrap-ip&gt;:&lt;your_port&gt;/dcos_install.sh
</code></pre></li>
<li><p>Run this command to install DCOS on your agent node:</p>

<pre><code>$ sudo bash dcos_install.sh slave,minuteman
</code></pre></li>
<li><p>Monitor Exhibitor and wait for it to converge at <code>http://&lt;master-ip&gt;:8181/exhibitor/v1/ui/index.html</code>.</p>

<p><strong>Tip:</strong> This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a></p>

<p>When the status icons are green, you can access the DCOS web interface.</p></li>
<li><p>Launch the DCOS web interface at: <code>http://&lt;load-balanced-ip&gt;/</code>.</p></li>
<li><p>Click <strong>Log In To DCOS</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a></p></li>
<li><p>Enter your administrator username and password.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" rel="attachment wp-att-3182"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" alt="ui-installer-5" width="368" height="388" class="alignnone size-full wp-image-3182" /></a></p>

<p>You are done!</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" alt="dashboardsmall" width="1338" height="828" class="alignnone size-full wp-image-1120" /></a></p></li>
</ol>

<h3>Next Steps</h3>

<p>Now you can <a href="../security-and-authentication/managing-authorization/">assign user roles</a>.</p>