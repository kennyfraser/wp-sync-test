---
UID: 56df3ded6e06b
post_title: Deploying a local package repository
post_excerpt: ""
layout: page
published: true
menu_order: 200
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>Follow these instructions to download and configure a local <a href="http://mesosphere.github.io/universe/" target="_blank">DCOS Universe</a>. After downloading the Universe components, you can install and run DCOS services on a datacenter without internet access.</p>

<h4>Prerequisites</h4>

<ul>
<li>DCOS cluster</li>
<li>8&#046;5 GB of disk space</li>
</ul>

<p>To install the DCOS local Universe:</p>

<ol>
<li><p>Download the package binaries as a single tarball. This package includes regular tarballs and Docker images.</p>

<pre><code>$ wget https://s3-us-west-2.amazonaws.com/local-universe-archive/dcos-services.tar.gz
</code></pre>

<p>There is a tarball named <code>docker-images.tar</code> that contains all of the required Docker images. Here is list of the tags and image IDs:</p>

<table>
<thead>
<tr>
  <th>REPOSITORY</th>
  <th>TAG</th>
  <th>IMAGE ID</th>
</tr>
</thead>
<tbody>
<tr>
  <td>docker.io/mesosphere/marathon</td>
  <td>v0.11.1</td>
  <td>5e187be16235</td>
</tr>
<tr>
  <td>docker.io/arangodb/arangodb-mesos</td>
  <td>latest</td>
  <td>17881b9777fe</td>
</tr>
<tr>
  <td>docker.io/mesosphere/chronos</td>
  <td>chronos-...</td>
  <td>99852a4a1a6e</td>
</tr>
<tr>
  <td>docker.io/mesosphere/spark</td>
  <td>1&#046;5.0-...</td>
  <td>23b1cfe8e04a</td>
</tr>
</tbody>
</table></li>
<li><p>Extract the tarball:</p>

<pre><code>$ tar xvf dcos-services.tar.gz
</code></pre></li>
<li><p>Host the binaries in an artifact store.</p>

<ol>
<li><p>For the Docker images, you can do the following:</p>

<pre><code>$ docker run -d -p 5000:5000 --restart=always --name registry registry:2
$ docker load &lt; dcos-services/docker-images.tar # This will take a long time.
$ for img in $(docker images | awk '{ print $1 ":" $2 }' | grep mesos); do
    img_tag=$(ip addr show eth0 | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | head -1):5000/$img
    docker tag $img $img_tag
    docker push $img_tag
  done
</code></pre>

<p><strong>Important:</strong> You must have all of the Docker daemons in your cluster setup with <code>--insecure-registry</code>. For more information, see the <a href="https://docs.docker.com/registry/">Docker Registry documentation</a>.</p></li>
<li><p>Serve the other artifacts with this command:</p>

<pre><code>$ docker run -p 80:80 --name some-nginx -v $(pwd)/dcos-services:/usr/share/nginx/html:ro -d nginx
</code></pre></li>
</ol></li>
<li><p>On a machine with internet access, clone the Mesosphere Universe repository:</p>

<pre><code>$ git clone https://github.com/mesosphere/universe.git
$ cd universe
$ git checkout a0b1317deccb158b11eb7fca942e17460f3640d2
</code></pre>

<p><strong>Tip:</strong> The remaining steps do not require internet access.</p></li>
<li><p>Update your local Universe packages and then go to the <a href="#next">next step</a>.</p>

<ul>
<li><a href="#cassandra">Cassandra</a></li>
<li><a href="#chronos">Chronos</a></li>
<li><a href="#kafka">Kafka</a></li>
<li><a href="#hdfs">HDFS</a></li>
<li><a href="#marathon">Marathon</a></li>
<li><a href="#spark">Spark</a></li>
</ul>

<h3><a name="arangodb"></a>ArangoDB</h3>

<ol>
<li><p>In the <code>universe/repo/packages/A/arangodb/2/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "assets": {
    "container": {
      "docker": {
        "17881b9777fe": "&lt;local-docker-hostname&gt;/agangodb/arangodb-mesos:latest"
      }
    }
  },
  "images": {
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/arangodb_small.png",
    "icon-medium": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/arangodb_medium.png",
    "icon-large": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/arangodb_large.png"
  }
}
</code></pre></li>
</ol>

<h3><a name="cassandra"></a>Cassandra</h3>

