---
UID: 56df3def020dd
post_title: Kubernetes
post_excerpt: ""
layout: page
published: true
menu_order: 9
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p><strong>Disclaimer:</strong> Kubernetes is available at the alpha level and is not recommended for Mesosphere DCOS production systems.</p>

<p>Kubernetes is an open-source container orchestration project introduced by Google. With the Mesosphere DCOS, you can deploy Kubernetes in your own datacenter or in the cloud. The Kubernetes service on DCOS <a href="https://github.com/kubernetes/kubernetes/blob/release-1.1/contrib/mesos/README.md">adds several benefits to standalone Kubernetes</a>, including high availability, easy installation, and easy maintenance. Kubernetes on DCOS runs reliably alongside other popular next-generation services on the same cluster without competing for resources.</p>

<p>The current Kubernetes alpha <a href="https://github.com/mesosphere/kubernetes/releases/tag/v0.7.2-v1.1.5" target="_blank">DCOS package v0.7.2-v1.1.5</a> is based on <a href="https://github.com/GoogleCloudPlatform/kubernetes/releases/tag/v1.1.5" target="_blank">Kubernetes v1.1.5</a>. For more information see the <a href="https://github.com/mesosphere/kubernetes-mesos" target="_blank">Kubernetes-Mesos project on GitHub</a>. There are behavioral differences between the Kubernetes DCOS service and standalone Kubernetes, notably the <a href="https://github.com/kubernetes/kubernetes/blob/master/contrib/mesos/docs/scheduler.md">behavior of the scheduler</a>.</p>

<h1><a name="install"></a>Installing Kubernetes on DCOS</h1>

<p><strong>Prerequisite</strong></p>

<ul>
<li>The DCOS CLI must be <a href="/install/cli/">installed</a>.</li>
</ul>

<ol>
<li><p>Add the Multiverse package repository source and make sure the repository content is current:</p>

<pre><code>$ dcos config prepend package.sources https://github.com/mesosphere/multiverse/archive/version-2.x.zip
$ dcos package update
</code></pre></li>
<li><p>Install the <a href="https://github.com/mesosphere/etcd-mesos">etcd-mesos</a> service, which provides failover for etcd. By default, the Kubernetes DCOS service installs a single-node etcd cluster.</p>

<pre><code>$ dcos package install etcd
</code></pre></li>
<li><p>Verify that etcd-mesos service is installed and healthy before going on to the next step. The etcd-mesos service takes a few minutes to deploy.</p>

<pre><code>$ dcos package list
NAME  VERSION  APP    COMMAND  DESCRIPTION
etcd  0.0.1    /etcd  ---      A distributed consistent key-value store for shared configuration and service discovery.
</code></pre></li>
<li><p>Create a Kubernetes JSON configuration file that specifies the etcd-mesos services and save as <code>options.json</code>. This file is specified during DCOS Kubernetes installation.</p>

<pre><code>{
  "kubernetes": {
    "etcd-mesos-framework-name": "etcd"
  }
}
</code></pre></li>
<li><p>From the DCOS CLI, install Kubernetes with the <code>options.json</code> file specified:</p>

<pre><code>$ dcos package install --options=options.json kubernetes
</code></pre></li>
<li><p>Verify that Kubernetes is installed and healthy. The Kubernetes cluster takes a few minutes to deploy.</p>

<ul>
<li><p>From the DCOS CLI, enter this command to show the installed packages:</p>

<pre><code>    $ dcos package list
    NAME VERSION APP COMMAND DESCRIPTION
    kubernetes v0.7.2-v1.1.5-alpha /kubernetes kubernetes Kubernetes is an open source system for managing containerized applications across multiple hosts, providing basic mechanisms for deployment, maintenance, and scaling of applications.
</code></pre></li>
<li><p>From the DCOS web interface, go to the Services tab and confirm that Kubernetes is running at /#/services/.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/kubernetestask.png" rel="attachment wp-att-1401"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/kubernetestask.png" alt="kubernetestask" width="721" height="48" class="alignnone size-full wp-image-1401" /></a></p></li>
<li><p>Open a browser and navigate to the Kubernetes web interface at <code>&lt;hostname&gt;/service/kubernetes/</code>.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/kubernetes-interface.png" rel="attachment wp-att-1404"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/kubernetes-interface.png" alt="kubernetes-interface" width="674" height="614" class="alignnone size-full wp-image-1404" /></a></p></li>
<li><p>Open a browser and navigate to the Mesos web interface at <code>&lt;hostname&gt;/mesos</code>. Verify that the Kubernetes framework has registered and is starting tasks.</p></li>
</ul></li>
<li><p>Install the kubectl DCOS subcommand:</p>

<pre><code>$ dcos kubectl
</code></pre></li>
<li><p>Verify that Kube-DNS &amp; Kube-UI are deployed, running, and ready. The kube-ui service is automatically deployed on the cluster.</p>

<pre><code>$ dcos kubectl get pods --namespace=kube-system
NAME                READY     STATUS    RESTARTS   AGE
kube-dns-v8-tjxk9   4/4       Running   0          1m
kube-ui-v2-tjq7b    1/1       Running   0          1m
</code></pre>

<p><strong>Tip:</strong> Names and versions may vary.</p></li>
</ol>

<p>Now that Kubernetes is installed on DCOS, you can explore the <a href="http://kubernetes.io/v1.1/examples/">Kubernetes Examples</a> or the <a href="http://kubernetes.io/v1.1/docs/user-guide/README.html">Kubernetes User Guide</a>.</p>

<h1><a name="uninstall"></a>Uninstalling Kubernetes</h1>

<ol>
<li><p>Stop and delete all replication controllers and pods in each namespace:</p>

<p>Before uninstalling Kubernetes, destroy all the pods and replication controllers. The uninstall process will try to do this itself, but by default it times out quickly and may leave your cluster in a dirty state:</p>

<pre><code>$ dcos kubectl delete rc,pods --all --namespace=default
$ dcos kubectl delete rc,pods --all --namespace=kube-system
</code></pre></li>
<li><p>Validate that all pods have been deleted</p>

<pre><code>$ dcos kubectl get pods --all-namespaces
</code></pre></li>
<li><p>Uninstall Kubernetes</p>

<pre><code>$ dcos package uninstall kubernetes
</code></pre></li>
</ol>

<h3><a name="more-info"></a>For more information</h3>

<ul>
<li><strong>Kubernetes</strong> 

<ul>
<li><a href="http://kubernetes.io/v1.1/docs/user-guide/README.html">User Guide</a></li>
<li><a href="http://kubernetes.io/v1.1/examples/">Examples</a></li>
<li><a href="https://github.com/kubernetes/kubernetes">Source</a></li>
</ul></li>
<li><strong>Kubernetes-Mesos</strong> 

<ul>
<li><a href="http://kubernetes.io/v1.1/docs/getting-started-guides/mesos.html">Getting Started Guide</a></li>
<li><a href="https://github.com/mesosphere/kubernetes/releases">Release Notes</a></li>
<li><a href="https://github.com/mesosphere/kubernetes/blob/v0.7.2-v1.1.5/contrib/mesos/README.md">Documentation</a></li>
<li><a href="https://github.com/mesosphere/kubernetes/blob/v0.7.2-v1.1.5/contrib/mesos/docs/architecture.md">Architecture</a></li>
<li><a href="https://github.com/mesosphere/kubernetes/blob/v0.7.2-v1.1.5/contrib/mesos/docs/issues.md">Known Issues</a></li>
<li><a href="https://github.com/mesosphere/kubernetes-mesos">DCOS Package Source</a></li>
</ul></li>
</ul>