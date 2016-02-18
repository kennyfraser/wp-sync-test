---
ID: 3144
post_title: 'Step 4: Configure and install DCOS'
author: Joel Hamill
post_date: 2016-02-06 07:53:38
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/manual-installation/step-4-configure-and-install-dcos/
published: true
header:
  - "1"
page_header:
  - "1"
page_options_require_authentication:
  - ""
hide_from_navigation:
  - "0"
hide_from_related:
  - "1"
header_0_background:
  - fill
header_0_background_fill_style:
  - dark
header_0_logo_style:
  - color-light
header_0_navigation_style:
  - light
page_header_0_show_page_header:
  - "0"
page_header_0_size:
  - default
page_header_0_fill_screen:
  - "0"
page_header_0_background:
  - transparent
page_header_0_show_background_image:
  - "0"
page_header_0_show_background_video:
  - "0"
page_header_0_headline:
  - ""
page_header_0_headline_size:
  - default
page_header_0_description:
  - ""
page_header_0_description_size:
  - default
page_header_0_show_image:
  - "0"
page_header_0_content_alignment:
  - center
page_header_0_content_style:
  - dark
page_header_0_actions:
  - "0"
page_header_0_show_actions_footnote:
  - "0"
page_header_0_show_video:
  - "0"
---
# <a name="config-json"></a>4\.1 Configure your cluster

In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.

1.  Run this command to create a hashed password for superuser authentication, where `<superuser_password>` is the superuser password. Use the hashed password key for the `superuser_password` parameter in your `config.yaml` file.
    
        $ sudo bash dcos_generate_config.ee.sh --hash-password <superuser_password>
        Extracting image from this script and loading into docker daemon, this step can take a few minutes
        dcos-genconf.9eda4ae45de5488c0c-c40556fa73a00235f1.tar
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        00:42:10 dcos_installer.action_lib.prettyprint:: ====> HASHING PASSWORD TO SHA512
        00:42:11 root:: Hashed password for 'password' key:
        $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1 
        

