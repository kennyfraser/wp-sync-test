---
ID: 3144
post_title: 'Step 4: Configure and install DCOS'
author: Joel Hamill
post_date: 2016-02-06 07:53:38
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/step-4-configure-and-install-dcos/
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
Choose your DCOS installation method:

*   [Using the GUI installer to install DCOS][1]
*   [Using SSH to distribute DCOS across your nodes][2]
*   [Manually distributing DCOS across your nodes][3]

<!-- [Step 3: Configure and install DCOS using SSH to distribute DCOS across your nodes][5] -->

# <a name="gui"></a>Using the GUI installer to install DCOS

1.  From your terminal, start the DCOS installer with this command.
    
    **Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.
    
        $ sudo bash dcos_generate_config.ee.sh --web
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        16:36:09 dcos_installer.action_lib.prettyprint:: ====> Starting DCOS installer in web mode
        16:36:09 root:: Starting server ('0.0.0.0', 9000)
        
    
    You can add the verbose (`-v`) flag to see the debug output:
    
        $ sudo bash dcos_generate_config.ee.sh --web -v
        

2.  Launch the DCOS web installer in your browser at: `http://<public-ip>:9000`.

3.  Click **Begin Installation**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin.png" rel="attachment wp-att-3190"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-begin-800x510.png" alt="ui-installer-begin" width="800" height="510" class="alignnone size-large wp-image-3190" /></a>
    
    **Important:** If you exit your GUI installation before launching DCOS, you must do this before reinstalling:
    
    *   SSH to each node in your cluster and run `rm -rf /opt/mesosphere`.
    *   SSH to your bootstrap master node and run `rm -rf /var/lib/zookeeper`

4.  Specify your Deployment and DCOS Environment settings:
    
    ### Deployment Settings
    
    **Master IP Address List**
    :   Specify a comma-separated list of your static master IP addresses.
    
    **Agent IP Address List**
    :   Specify a comma-separated list of your static agent IP addresses.
    
    **Accessible Master IP Address**
    :   Specify the IP address to a publicly accessible proxy to one of your master nodes. If you either don't have a proxy or you already have access to the network where you are deploying this cluster, you can use one of the master IP's that you specified in the master list. This proxy IP address is used to access the DCOS web interface after the deployment succeeds.
    
    **SSH Username**
    :   Specify the SSH username, for example `centos`. SSH Listening Port
    :   Specify the port to SSH to, for example `22`. SSH Key
    :   Specify the SSH key with access to your master IPs.
    
    ### DCOS Environment Settings
    
    **Username**
    :   Specify the administrator username. This username is required for using DCOS.
    
    **Password**
    :   Specify the administrator password. This password is required for using DCOS.
    
    **Bootstrapping Zookeeper IP Address(es)**
    :   Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it’s configuration.
    
    **Bootstrapping Zookeeper Port**
    :   Specify the Zookeeper port. For example, `2181`.
    
    **Upstream DNS Servers**
    :   Specify the DNS servers, which can be on your private network or the public internet. Caution: If you set this parameter incorrectly you will have to reinstall DCOS. For more information about service discovery, see this [documentation][4].
    
    **IP Detect Script**
    :   Specify an IP detect script to broadcast the IP address of each node across the cluster. For more information, see the [documentation][5].

5.  Click **Run Pre-Flight**. The preflight script validates that your cluster is installable. This step can take up to 15 minutes to complete. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" rel="attachment wp-att-3197"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-pre-flight1.png" alt="ui-installer-pre-flight1" width="626" height="405" class="alignnone size-full wp-image-3197" /></a>

6.  Click **Deploy** to install DCOS on your cluster. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" rel="attachment wp-att-3195"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-deploy1.png" alt="ui-installer-deploy1" width="628" height="406" class="alignnone size-full wp-image-3195" /></a>

7.  Click **Run Post-Flight**. If errors any errors are found, fix and then click **Retry**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" rel="attachment wp-att-3196"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-post-flight1.png" alt="ui-installer-post-flight1" width="623" height="366" class="alignnone size-full wp-image-3196" /></a>
    
    **Tip:** You can click **Download Logs** to view your logs locally.

8.  Click **Log In To DCOS**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a>

