---
UID: 56df3deb44940
post_title: Jenkins
post_excerpt: ""
layout: page
published: true
menu_order: 6
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Jenkins is a continuous integration and continuous delivery application. You can use Mesosphere DCOS to deploy Jenkins on your cluster.</p>

<p>Jenkins on DCOS allows you to scale your Jenkins cluster by dynamically creating and destroying Jenkins agents as demand increases or decreases and enables you to avoid the statically partitioned infrastructure typical of other Jenkins clusters.</p>

<p>To use Jenkins on DCOS, you must provide a mount point to a shared file system on each of your DCOS agents. There are a number of existing shared file system solutions, including NFS, HDFS (plus its NFS gateway), Ceph and more. The following instructions use NFS as the example.</p>

<h2>Installing Jenkins on DCOS</h2>

<h4>Prerequisites</h4>

<ul>
<li><p>The DCOS CLI must be <a href="https://docs.mesosphere.com/install/cli/">installed</a>.</p></li>
<li><p>A mount point to a shared file system at the same path on each of your DCOS agents. NFS guides are available for <a href="https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/ch-nfs.html">Red Hat Enterprise Linux</a>, <a href="https://help.ubuntu.com/14.04/serverguide/network-file-system.html">Ubuntu</a>, and <a href="https://coreos.com/os/docs/latest/mounting-storage.html#mounting-nfs-exports">CoreOS</a>.</p></li>
</ul>

<ol>
<li><p>Update your local cache of the DCOS package repository to pull in the Jenkins package:</p>

<pre><code>$ dcos package update
</code></pre></li>
<li><p>Create a Jenkins JSON configuration file that specifies any instance or site-specific information, such as the framework name and the base path to the NFS share on the DCOS or Mesos agent. See the <a href="http://mesosphere.github.io/jenkins-mesos/docs/configuration.html">Configuration Reference</a> for a complete example.</p>

<p>Save the file as <code>options.json</code>. This file is specified during DCOS Jenkins installation.</p>

<pre><code>{
    "jenkins": {
        "framework-name": "jenkins-myteam",
        "host-volume": "/mnt/nfs/jenkins_data"
    }
}
</code></pre>

<p>By choosing a unique framework name, you can run multiple Jenkins instances on the same cluster, sharing the cluster’s resources among all Jenkins masters. Each Jenkins instance creates a subdirectory inside the directory that you specified for the host volume. In this example, the data is stored on the NFS server at <code>/mnt/nfs/jenkins_data/jenkins-myteam</code>.</p></li>
<li><p>From the DCOS CLI, install Jenkins with the <code>options.json</code> file specified:</p>

<pre><code>$ dcos package install jenkins --options=options.json
</code></pre>

<p>The first time Jenkins runs, it populates the directory on your NFS share with a basic Jenkins configuration and a small set of plugins. If Marathon needs to restart the task on a different host, the container automatically mounts your existing data directory, including job configurations, build history and installed plugins. For future versions of DCOS, we are planning add support for other distributed file systems and leverage the persistence primitives available in recent versions of Mesos.</p></li>
<li><p>Verify that Jenkins is installed and healthy.</p>

<ul>
<li><p>From the DCOS CLI, enter this command to show the installed packages:</p>

<pre><code>$ dcos package list
</code></pre></li>
<li><p>From the DCOS web interface, go to the Services tab and confirm that Jenkins is running at <code>/#/services/</code>. <!-- screenshot of web UI --></p></li>
</ul></li>
<li><p>Open a browser and navigate to the Jenkins web interface at <code>http://&lt;hostname&gt;/service/jenkins</code>.</p></li>
</ol>

<h2>Uninstalling Jenkins on DCOS</h2>

<ol>
<li><p>From the DCOS CLI, uninstall Jenkins:</p>

<pre><code>$ dcos package uninstall jenkins
</code></pre></li>
</ol>

<p><strong>Note:</strong> You must manually clean up any job configurations and/or build data was stored on your NFS share.</p>