2.  Customize this `config.yaml` template file for your environment. <!-- bootstrap_url is changeable -->
    
          bootstrap_url: http://<bootstrap_ip>:<your_port>       
          cluster_name: '<cluster-name>'
          exhibitor_storage_backend: zookeeper
          exhibitor_zk_hosts: <host1>:<port1>
          exhibitor_zk_path: /dcos
          master_discovery: static 
          master_list:
          - <master-ip-1>
          - <master-ip-2>
          - <master-ip-3>
          superuser_username: <username>
          superuser_password: <hashed-password>
          resolvers:
          - <dns-resolver-1>
          - <dns-resolver-2>
        
    
    Specify these configuration parameters. <!-- log_directory: /genconf/logs, process_timeout: 120, ssh_key_path: /genconf/ssh-key -->
    
    **cluster_name**
    :   Specify the name of your cluster.
    
    **bootstrap_url**
    
    :   This parameter specifies the URI path for the DCOS installer to store the customized DCOS build files, which can be local (`bootstrap_url:file:///opt/dcos_install_tmp`) or hosted (`http://<your-web-server>`). By default this is set to `file:///opt/dcos_install_tmp` in the `config.yaml` template file, which is the location where the DCOS installer puts your install tarball.
        
        **Tip:** This parameter is for advanced users. The default value should work for most installations.
    
    **exhibitor_storage_backend**
    :   This parameter specifies the type of storage backend for Exhibitor. By default this is set to `zookeeper` in the `config.yaml` template file. During DCOS installation, a storage system is required for configuring and orchestrating Zookeeper with Exhibitor on the master nodes. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.
    
    **exhibitor_zk_hosts**
    :   Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it's configuration.
    
    **master_discovery**
    :   This parameter specifies the Mesos master discovery method. By default this is set to `static` in the `config.yaml` template file. The `static` method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address
    
    **master_list**
    :   Specify a list of your static master IP addresses as a YAML nested series (`-`).
    
    **superuser_password**
    :   This parameter specifies the hashed Admin password. This password is required for using DCOS. For example:
    
    **superuser_password**
    :   $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1
    
    **superuser_username**
    :   This parameter specifies the Admin username. This username is required for using DCOS.
    
    **resolvers**
    
    :   Specify a JSON-formatted list of DNS servers for your DCOS host nodes. You must include the escape characters (`\`) as shown in the template. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them.
        
        *Caution:* If you set the `resolvers` parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.
    
    For more configuration examples and all available options, see the [configuration file options][1].

3.  Save as `genconf/config.yaml`.

# <a name="install-bash"></a>4\.2 Install DCOS

In this step you create a custom DCOS build file on your bootstrap node and then install DCOS onto your cluster. With this method you package the DCOS distribution yourself and connect to every server manually and run the commands.

**Tip:** If something goes wrong and you want to rerun your setup, use these cluster [cleanup instructions][2].

**Prerequisites**

*   A `genconf/config.yaml` file that is optimized for [manual distribution of DCOS across your nodes][3].
*   A `genconf/ip-detect` [script][4]. 
*   The Docker nginx image must be on your bootstrap node. You can use this command to install the nginx container:
    
         docker pull nginx
        
    
    For more information, see <a href="https://hub.docker.com/_/nginx/" target="_blank">DockerHub nginx</a> documentation.

<!-- Early access URL: https://downloads.mesosphere.com/dcos/EarlyAccess/dcos_generate_config.sh -->

<!-- Stable URL: https://downloads.mesosphere.com/dcos/stable/dcos_generate_config.sh --> To install DCOS:

1.  From the `dcos` directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to `./genconf/serve/`.
    
    At this point your directory structure should resemble:
    
        ├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
        ├── dcos_generate_config.sh
        ├── genconf
        │   ├── config.yaml
        │   ├── ip-detect
        

2.  Run this command to generate your customized DCOS build file:
    
        $ sudo bash dcos_generate_config.sh
        
    
    **Tip:** For the install script to work, you must have created [genconf/config.yaml][3] and [genconf/ip-detect][4].

3.  From the `dcos` directory, run this command to host the DCOS install package through an nginx Docker container. For `<your-port>`, specify the port value that is used in the `bootstrap_url`.
    
        $ docker run -p <your-port>:80 -v $PWD/genconf/serve:/usr/share/nginx/html:ro nginx
        

4.  Run these commands on each of your master nodes in succession to install DCOS using your custom build file.
    
    **Tip:** Although there is no actual harm to your cluster, DCOS may issue error messages until all of your master nodes are configured.

5.  SSH to your node:
    
        $ ssh <master-ip>
        

6.  Make a new directory and navigate to it:
    
        $ mkdir /tmp/dcos && cd /tmp/dcos
        

7.  Download the DCOS installer from the nginx Docker container, where `<bootstrap-ip>` and `<your_port>` are specified in `bootstrap_url`:
    
        $ curl -O http://<bootstrap-ip>:<your_port>/dcos_install.sh
        

8.  Run this command to install DCOS on your master node:
    
        $ sudo bash dcos_install.sh master
        

9.  Run these commands on each of your agent nodes to install DCOS using your custom build file.

10. SSH to your node:
    
        $ ssh <master-ip>
        

11. Make a new directory and navigate to it:
    
        $ mkdir /tmp/dcos && cd /tmp/dcos
        

12. Download the DCOS installer from the nginx Docker container, where `<bootstrap-ip>` and `<your_port>` are specified in `bootstrap_url`:
    
        $ curl http://<bootstrap-ip>:<your_port>/dcos_install.sh
        

13. Run this command to install DCOS on your agent node:
    
        $ sudo bash dcos_install.sh slave
        

14. Monitor Exhibitor and wait for it to converge at `http://<master-ip>:8181/exhibitor/v1/ui/index.html`.
    
    **Tip:** This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a>
    
    When the status icons are green, you can access the DCOS web interface.

15. Launch the DCOS web interface at: `http://<load-balanced-ip>/`.

16. Click **Log In To DCOS**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a>

17. Enter your administrator username and password.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a>
    
    You are done!
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a>

### Next Steps

Now you can [assign user roles][5].

 [1]: ../configuration-parameters-1-5/
 [2]: http://mesos.apache.org/documentation/latest/containerizer/
 [3]: ../administration/introcli/
 [4]: ../step-3-ip-address-discovery-script/
 [5]: ../security-and-authentication/