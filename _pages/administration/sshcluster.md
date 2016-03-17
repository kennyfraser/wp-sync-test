---
UID: 56df3deeda9f6
post_title: SSHing to a DCOS Cluster
post_excerpt: ""
layout: page
published: true
menu_order: 14
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>These instructions explain how to set up an SSH connection to your DCOS cluster from an outside network. If you are on the same network as your cluster or connected by using VPN, you can instead use the <code>dcos node ssh</code> command. For more information, see the <a href="../introcli/command-reference/">dcos node section</a> of the CLI reference.</p>

<ul>
<li><a href="#unix">SSH to your DCOS cluster on Unix/Linux (OS X, Ubuntu, etc)</a></li>
<li><a href="#windows">SSH to your DCOS cluster on Windows</a></li>
</ul>

<p><strong>Requirements:</strong></p>

<ul>
<li>During <a href="/install/awscluster/#create">AWS DCOS cluster creation</a>, a <code>.pem</code> file is created. You will need this <code>.pem</code> file to SSH to your cluster.</li>
</ul>

<h3><a name="unix"></a>SSH to your DCOS cluster on Unix/Linux (OS X, Ubuntu, etc)</h3>

<ol>
<li><p>Change the permissions on the <code>.pem</code> file to owner read/write by using the <code>chmod</code> command.</p>

<p><strong>Important:</strong> Your <code>.pem</code> file must be located in the <code>~/.ssh</code> directory.</p>

<pre><code>chmod 600 &lt;private-key&gt;.pem
</code></pre></li>
<li><p>SSH into the cluster.</p>

<ol>
<li><p>From your terminal, add your new configuration to the <code>.pem</code> file, where <code>&lt;private-key&gt;</code> is your <code>.pem</code> file.</p>

<pre><code>$ ssh-add ~/.ssh/&lt;private-key&gt;.pem

Identity added: /Users/&lt;yourdir&gt;/.ssh/&lt;private-key&gt;.pem (/Users/&lt;yourdir&gt;/.ssh/&lt;private-key&gt;.pem)
</code></pre></li>
</ol>

<ul>
<li><p><strong>To SSH to a master node:</strong></p>

<ol>
<li><p>From the DCOS CLI, enter the following command:</p>

<pre><code>$ dcos node ssh --master-proxy --master
</code></pre></li>
</ol></li>
<li><p><strong>To SSH to an agent node:</strong></p>

