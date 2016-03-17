---
UID: 56df3deb60d0c
post_title: Automated command line installation
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>The automated command line installation method provides a guided installation of DCOS Enterprise Edition.</p>

<p>This installation method uses a bootstrap node to administer the DCOS installation across your cluster. The bootstrap node uses an SSH key to connect to each node in your cluster to automate the DCOS installation.</p>

<p>To use the automated command-line installation method:</p>

<ul>
<li>Cluster nodes must be network accessible from the bootstrap node </li>
<li>Cluster nodes must have SSH enabled and ports open from the bootstrap node</li>
<li>The bootstrap node must have an unencrypted SSH key that can be used to authenticate with the cluster nodes over SSH </li>
</ul>

<p>[installing-enterprise-edition-hardware] [installing-enterprise-edition-software-ssh] [installing-enterprise-edition-ip-detect]</p>

<h1>Step 2: Configure and Install DCOS</h1>

<p>In this step you create a customized YAML configuration file and install DCOS across your cluster using SSH.</p>

<p><strong>Prerequisite:</strong></p>

<ul>
<li>Encrypted SSH keys are not supported.</li>
</ul>

<h2><a name="config-json"></a>Configure your cluster</h2>

<p>In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.</p>

<ol>
<li><p>Run this command to create a hashed password for superuser authentication, where <code>&lt;superuser_password&gt;</code> is the superuser password. Use the hashed password key for the <code>superuser_password_hash</code> parameter in your <code>config.yaml</code> file.</p>

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

<pre><code>###########################################
# Use this bootstrap_url value unless you # 
# have moved the DCOS installer assets.   # 
###########################################
bootstrap_url: file:///opt/dcos_install_tmp
cluster_name: &lt;cluster-name&gt;
exhibitor_storage_backend: zookeeper
exhibitor_zk_hosts: &lt;host1&gt;:2181,&lt;host2&gt;:2181,&lt;host2&gt;:2181
exhibitor_zk_path: /dcos
master_discovery: static 
master_list:
- &lt;master-private-ip-1&gt;
- &lt;master-private-ip-2&gt;
- &lt;master-private-ip-3&gt;
resolvers:
- 8.8.8.8 
- 8.8.4.4
ssh_port: '22'
ssh_user: &lt;username&gt;
superuser_username: &lt;username&gt;
superuser_password_hash: &lt;hashed-password&gt;
agent_list:
- &lt;agent-private-ip-1&gt;
- &lt;agent-private-ip-2&gt;
- &lt;agent-private-ip-3&gt;
- &lt;agent-private-ip-4&gt;
- &lt;agent-private-ip-5&gt;
</code></pre>

<p>Specify these configuration parameters.</p>

<dl>
<dt><strong>bootstrap_url</strong></dt>
<dd>
<p>This parameter specifies the URI path for the DCOS installer to store the customized DCOS build files. Use the default value of <code>bootstrap_url: file:///opt/dcos_install_tmp</code>, unless you have moved the installer assets from their default location.</p>
</dd>

<dt><strong>cluster_name</strong></dt>
<dd>Specify the name of your cluster.</dd>

<dt><strong>exhibitor_storage_backend</strong></dt>
<dd>This parameter specifies the type of storage backend for Exhibitor. In the example file above, a ZooKeeper instance is used for external storage. During DCOS installation, a storage system is required for configuring and orchestrating Zookeeper with Exhibitor on the master nodes. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.</dd>

<dt><strong>exhibitor_zk_hosts</strong></dt>
<dd>Specify a comma-separated list of one or more ZooKeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this ZooKeeper cluster to orchestrate it's configuration. Multiple ZooKeeper instances are recommended for failover in production environments.</dd>

<dt><strong>master_discovery</strong></dt>
<dd>This parameter specifies the Mesos master discovery method. By default this is set to <code>static</code> in the <code>config.yaml</code> template file. The <code>static</code> method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address</dd>

<dt><strong>master_list</strong></dt>
<dd>Specify a list of your static master IP addresses as a YAML nested series (<code>-</code>).</dd>

<dt><strong>resolvers</strong></dt>
<dd>
<p>Specify a YAML-formatted list of DNS resolvers for your DCOS cluster nodes. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them. If you have no internal hostnames to resolve, you can set this to a public nameserver like Google or AWS. In the example file above, the <a href="https://developers.google.com/speed/public-dns/docs/using" target="_blank">Google Public DNS IP addresses (IPv4)</a> are specified (<code>8.8.8.8</code> and <code>8.8.4.4</code>).</p>