<ol>
<li><p>In the <code>universe/repo/packages/C/cassandra/3/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "assets": {
    "uris": {
      "cassandra-mesos-0-2-0-1-tar-gz": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/cassandra-mesos-0.2.0-1.tar.gz",
      "jre-7u76-linux-x64-tar-gz": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/jre-7u76-linux-x64.tar.gz"
    }
  },
  "images": {
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/cassandra-small.png",
    "icon-medium": "https://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/cassandra-medium.png",
    "icon-large": "https://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/cassandra-large.png"
  }
}
</code></pre></li>
</ol>

<h3><a name="chronos"></a>Chronos</h3>

<ol>
<li><p>In the <code>universe/repo/packages/C/chronos/3/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "assets": {
    "container": {
      "docker": {
        "99852a4a1a6e": "&lt;local-docker-hostname&gt;/mesosphere/chronos:chronos-2.4.0-0.1.20150828104228.ubuntu1404-mesos-0.23.0-1.0.ubuntu1404"
      }
    }
  },
  "images": {
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-chronos-small.png",
    "icon-medium": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-chronos-medium.png",
    "icon-large": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-chronos-large.png"
  },
}
</code></pre></li>
</ol>

<h3><a name="kafka"></a>Kafka</h3>

<ol>
<li><p>In the <code>universe/repo/packages/K/kafka/2/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "assets": {
    "uris": {
      "jre-7u79-openjdk-tgz": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/jre-7u79-openjdk.tgz",
      "kafka_2-10-0-8-2-1-tgz": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/kafka_2.10-0.8.2.1.tgz",
      "kafka-mesos-0-9-2-0-jar": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/kafka-mesos-0.9.2.0.jar"
    }
  },
  "images": {
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-small.png",
    "icon-medium": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-medium.png",
    "icon-large": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-large.png",
    "screenshots": ["http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/screen-0-kafka.png"]
  }
}
</code></pre></li>
</ol>

<h3><a name="hdfs"></a>HDFS</h3>

<ol>
<li><p>In the <code>universe/repo/packages/H/hdfs/4/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "images": {
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-hdfs-small.png",
    "icon-medium": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-hdfs-medium.png",
    "icon-large": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-hdfs-large.png"
  },
  "assets": {
    "uris": {
      "hdfs-mesos-0-1-5-tgz": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/hdfs-mesos-0.1.5.tgz",
      "jre-7u76-linux-x64-tar-gz": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/jre-7u76-linux-x64.tar.gz"
    }
  }
}
</code></pre></li>
</ol>

<h3><a name="marathon"></a>Marathon</h3>

<ol>
<li><p>In the <code>universe/repo/packages/M/marathon/8/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "images": {
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-marathon-small.png",
    "icon-medium": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-marathon-medium.png",
    "icon-large": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-marathon-large.png"
  },
  "assets": {
    "container": {
      "docker": {
        "5e187be16235": "&lt;local-docker-hostname&gt;/mesosphere/marathon:v0.11.1"
      }
    }
  }
}
</code></pre></li>
</ol>

<h3><a name="spark"></a>Spark</h3>

<ol>
<li><p>In the <code>universe/repo/packages/S/spark/5/resource.json</code> file, replace all of the URLs with the locally hosted URLs. For example:</p>

<pre><code>{
  "assets": {
    "uris": {
      "log4j-properties": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/log4j.properties"
    },
    "container": {
      "docker": {
        "23b1cfe8e04a": "&lt;local-docker-hostname&gt;/mesosphere/spark:1.5.0-multi-roles-v2-bin-2.4.0"
      }
    }
  },
  "images": {
    "icon-medium": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-spark-medium.png",
    "icon-small": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-spark-small.png",
    "icon-large": "http://&lt;local-http-hostname&gt;/&lt;path-to-file&gt;/icon-service-spark-large.png"
  }
}
</code></pre></li>
</ol></li>
<li><p><a name="next"></a>Set the DCOS CLI package source to your local Universe repository:</p>

<pre><code>$ dcos config set package.sources '["file:///&lt;path-to-cloned-repo&gt;"]'
</code></pre></li>
<li><p>Turn off CLI usage and exception reporting:</p>

<pre><code>$ dcos config set core.reporting false
</code></pre></li>
<li><p>Update the local cache of the Universe repository.</p>

<pre><code>$ dcos package update
</code></pre></li>
<li><p>Install DCOS services. For example, to install Cassandra:</p>

<pre><code>dcos package install cassandra
</code></pre></li>
</ol>