---
ID: 3144
post_title: 'Step 2: Configure and install DCOS'
author: Joel Hamill
post_date: 2016-02-06 07:53:38
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/manual-installation/configure-and-install-dcos/
published: true
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: true
page_options_show_link_unauthenticated: false
---
The DCOS installation creates these folders:

*   `/opt/mesosphere`
    :   Contains all the DCOS binaries, libraries, cluster configuration. Do not modify.

*   `/etc/systemd/system/dcos.target.wants`
    :   Contains the systemd services which start the things that make up systemd. They must live outside of `/opt/mesosphere` because of systemd constraints.

*   Various units prefixed with `dcos` in `/etc/systemd/system`
    :   Copies of the units in `/etc/systemd/system/dcos.target.wants`. They must be at the top folder as well as inside `dcos.target.wants`.

# <a name="config-json"></a>Configure your cluster

In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.

1.  From the bootstrap node, run this command to create a hashed password for superuser authentication, where `<superuser_password>` is the superuser password. Use the hashed password key for the `superuser_password_hash` parameter in your `config.yaml` file.
    
        $ sudo bash dcos_generate_config.sh --hash-password <superuser_password>
        
    
    Here is an example of a hashed password output.
    
        Extracting image from this script and loading into docker daemon, this step can take a few minutes
        dcos-genconf.9eda4ae45de5488c0c-c40556fa73a00235f1.tar
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        00:42:10 dcos_installer.action_lib.prettyprint:: ====> HASHING PASSWORD TO SHA512
        00:42:11 root:: Hashed password for 'password' key:
        $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1
        

2.  Create a configuration file and save as `genconf/config.yaml`. You can use this example as a template.
    
          ---
          bootstrap_url: http://<bootstrap_public_ip>:<your_port>       
          cluster_name: '<cluster-name>'
          exhibitor_storage_backend: zookeeper
          exhibitor_zk_hosts: <host1>:2181,<host2>:2181,<host3>:2181
          exhibitor_zk_path: /dcos
          master_discovery: static 
          master_list:
          - <master-private-ip-1>
          - <master-private-ip-2>
          - <master-private-ip-3>
          superuser_username: <username>
          superuser_password_hash: <hashed-password>
          resolvers:
          - <dns-resolver-1>
          - <dns-resolver-2>
        
    
    **bootstrap_url**
    :   Specify the URI path for the DCOS installer to store the customized DCOS build files. For example, `http://<bootstrap_ip>:<your_port>`. By default this is set to `file:///opt/dcos_install_tmp` in the `config.yaml` template file, but for manual installations you must change this value.
    
    **cluster_name**
    :   Specify the name of your cluster.
    
    **exhibitor_storage_backend**
    :   This parameter specifies the type of storage backend for Exhibitor. In the example file above, a ZooKeeper instance is used for external storage. During DCOS installation, a storage system is required for configuring and orchestrating ZooKeeper with Exhibitor on the master nodes. Exhibitor automatically configures your ZooKeeper installation on the master nodes during your DCOS installation. Multiple ZooKeeper instances are recommended for failover in production environments.
    
    **exhibitor_zk_hosts**
    :   Specify a comma-separated list of one or more ZooKeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this ZooKeeper cluster to orchestrate it's configuration. Where `<host1>` is your internal bootstrap node IP and port `2181` is the default ZooKeeper port.
    
    **master_discovery**
    :   This parameter specifies the Mesos master discovery method. By default this is set to `static` in the `config.yaml` template file. The `static` method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address
    
    **master_list**
    :   Specify a list of your static master IP addresses as a YAML nested series (`-`).
    
    **resolvers**
    
    :   Specify a YAML-formatted nested series (`-`) of DNS resolvers for your DCOS host nodes, or accept the default <a href="https://developers.google.com/speed/public-dns/docs/using" target="_blank">Google Public DNS IP addresses (IPv4)</a> of `8.8.8.8` and `8.8.4.4`. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them. If you have no internal hostnames to resolve, it
        
        *Caution:* If you set the `resolvers` parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.
    
    **superuser_password_hash**
    :   This parameter specifies the hashed Admin password. This password is required for using DCOS. See step 1 for more information on how to create.
    
    **superuser_username**
    :   Specify a new username for the `Bootstrap superuser`. This username is required for using DCOS. For more information, see <a href="../security-and-authentication/managing-authorization/" target="_blank">Managing Authentication</a>.
    
    For the complete list of available configuration options and parameters, see the [Configuration Parameters][1] documentation.