<p><em>Caution:</em> If you set the <code>resolvers</code> parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.</p>
</dd>

<dt><strong>ssh_port</strong></dt>
<dd>This parameter specifies the port to SSH to, for example <code>22</code>.</dd>

<dt><strong>ssh_user</strong></dt>
<dd>This parameter specifies the SSH username, for example <code>centos</code>.</dd>

<dt><strong>superuser_password_hash</strong></dt>
<dd>
<p>This parameter specifies the hashed superuser password. This password is required for using DCOS. The <code>superuser_password_hash</code> is generated by using the installer <code>--hash-password</code> flag. Here is an example of a hashed password value:</p>

<pre><code>superuser_password_hash: $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1 
</code></pre>
</dd>

<dt><strong>superuser_username</strong></dt>
<dd>This parameter specifies the Admin username. This username is required for using DCOS.</dd>

<dt><strong>agent_list</strong></dt>
<dd>This parameter specifies a complete list of IPv4 addresses to your agent host names. This must be a YAML-formatted nested series (-).</dd>
</dl>

<p>For more configuration examples and all available options, see the <a href="../configuration-parameters-1-6/">configuration file options</a>.</p></li>
<li><p>Copy your private SSH key to <code>genconf/ssh_key</code>. For more information, see the <a href="http://mesos.apache.org/documentation/latest/containerizer/">ssh_key_path</a> parameter.</p>

<pre><code>$ cp &lt;path-to-key&gt; genconf/ssh_key &amp;&amp; chmod 0600 genconf/ssh_key
</code></pre></li>
</ol>

<h2><a name="install-bash"></a>Install DCOS</h2>

<p>In this step you create a custom DCOS build file on your bootstrap node and then install DCOS across your cluster nodes with SSH. With this installation method you create a bootstrap server that uses your SSH key and connects to every node to automate the deployment.</p>

<p>You can view all of the automated command line installer options with the <code>--help</code> flag:</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --help
Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
usage: 
Install Mesosophere's Data Center Operating System

dcos_installer [-h] [-f LOG_FILE] [--hash-password HASH_PASSWORD] [-v]
[--web | --genconf | --preflight | --deploy | --postflight | --uninstall | --validate-config | --test]

Environment Settings:

  PORT                  Set the :port to run the web UI
  CHANNEL_NAME          ADVANCED - Set build channel name
  BOOTSTRAP_ID          ADVANCED - Set bootstrap ID for build

optional arguments:
  -h, --help            show this help message and exit
  --hash-password HASH_PASSWORD
                        Hash a password on the CLI for use in the config.yaml.
  -v, --verbose         Verbose log output (DEBUG).
  --offline             Do not install preflight prerequisites on CentOS7,
                        RHEL7 in web mode
  --web                 Run the web interface.
  --genconf             Execute the configuration generation (genconf).
  --preflight           Execute the preflight checks on a series of nodes.
  --install-prereqs     Install preflight prerequisites. Works only on CentOS7
                        and RHEL7.
  --deploy              Execute a deploy.
  --postflight          Execute postflight checks on a series of nodes.
  --uninstall           Execute uninstall on target hosts.
  --validate-config     Validate the configuration in config.yaml
  --test                Performs tests on the dcos_installer application
</code></pre>

<p><strong>Tip:</strong> If something goes wrong and you want to rerun your setup, use these cluster <a href="https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/" target="_blank">cleanup instructions</a>.</p>

<p>To install DCOS:</p>

<ol>
<li><p>From the <code>dcos</code> directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to <code>./genconf/serve/</code>.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --genconf
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>Extracking docker container from this script
dcos-genconf.4543c7745c7e-2af26a89fa52-cb932597d7b992.tar
Loading container into Docker daemon
...
</code></pre>

<p>At this point your directory structure should resemble:</p>