<ol>
<li><p>From the Mesos web interface, copy the agent node ID. You can find the IDs on the <strong>Frameworks</strong> (<code>&lt;master-node-IPaddress&gt;/mesos/#/frameworks</code>) or <strong>Slaves</strong> page (<code>&lt;master-node-IPaddress&gt;/mesos/#/slaves</code>).</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/mesos-sandbox-slave-copy.png" rel="attachment wp-att-1560"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/mesos-sandbox-slave-copy-600x119.png" alt="mesos-sandbox-slave-copy" width="500" height="260" class="alignnone size-medium wp-image-1560" /></a></p></li>
<li><p>From the DCOS CLI, enter the following command, where <code>&lt;slave-id&gt;</code> is your agent ID:</p>

<pre><code>$ dcos node ssh --master-proxy --slave=&lt;slave-id&gt;
</code></pre></li>
</ol></li>
</ul></li>
</ol>

<h3><a name="windows"></a>SSH to your DCOS cluster on Windows</h3>

<p><strong>Requirements:</strong></p>

<ul>
<li>PuTTY SSH client or equivalent (These instructions assume you are using PuTTY, but almost any SSH client will work.)</li>
<li>PuTTYgen RSA and DSA key generation utility</li>
<li>Pageant SSH authentication agent </li>
</ul>

<p>To install these programs, download the Windows installer <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">from the official PuTTY download page.</a></p>

<ol>
<li><p>Convert the <code>.pem</code> file type to <code>.ppk</code> by using PuTTYgen:</p>

<ol>
<li><p>Open PuTTYgen, select <strong>File &gt; Load Private Key</strong>, and choose your <code>.pem</code> file.</p></li>
<li><p>Select <strong>SSH-2 RSA</strong> as the key type, click <strong>Save private key</strong>, then choose the name and location to save your new .ppk key.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsputtykey.png" rel="attachment wp-att-1611"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsputtykey-600x596.png" alt="windowsputtykey" width="500" height="498" class="alignnone size-medium wp-image-1611" /></a></p></li>
<li><p>Close PuTTYgen.</p></li>
</ol></li>
<li><p>SSH into the cluster.</p>

<ul>
<li><p><strong>To SSH to a master node:</strong></p>

<ol>
<li><p>From the DCOS web interface, copy the IP address of the master node. The IP address is displayed beneath your cluster name.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/nodehostname.png" rel="attachment wp-att-1567"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/nodehostname-600x231.png" alt="nodehostname" width="300" height="116" class="alignnone size-medium wp-image-1567" /></a></p></li>
<li><p>Open PuTTY and enter the master node IP address in the <strong>Host Name (or IP address)</strong> field.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsputtybasic.png" rel="attachment wp-att-1610"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsputtybasic-600x592.png" alt="windowsputtybasic" width="500" height="496" class="alignnone size-medium wp-image-1610" /></a></p></li>
<li><p>In the <strong>Category</strong> pane on the left side of the PuTTY window, choose <strong>Connection &gt; SSH &gt; Auth</strong>, click <strong>Browse</strong>, locate and select your <code>.ppk</code> file, then click <strong>Open</strong>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsputtysshopt.png" rel="attachment wp-att-1612"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsputtysshopt-600x592.png" alt="windowsputtysshopt" width="500" height="496" class="alignnone size-medium wp-image-1612" /></a></p></li>
<li><p>Login as user “core”.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowscore.png" rel="attachment wp-att-1607"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowscore-600x349.png" alt="windowscore" width="500" height="375" class="alignnone size-medium wp-image-1607" /></a></p></li>
</ol></li>
<li><p><strong>To SSH to an agent node</strong></p>

<p><strong>Prerequisite:</strong> You must be logged out of your master node.</p>

<ol>
<li><p>Enable agent forwarding in PuTTY.</p>

<p><strong>Caution:</strong> SSH agent forwarding has security implications. Only add servers that you trust and that you intend to use with agent forwarding. For more information on agent forwarding, see <a href="https://developer.github.com/guides/using-ssh-agent-forwarding/" target="_blank">Using SSH agent forwarding.</a></p>

<ol>
<li><p>Open PuTTY. In the <strong>Category</strong> pane on the left side of the PuTTY window, choose <strong>Connection &gt; SSH &gt; Auth</strong> and check the <strong>Allow agent forwarding</strong> box.</p></li>
<li><p>Click the <strong>Browse</strong> button and locate the <code>.ppk</code> file that you created previously using PuTTYgen.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsforwarding.png" rel="attachment wp-att-1608"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowsforwarding-600x592.png" alt="windowsforwarding" width="500" height="496" class="alignnone size-medium wp-image-1608" /></a></p></li>
</ol></li>
<li><p>Add the <code>.ppk</code> file to Pageant.</p>

<ol>
<li><p>Open Pageant. If the Pageant window does not appear, look for the Pageant icon in the notification area in the lower right area of the screen next to the clock and double-click it to open Pageant's main window.</p></li>
<li><p>Click the <strong>Add Key</strong> button.</p></li>
<li><p>Locate the <code>.ppk</code> file that you created using PuTTYgen and click <strong>Open</strong> to add your key to Pageant.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowspageant.png" rel="attachment wp-att-1609"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowspageant-600x433.png" alt="windowspageant" width="500" height="417" class="alignnone size-medium wp-image-1609" /></a></p></li>
<li><p>Click the <strong>Close</strong> button to close the Pageant window.</p></li>
</ol></li>
<li><p>SSH into the master node.</p>

<ol>
<li><p>From the DCOS web interface, copy the IP address of the master node. The IP address is displayed beneath your cluster name.</p></li>
<li><p>In the <strong>Category</strong> pane on the left side of the PuTTY window, choose <strong>Session</strong> and enter the master node IP address in the <strong>Host Name (or IP address)</strong> field.</p></li>
<li><p>Login as user “core”.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowscore.png" rel="attachment wp-att-1607"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/windowscore-600x349.png" alt="windowscore" width="500" height="375" class="alignnone size-medium wp-image-1607" /></a></p></li>
</ol></li>
<li><p>From the master node, SSH into the agent node.</p>

<ol>
<li><p>From the Mesos web interface, copy the agent node hostname. You can find hostnames on the <strong>Frameworks</strong> (<code>&lt;master-node-IPaddress&gt;/mesos/#/frameworks</code>) or <strong>Slaves</strong> page (<code>&lt;master-node-IPaddress&gt;/mesos/#/slaves</code>).</p></li>
<li><p>SSH into the agent node as the user <code>core</code> with the agent node hostname specified:</p>

<pre><code>ssh core@&lt;agent-node-hostname&gt;
</code></pre></li>
</ol></li>
</ol></li>
</ul></li>
</ol>