# <a name="install-bash"></a>Install DCOS

In this step you create a custom DCOS build file on your bootstrap node and then install DCOS onto your cluster. With this method you package the DCOS distribution yourself and connect to every server manually and run the commands.

**Tip:** If something goes wrong and you want to rerun your setup, use these cluster [cleanup instructions][2].

**Prerequisites**

*   A `genconf/config.yaml` file that is optimized for [manual distribution of DCOS across your nodes][3].
*   A `genconf/ip-detect` [script][4].

<!-- Early access URL: https://downloads.mesosphere.com/dcos/EarlyAccess/dcos_generate_config.sh -->

<!-- Stable URL: https://downloads.mesosphere.com/dcos/stable/dcos_generate_config.sh --> To install DCOS:

1.  From the bootstrap node, run the DCOS installer shell script to generate a customized DCOS build file. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to `./genconf/serve/`.
    
    At this point your directory structure should resemble:
    
        ├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
        ├── dcos_generate_config.sh
        ├── genconf
        │   ├── config.yaml
        │   ├── ip-detect
        

2.  Run this command to generate your customized DCOS build file:
    
        $ sudo bash dcos_generate_config.sh
        
    
    **Tip:** For the install script to work, you must have created [genconf/config.yaml][3] and [genconf/ip-detect][4].

3.  From your home directory, run this command to host the DCOS install package through an nginx Docker container. For `<your-port>`, specify the port value that is used in the `bootstrap_url`.
    
        $ sudo docker run -d -p <your-port>:80 -v $PWD/genconf/serve:/usr/share/nginx/html:ro nginx
        

4.  Run these commands on each of your master nodes in succession to install DCOS using your custom build file.
    
    **Tip:** Although there is no actual harm to your cluster, DCOS may issue error messages until all of your master nodes are configured.
    
    1.  SSH to your master nodes:
        
            $ ssh <master-ip>
            
    
    2.  Make a new directory and navigate to it:
        
            $ mkdir /tmp/dcos && cd /tmp/dcos
            
    
    3.  Download the DCOS installer from the nginx Docker container, where `<bootstrap-ip>` and `<your_port>` are specified in `bootstrap_url`:
        
            $ curl -O http://<bootstrap-ip>:<your_port>/dcos_install.sh
            
    
    4.  Run this command to install DCOS on your master nodes:
        
            $ sudo bash dcos_install.sh master
            

5.  Run these commands on each of your agent nodes to install DCOS using your custom build file.
    
    1.  SSH to your agent nodes:
        
            $ ssh <agent-ip>
            
    
    2.  Make a new directory and navigate to it:
        
            $ mkdir /tmp/dcos && cd /tmp/dcos
            
    
    3.  Download the DCOS installer from the nginx Docker container, where `<bootstrap-ip>` and `<your_port>` are specified in `bootstrap_url`:
        
            $ curl -O http://<bootstrap-ip>:<your_port>/dcos_install.sh
            
    
    4.  Run this command to install DCOS on your agent nodes:
        
            $ sudo bash dcos_install.sh slave
            

6.  Monitor Exhibitor and wait for it to converge at `http://<master-ip>:8181/exhibitor/v1/ui/index.html`.
    
    **Tip:** This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a>
    
    When the status icons are green, you can access the DCOS web interface.

7.  Launch the DCOS web interface at: `http://<master-node-public-ip>/`.

8.  Click **Log In To DCOS**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a>

9.  Enter your administrator username and password.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a>
    
    You are done!
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a>

### Next Steps

Now you can [assign user roles][5].

 [1]: ../configuration-parameters/
 [2]: https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/
 [3]: ../administration/introcli/
 [4]: ../step-3-ip-address-discovery-script/
 [5]: ../security-and-authentication/