9.  Enter your administrator username and password.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" rel="attachment wp-att-3182"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-5.png" alt="ui-installer-5" width="368" height="388" class="alignnone size-full wp-image-3182" /></a>
    
    You are done!
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" alt="dashboardsmall" width="1338" height="828" class="alignnone size-full wp-image-1120" /></a>

### Next Steps

Now you can [assign user roles][6].

# <a name="ssh"></a>Using SSH to distribute DCOS across your nodes

### <a name="config-json"></a>4\.1 Configure your cluster

In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.

1.  Customize this `config.yaml` template file for your environment. <!-- do not change bootstrap_url -->
    
          cluster_name: '<cluster-name>'
          ##########################################
          # DO NOT CHANGE the bootstrap_url value, # 
          # unless you have moved installer assets.# 
          ##########################################
          bootstrap_url: file:///opt/dcos_install_tmp
          cluster_name: <cluster-name>
          exhibitor_storage_backend: zookeeper
          exhibitor_zk_hosts: <host1>:<port1>
          exhibitor_zk_path: /dcos
          log_directory: /genconf/logs
          master_discovery: static 
          master_list:
          - <master-ip-1>
          - <master-ip-2>
          - <master-ip-3>
          resolvers:
          - <dns-resolver-1>
          - <dns-resolver-2>
          ssh_port: '<port-number>'
          ssh_user: <username>
          superuser_password: <admin-username>
          superuser_username: <admin-password>
          target_hosts:
          - <target-host-1>
          - <target-host-2>
          - <target-host-3>
          - <target-host-4>
          - <target-host-5>
        
    
    Specify these configuration parameters. <!-- log_directory: /genconf/logs, process_timeout: 120, ssh_key_path: /genconf/ssh-key -->
    
    **bootstrap_url**
    
    :   This parameter specifies the URI path for the DCOS installer to store the customized DCOS build files. Use the default value of `bootstrap_url: file:///opt/dcos_install_tmp`, unless you have moved the installer assets from their default location.
    
    **cluster_name**
    :   Specify the name of your cluster.
    
    **exhibitor_storage_backend**
    :   This parameter specifies the type of storage backend for Exhibitor. By default this is set to `zookeeper` in the `config.yaml` template file. During DCOS installation, a storage system is required for configuring and orchestrating Zookeeper with Exhibitor on the master nodes. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.
    
    **exhibitor_zk_hosts**
    :   Specify a comma-separated list of one or more Zookeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this Zookeeper cluster to orchestrate it's configuration.
    
    **log_directory**
    :   This parameter specifies the path to the installer host logs from the SSH processes. By default this is set to `/genconf/logs`. This should not be changed because `/genconf` is local to the container that is running the installer, and is a mounted volume.
    
    **master_discovery**
    :   This parameter specifies the Mesos master discovery method. By default this is set to `static` in the `config.yaml` template file. The `static` method uses the Mesos agents to discover the masters by giving each agent a static list of master IPs. The masters must not change IP addresses, and if a master is replaced, the new master must take the old master's IP address
    
    **master_list**
    :   Specify a list of your static master IP addresses as a YAML nested series (`-`).
    
    **resolvers**
    
    :   Specify a JSON-formatted list of DNS servers for your DCOS host nodes. You must include the escape characters (`\`) as shown in the template. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them.
        
        *Caution:* If you set the `resolvers` parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.
    
    **ssh_port**
    :   This parameter specifies the port to SSH to, for example `22`.
    
    **ssh_user**
    :   This parameter specifies the SSH username, for example `centos`.
    
    **superuser_password**
    :   This parameter specifies the Admin password. This password is required for using DCOS.
    
    **superuser_username**
    :   This parameter specifies the Admin username. This username is required for using DCOS.
    
    **target_hosts**
    :   This parameter specifies a complete list of IPv4 addresses to your target DCOS hosts, including agent host names. This must be a YAML-formatted nested series (-).
    
    For more configuration examples and all available options, see the [configuration file options][5].

2.  Save as `genconf/config.yaml`.

3.  Move your private RSA key to `genconf/ssh_key`. For more information, see the [ssh_key_path][7] parameter.
    
        $ cp <path-to-key> genconf/ssh_key && chmod 0600 genconf/ssh_key
        

### <a name="install-bash"></a>4\.2 Install DCOS

In this step you create a custom DCOS build file on your workstation and then install DCOS onto your cluster. With this installation method you create a bootstrap server that uses your SSH key and connects to every node to automate the deployment.

**Tip:** If something goes wrong and you want to rerun your setup, use these cluster [cleanup instructions][8].

**Prerequisites**

*   A `genconf/config.yaml` file that is optimized for \[automatic distribution of DCOS across your nodes with SSH\]\[4\].
*   A `genconf/ip-detect` \[script\]\[5\].

To install DCOS:

1.  Download and save the DCOS setup file, `dcos_generate_config.sh`, to the `dcos` directory on your workstation. This file is used to create your customized DCOS build file.
    
    **Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

2.  From the `dcos` directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to `./genconf/serve/`.
    
        $ sudo bash dcos_generate_config.sh --genconf
        Extracking docker container from this script
        dcos-genconf.4543c7745c7e-2af26a89fa52-cb932597d7b992.tar
        Loading container into Docker daemon
        ...
        
    
    At this point your directory structure should resemble:
    
        ├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
        ├── dcos_generate_config.sh
        ├── genconf
        │   ├── config.yaml
        │   ├── ip-detect
        

3.  Run a preflight script to validate that your cluster is installable.
    
        $ sudo bash dcos_generate_config.sh --preflight 
        Running mesosphere/dcos-genconf docker BUILD_DIR set to /home/someuser/genconf 
        ... 
        Running preflight checks
        
    
    **Tip:** For a detailed view, you can add append log level debug (`-l debug`) to your command. For example `sudo bash dcos_generate_config.sh --preflight -l debug`.

4.  Install DCOS on your cluster.
    
        $ sudo bash dcos_generate_config.sh --deploy
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/someuser/genconf
        ...
        Starting DCOS install process
        Running preflight checks
        Creating directories under /etc/mesosphere
        Creating role file for master
        Configuring DCOS
        Setting and starting DCOS
        
        Cleaning up temp directory /opt/dcos_install_tmp
        
    
    **Tip:** For a detailed view, you can add append log level debug (`-l debug`) to your command. For example `sudo bash dcos_generate_config.sh --deploy -l debug`.

5.  Run the DCOS diagnostic script to verify that services are up and running.
    
        $ sudo bash dcos_generate_config.sh --postflight
        
    
    **Tip:** For a detailed view, you can add append log level debug (`-l debug`) to your command. For example `sudo bash dcos_generate_config.sh --postflight -l debug`.

6.  Monitor Exhibitor and wait for it to converge at `http://<master-ip>:8181/exhibitor/v1/ui/index.html`.
    
    **Tip:** This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a>
    
    When the status icons are green, you can access the DCOS web interface.

7.  Launch the DCOS web interface at: `http://<load-balanced-ip>/`:
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" alt="dashboardsmall" width="1338" height="828" class="alignnone size-full wp-image-1120" /></a>

You are done!

### Next Steps

Now you can [assign user roles][6].

# <a name="manual"></a>Manually distributing DCOS across your nodes

### <a name="config-json"></a>4\.1 Configure your cluster

In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.

1.  Customize this `config.yaml` template file for your environment. <!-- bootstrap_url is changeable -->
    
          bootstrap_url: http://<workstation_ip>:<your_port>       
          cluster_name: '<cluster-name>'
          exhibitor_storage_backend: zookeeper
          exhibitor_zk_hosts: <host1>:<port1>
          exhibitor_zk_path: /dcos
          master_discovery: static 
          master_list:
          - <master-ip-1>
          - <master-ip-2>
          - <master-ip-3>
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
    
    **resolvers**
    
    :   Specify a JSON-formatted list of DNS servers for your DCOS host nodes. You must include the escape characters (`\`) as shown in the template. Set this parameter to the most authoritative nameservers that you have. If you want to resolve internal hostnames, set it to a nameserver that can resolve them.
        
        *Caution:* If you set the `resolvers` parameter incorrectly, you will permanently damage your configuration and have to reinstall DCOS.
    
    For more configuration examples and all available options, see the [configuration file options][9].

2.  Save as `genconf/config.yaml`.

## <a name="install-bash"></a>4\.2 Install DCOS

In this step you create a custom DCOS build file on your workstation and then install DCOS onto your cluster. With this method you package the DCOS distribution yourself and connect to every server manually and run the commands.

**Tip:** If something goes wrong and you want to rerun your setup, use these cluster [cleanup instructions][7].

**Prerequisites**

*   A `genconf/config.yaml` file that is optimized for [manual distribution of DCOS across your nodes][8].
*   A `genconf/ip-detect` [script][10]. 
*   The Docker nginx image must be on your workstation. You can use this command to install the nginx container:
    
         docker pull nginx
        
    
    For more information, see <a href="https://hub.docker.com/_/nginx/" target="_blank">DockerHub nginx</a> documentation.

<!-- Early access URL: https://downloads.mesosphere.com/dcos/EarlyAccess/dcos_generate_config.sh -->

<!-- Stable URL: https://downloads.mesosphere.com/dcos/stable/dcos_generate_config.sh --> To install DCOS:

1.  Download and save the DCOS setup file, `dcos_generate_config.sh`, to the `dcos` directory on your workstation. This file is used to create your customized DCOS build file.
    
    **Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

2.  From the `dcos` directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to `./genconf/serve/`.
    
    At this point your directory structure should resemble:
    
        ├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
        ├── dcos_generate_config.sh
        ├── genconf
        │   ├── config.yaml
        │   ├── ip-detect
        

3.  Run this command to generate your customized DCOS build file:
    
        $ sudo bash dcos_generate_config.sh
        
    
    **Tip:** For the install script to work, you must have created [genconf/config.yaml][8] and [genconf/ip-detect][10].

4.  From the `dcos` directory, run this command to host the DCOS install package through an nginx Docker container. For `<your-port>`, specify the port value that is used in the [bootstrap_url][8].
    
        $ docker run -p <your-port>:80 -v $PWD/genconf/serve:/usr/share/nginx/html:ro nginx
        

5.  Run these commands on each of your master nodes in succession to install DCOS using your custom build file.
    
    **Tip:** Although there is no actual harm to your cluster, DCOS may issue error messages until all of your master nodes are configured.

6.  SSH to your node:
    
        $ ssh <master-ip>
        

7.  Make a new directory and navigate to it:
    
        $ mkdir /tmp/dcos && cd /tmp/dcos
        

8.  Download the DCOS installer from the nginx Docker container, where `<workstation-ip>` and `<your_port>` are specified in [bootstrap_url][8]:
    
        $ curl -O http://<workstation-ip>:<your_port>/dcos_install.sh
        

9.  Run this command to install DCOS on your master node:
    
        $ sudo bash dcos_install.sh master
        

10. Run these commands on each of your agent nodes to install DCOS using your custom build file.

11. SSH to your node:
    
        $ ssh <master-ip>
        

12. Make a new directory and navigate to it:
    
        $ mkdir /tmp/dcos && cd /tmp/dcos
        

13. Download the DCOS installer from the nginx Docker container, where `<workstation-ip>` and `<your_port>` are specified in [bootstrap_url][8]:
    
        $ curl http://<workstation-ip>:<your_port>/dcos_install.sh
        

14. Run this command to install DCOS on your agent node:
    
        $ sudo bash dcos_install.sh slave
        

15. Monitor Exhibitor and wait for it to converge at `http://<master-ip>:8181/exhibitor/v1/ui/index.html`.
    
    **Tip:** This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a>
    
    When the status icons are green, you can access the DCOS web interface.

16. Launch the DCOS web interface at: `http://<load-balanced-ip>/`:
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" alt="dashboardsmall" width="1338" height="828" class="alignnone size-full wp-image-1120" /></a>

You are done!

### Next Steps

Now you can [assign user roles][6].

 [1]: #gui
 [2]: #ssh
 [3]: #manual
 [4]: ../installing-enterprise-edition-1-6/#scrollNav-3
 [5]: ../configuration-parameters-1-6/
 [6]: ../getting-started/installing/installing-enterprise-edition/enabling-authorization/
 [7]: http://mesos.apache.org/documentation/latest/containerizer/
 [8]: ../administration/introcli/
 [9]: ../configuration-parameters-1-5/
 [10]: ../getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/