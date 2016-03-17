---
UID: 56df3defbcc14
post_title: Installing the CLI
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<ul>
<li><a href="#linux">Installing the DCOS CLI on Unix, Linux, and OS X</a></li>
<li><a href="#windows">Installing the DCOS CLI on Windows</a></li>
</ul>

<h1><a name="linux"></a>Installing on Unix, Linux, and OS X</h1>

<p>The easiest way to install the DCOS CLI is by clicking the signpost icon in the lower left corner of the DCOS web interface. Or, you can manually install the CLI by following these instructions.</p>

<h4>Prerequisites</h4>

<ul>
<li>A system external to your DCOS cluster that you can install the CLI</li>
<li>Network access from the external system to your DCOS cluster</li>
<li>A command-line environment, such as Terminal</li>
<li>cURL: Installed by default on OS X and most Linux distributions.</li>
<li>Python 2.7.x or 3.4.x: Installed by default on OS X and most Linux distributions. (Python 3.5.x will not work.)</li>
<li>Pip: See the <a href="https://pip.pypa.io/en/stable/installing.html#install-pip" target="_blank">installation documentation</a>.</li>
<li>Git: 

<ul>
<li><strong>OS X:</strong> Get the installer from <a href="http://git-scm.com/download/mac">Git downloads</a>.</li>
<li><strong>Unix/Linux:</strong> See these <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git" target="_blank">installation instructions</a>.</li>
</ul></li>
</ul>

<h4>Installing the DCOS CLI</h4>

<ol>
<li><p>Install <strong>virtualenv</strong>:</p>

<pre><code>$ sudo pip install virtualenv
</code></pre>

<p><strong>Tip:</strong> On some older Python versions, you might receive an "Insecure Platform" warning, but you can safely ignore this. For more information, see <a href="https://virtualenv.pypa.io/en/latest/installation.html" target="_blank">https://virtualenv.pypa.io/en/latest/installation.html</a>.</p></li>
<li><p>From the command line, create a new directory named <code>dcos</code> and change your working directory to it.</p>

<pre><code>$ sudo mkdir dcos &amp;&amp; cd dcos
</code></pre></li>
<li><p>Download the DCOS CLI install script to your new directory:</p>

<pre><code>$ sudo curl -O https://downloads.mesosphere.io/dcos-cli/install.sh
</code></pre></li>
<li><p>Run the DCOS CLI install script, where <code>&lt;installdir&gt;</code> is the DCOS installation directory and <code>&lt;hosturl&gt;</code> is the hostname of your master node prefixed with <code>http://</code>:</p>

<pre><code>$ sudo bash install.sh &lt;install_dir&gt; http://&lt;hosturl&gt;
</code></pre>

<p>For example, if the hostname of your AWS master node is <code>dcos-ab-1234.us-west-2.elb.amazonaws.com</code>:</p>

<pre><code>$ sudo bash install.sh . http://dcos-ab-1234.us-west-2.elb.amazonaws.com
</code></pre></li>
<li><p>Follow the on-screen DCOS CLI instructions. You can ignore any Python "Insecure Platform" warnings.</p>

<p>You can now use the CLI.</p>

<pre><code>Command line utility for the Mesosphere Datacenter Operating 
System (DCOS). The Mesosphere DCOS is a distributed operating 
system built around Apache Mesos. This utility provides tools 
for easy management of a DCOS installation.

Available DCOS commands:

    config Get and set DCOS CLI configuration properties 
    help Display command line usage information 
    marathon Deploy and manage applications on the DCOS 
    node Manage DCOS nodes 
    package Install and manage DCOS packages 
    service Manage DCOS services 
    task Manage DCOS tasks

Get detailed command description with 'dcos &lt;command&gt; --help'.
</code></pre></li>
</ol>

<h1><a name="windows"></a>Installing on Windows</h1>

<h4>Prerequisites</h4>

<ul>
<li>A system external to your DCOS cluster onto which you will install the CLI</li>
<li>Network access from the external system to your DCOS cluster</li>
<li>Windows PowerShell: Installed by default on Windows 7 and later</li>
</ul>

<p>To install the DCOS CLI, you must first install Python, pip, Git, and virtualenv.</p>

<p><strong>Important:</strong> Disable any security or antivirus software before beginning the installation.</p>

<h4>Installing Python</h4>

<ol>
<li><p>Download the Python 2.7 or Python 3.4 installer <a href="https://www.python.org/downloads/windows/" target="_blank">here</a>.</p>

<p><strong>Important:</strong> You must use Python version 2.7.x or 3.4.x. Python version 3.5.x will not work with the DCOS CLI. Use the x86-64 installer for 64-bit versions of Windows and the x86 installer for 32-bit versions of Windows.</p></li>
<li><p>Run the installer and select the "Add python.exe to Path" option when you are prompted to "Customize Python" as shown in the screenshot below.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/01/python.png" rel="attachment wp-att-2944"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/01/python.png" alt="Python Installer with Path Option Selected" width="495" height="427" class="alignnone size-full wp-image-2944" /></a></p>

<p><strong>Important:</strong> If you do not select the "Add python.exe to Path" option, you must manually add the Python installation folder to your Path or the DCOS CLI will not work.</p></li>
</ol>

<h4>Upgrading pip</h4>

<p>Pip is included with Python, but you must upgrade pip for it to work properly with the DCOS CLI.</p>

