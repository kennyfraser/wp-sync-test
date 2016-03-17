---
UID: 56df3ded8fff4
post_title: >
  Adding a Containerized Docker App to
  Marathon
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>In this tutorial, a custom Docker app is created and added to Marathon.</p>

<h3>Prerequisites</h3>

<ul>
<li><a href="https://www.docker.com">Docker</a> installed on your workstation</li>
<li><a href="https://hub.docker.com">Docker Hub</a> account</li>
<li><a href="../overview/installing/">DCOS and DCOS CLI</a> are installed</li>
</ul>

<h1>Create a custom Docker container</h1>

<ol>
<li><p>In the <code>dcos</code> directory created by the DCOS CLI installation script, create a new directory named <code>simple-docker-tutorial</code> and navigate to it:</p>

<pre><code>$ mkdir simple-docker-tutorial
$ cd simple-docker-tutorial
</code></pre></li>
<li><p>Create a file named <code>index.html</code> by using nano, or another text editor of your choice:</p>

<pre><code>$ nano index.html
</code></pre></li>
<li><p>Paste the following markup into <code>index.html</code> and save:</p>

<pre><code>&lt;html&gt;
    &lt;body&gt;
    &lt;h1&gt; Hello brave new world! &lt;/h1&gt;
    &lt;/body&gt;
&lt;/html&gt;
</code></pre></li>
<li><p>Create and edit a Dockerfile by using nano, or another text editor of your choice:</p>

<pre><code>$ nano Dockerfile
</code></pre></li>
<li><p>Paste the following commands into it and save:</p>

<pre><code>FROM nginx:1.9
COPY index.html /usr/share/nginx/html/index.html
</code></pre></li>
<li><p>Build the container, where <code>&lt;username&gt;</code> is your Docker Hub username:</p>

<pre><code>$ docker build -t &lt;username&gt;/simple-docker .
</code></pre></li>
<li><p>Log in to Docker Hub:</p>

<pre><code>$ docker login
</code></pre></li>
<li><p>Push your container to Docker Hub, where <code>&lt;username&gt;</code> is your Docker Hub username:</p>

<pre><code>$ docker push &lt;username&gt;/simple-docker
</code></pre></li>
</ol>

<h1>Add your Docker app to Marathon</h1>

<ol>
<li><p>Create a file named <code>nginx.json</code> by using nano, or another text editor of your choice:</p>

<pre><code>$ nano nginx.json
</code></pre></li>
<li><p>Paste the following into the <code>nginx.json</code> file. If you’ve created your own Docker container, replace the image name mesosphere with your Docker Hub username:</p>

<pre><code>{
    "id": "nginx",
    "container": {
    "type": "DOCKER",
    "docker": {
          "image": "mesosphere/simple-docker",
          "network": "BRIDGE",
          "portMappings": [
            { "hostPort": 80, "containerPort": 80, "protocol": "tcp"}
          ]
        }
    },
    "acceptedResourceRoles": ["slave_public"],
    "instances": 1,
    "cpus": 0.1,
    "mem": 64
}
</code></pre>

<p>This file specifies a simple Marathon application called “nginx” that runs one instance of itself on a public node.</p></li>
<li><p>Add the nginx Docker container to Marathon by using the DCOS command:</p>

<pre><code>$ dcos marathon app add nginx.json
</code></pre>

<p>If this is added successfully, there is no output.</p></li>
<li><p>Verify that the app is added:</p>

<pre><code>$ dcos marathon app list
ID      MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  CONTAINER  CMD                        
/nginx   64  0.1    0/1    ---      scale       DOCKER   None
</code></pre></li>
</ol>