<pre><code>├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
├── dcos_generate_config.ee.sh
├── genconf
│   ├── config.yaml
│   ├── ip-detect     
</code></pre></li>
<li><p>Install the cluster prerequisites, including system updates, compression utilities (UnZip, GNU tar, and XZ Utils), and cluster permissions. For a full list of cluster prerequisites, see this <a href="../step-2-cluster-prerequisites/">documentation</a>.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --install-prereqs
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>20:47:49 dcos_installer.action_lib.prettyprint:: ====&gt; EXECUTING INSTALL PREREQUISITES
20:47:49 dcos_installer.action_lib.prettyprint:: ====&gt; START install_prereqs
20:52:32 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE install_prereqs
20:52:55 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE install_prereqs
20:52:55 dcos_installer.action_lib.prettyprint:: ====&gt; END install_prereqs with returncode: 0
20:52:55 dcos_installer.action_lib.prettyprint:: ====&gt; SUMMARY
20:52:55 dcos_installer.action_lib.prettyprint:: 2 out of 2 hosts successfully completed install_prereqs stage.
</code></pre></li>
<li><p>Run a preflight script to validate that your cluster is installable.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --preflight
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
20:54:02 dcos_installer.action_lib.prettyprint:: ====&gt; EXECUTING PREFLIGHT
20:54:02 dcos_installer.action_lib.prettyprint:: ====&gt; START run_preflight
20:54:03 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE preflight
20:54:03 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE preflight
20:54:03 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE preflight_cleanup
20:54:03 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE preflight_cleanup
20:54:03 dcos_installer.action_lib.prettyprint:: ====&gt; END run_preflight with returncode: 0
20:54:03 dcos_installer.action_lib.prettyprint:: ====&gt; SUMMARY
20:54:03 dcos_installer.action_lib.prettyprint:: 2 out of 2 hosts successfully completed run_preflight stage.
</code></pre>

<p><strong>Tip:</strong> For a detailed view, you can append log level debug (<code>-v</code>) to your command. For example <code>sudo bash dcos_generate_config.ee.sh --preflight -v</code>.</p></li>
<li><p>Install DCOS on your cluster.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --deploy
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
20:55:00 dcos_installer.action_lib.prettyprint:: ====&gt; EXECUTING DCOS INSTALLATION
20:55:00 dcos_installer.action_lib.prettyprint:: ====&gt; START deploy_master
20:57:04 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE deploy_master
20:57:04 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE deploy_master_cleanup
20:57:04 dcos_installer.action_lib.prettyprint:: ====&gt; END deploy_master with returncode: 0
20:57:04 dcos_installer.action_lib.prettyprint:: ====&gt; SUMMARY
20:57:04 dcos_installer.action_lib.prettyprint:: 1 out of 1 hosts successfully completed deploy_master stage.
20:57:04 dcos_installer.action_lib.prettyprint:: ====&gt; START deploy_agent
20:59:19 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE deploy_agent
20:59:19 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE deploy_agent_cleanup
20:59:19 dcos_installer.action_lib.prettyprint:: ====&gt; END deploy_agent with returncode: 0
20:59:19 dcos_installer.action_lib.prettyprint:: ====&gt; SUMMARY
20:59:19 dcos_installer.action_lib.prettyprint:: 1 out of 1 hosts successfully completed deploy_agent stage.
</code></pre></li>
<li><p>Run the DCOS diagnostic script to verify that services are up and running.</p>

<pre><code>$ sudo bash dcos_generate_config.ee.sh --postflight
</code></pre>

<p>Here is an example of the output.</p>

<pre><code>Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
21:22:44 dcos_installer.action_lib.prettyprint:: ====&gt; EXECUTING POSTFLIGHT
21:22:44 dcos_installer.action_lib.prettyprint:: ====&gt; START run_postflight
21:22:45 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE postflight
21:22:45 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE postflight
21:22:45 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE postflight_cleanup
21:22:45 dcos_installer.action_lib.prettyprint:: ====&gt; STAGE postflight_cleanup
21:22:45 dcos_installer.action_lib.prettyprint:: ====&gt; END run_postflight with returncode: 0
21:22:45 dcos_installer.action_lib.prettyprint:: ====&gt; SUMMARY
21:22:45 dcos_installer.action_lib.prettyprint:: 2 out of 2 hosts successfully completed run_postflight stage.
</code></pre></li>
<li><p>Monitor Exhibitor and wait for it to converge at <code>http://&lt;master-ip&gt;:8181/exhibitor/v1/ui/index.html</code>.</p>

<p><strong>Tip:</strong> This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a></p>

<p>When the status icons are green, you can access the DCOS web interface.</p></li>
<li><p>Launch the DCOS web interface at: <code>http://&lt;public-master-ip&gt;/</code>.</p></li>
<li><p>Click <strong>Log In To DCOS</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a></p></li>
<li><p>Enter your administrator username and password.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a></p>

<p>You are done!</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a></p></li>
</ol>

<h3>Next Steps</h3>

<p>Now you can <a href="../security-and-authentication/managing-authorization/">assign user roles</a>.</p>