<ol>
<li><p>Download the <code>get-pip.py</code> script from <a href="https://pip.pypa.io/en/stable/installing.html#install-pip" target="_blank">the pip download page</a>.</p></li>
<li><p>Run Powershell as Administrator by right-clicking the Start menu shortcut as shown in the screenshot below:</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/01/powershell.png" rel="attachment wp-att-2943"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/01/powershell.png" alt="Start Menu with &#039;Launch as Administrator&#039; Option Highlighted" width="392" height="622" class="alignnone size-full wp-image-2943" /></a></p>

<p><strong>Important:</strong> You must run Powershell as Administrator or the CLI installation will fail.</p></li>
<li><p>Run the script.</p>

<pre><code>PS C:&gt; python get-pip.py
</code></pre></li>
</ol>

<h4>Installing Git</h4>

<ol>
<li><p>Download the Git installer from <a href="http://git-scm.com/download/win" target="_blank">the Git download page</a>.</p></li>
<li><p>Run the installer.</p>

<p><strong>Tip:</strong> You might receive a security warning that blocks execution of the installer and states that "Windows protected your PC." It is safe to ignore this warning and click "Run Anyway."</p></li>
<li><p>During the Git install, select the "Use Git from the Windows Command Prompt" option when you are prompted as shown in the screenshot below.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2016/01/git.png" rel="attachment wp-att-2945"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/01/git.png" alt="Git Installer with Windows Command Prompt Option Enabled" width="499" height="387" class="alignnone size-full wp-image-2945" /></a></p>

<p><strong>Important:</strong> If you do not select the "Use Git from the Windows Command Prompt" option, you must manually add the Git installation folder to your Path or the DCOS CLI will not work.</p></li>
</ol>

<h4>Installing virtualenv</h4>

<ol>
<li><p>Run Powershell as Administrator.</p></li>
<li><p>Install <code>virtualenv</code>:</p>

<pre><code>PS C:&gt; pip install virtualenv
</code></pre>

<p><strong>Tip:</strong> You can safely ignore any Python "Insecure Platform" warnings. For more information, see <a href="https://virtualenv.pypa.io/en/latest/installation.html" target="_blank">https://virtualenv.pypa.io/en/latest/installation.html</a>.</p></li>
</ol>

<h4>Installing the CLI</h4>

<ol>
<li><p>Run Powershell as Administrator.</p></li>
<li><p>From the command line, create a new directory named <code>dcos</code> (it can be located wherever you wish) and change your working directory to it.</p>

<pre><code>PS C:&gt; md dcos


Directory: C:


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        1/29/2016   2:33 PM                dcos


PS C:&gt; cd dcos
PS C:dcos&gt;
</code></pre></li>
<li><p>Download the DCOS CLI Powershell install script <a href="https://downloads.mesosphere.io/dcos-cli/install.ps1">by clicking here</a>. Save the script to the <code>dcos</code> directory.</p>

<p>If you are running Windows 10, or if you are running an earlier version of Windows and have manually installed the <code>wget</code> software package, you can alternatively download the Powershell script directly from within Powershell:</p>

<pre><code>PS C:dcos&gt; wget https://downloads.mesosphere.io/dcos-cli/install.ps1 -OutFile install.ps1
</code></pre></li>
<li><p>Change the default security policy of your Powershell to allow the install script to run:</p>

<pre><code>PS C:dcos&gt; set-executionpolicy Unrestricted -Scope Process

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. 
Changing the execution policy might expose you to the security risks 
described in the about_Execution_Policies help topic at
http://go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the 
execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help 
(default is "N"): Y
</code></pre></li>
<li><p>Run the DCOS CLI install script, where <code>&lt;installdir&gt;</code> is the DCOS installation directory and <code>&lt;hosturl&gt;</code> is the hostname of your master node prefixed with <code>http://</code>:</p>

<pre><code>PS C:dcos&gt; .install.ps1 &lt;install_dir&gt; &lt;hosturl&gt;
</code></pre>

<p>For example, if the hostname of your AWS master node is <code>dcos-ab-1234.us-west-2.elb.amazonaws.com</code>:</p>

<pre><code>PS C:dcos&gt; .install.ps1 . http://dcos-ab-1234.us-west-2.elb.amazonaws.com
</code></pre>

<p><strong>Tip:</strong> You might receive a security warning. It is safe to ignore this warning and allow the script to run:</p>

<pre><code>Security Warning
Run only scripts that you trust. While scripts from the Internet can
be useful, this script can potentially harm your computer. Do you
want to run C:dcosinstall.ps1?
[D] Do not run  [R] Run once  [S] Suspend  [?] Help (default is "D"): R
</code></pre></li>
<li><p>Enter the Mesosphere verification code when prompted or hit ENTER to continue. You can ignore any Python "Insecure Platform" warnings.</p></li>
<li><p>Follow the on-screen DCOS CLI instructions to add the <code>dcos</code> command to your PATH and complete installation:</p>

<pre><code>PS C:dcos&gt; &amp; .activate.ps1; dcos help
</code></pre>

<p>You can now use the CLI.</p>

<pre><code>Command line utility for the Mesosphere Datacenter Operating 
System (DCOS). The Mesosphere DCOS is a distributed operating 
system built around Apache Mesos. This utility provides tools 
for easy management of a DCOS installation.

Available DCOS commands:

    config Get and set DCOS CLI configuration properties 
    help Display command line usage information 
    marathon Deploy and manage applications on the DCOS 
    node Manage DCOS nodes 
    package Install and manage DCOS packages 
    service Manage DCOS services 
    task Manage DCOS tasks

Get detailed command description with 'dcos &lt;command&gt; --help'.
</code></pre></li>
</ol>