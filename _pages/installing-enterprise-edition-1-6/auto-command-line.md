---
ID: 3410
post_title: Automated command line installation
post_date: 2016-02-17 11:23:16
post_excerpt: ""
layout: page
permalink: >
  https://test-mesosphere-documentation.pantheon.io/installing-enterprise-edition-1-6/auto-command-line/
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: true
---
The automated command line installation method provides a guided installation of DCOS Enterprise Edition.

This installation method uses a bootstrap node to administer the DCOS installation across your cluster. The bootstrap node uses an SSH key to connect to each node in your cluster to automate the DCOS installation.

To use the automated command-line installation method:

*   Cluster nodes must be network accessible from the bootstrap node 
*   Cluster nodes must have SSH enabled and ports open from the bootstrap node
*   The bootstrap node must have an unencrypted SSH key that can be used to authenticate with the cluster nodes over SSH 

# Step 1: Bootstrap node prerequisites

Before installing DCOS, you must prepare your bootstrap node that will be used to run the DCOS installation commands. A bootstrap node is any physical, virtual, or cloud machine. It must have IP-to-IP connectivity from the bootstrap node to all nodes in your cluster environment. Your bootstrap node must not be a part of your cluster.

## Docker

Docker version 1.9 or greater must be installed on your bootstrap and cluster nodes. You must run Docker commands as the root user (`sudo`). For more information, see <a href="http://docs.docker.com/engine/installation/" target="_blank">Docker installation</a>.

Install Docker by using these commands your Linux distribution. CoreOS includes Docker natively.

*   **RHEL** Install Docker by using a subscription channel. For more information, see <a href="https://access.redhat.com/articles/881893" target="_blank">Docker Formatted Container Images on Red Hat Systems</a>. <!-- $ curl -sSL https://get.docker.com | sudo sh -->

*   **CentOS** Install Docker by using OverlayFS.
    
    1.  Add the Docker yum repo to your bootstrap node:
        
            $ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
            [dockerrepo]
            name=Docker Repository
            baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
            enabled=1
            gpgcheck=1
            gpgkey=https://yum.dockerproject.org/gpg
            EOF
            sudo yum -y update
            sudo mkdir -p /etc/systemd/system/docker.service.d
            sudo tee /etc/systemd/system/docker.service.d/override.conf <<- EOF
            [Service]
            ExecStart=
            ExecStart=/usr/bin/docker daemon --storage-driver=overlay -H fd://
            EOF
            
    
    2.  Install the Docker engine:
        
            $ sudo yum install -y docker-engine
            
    
    3.  Start the Docker daemon:
        
            $ sudo systemctl start docker
            
    
    4.  Enable the Docker service:
        
            $ sudo systemctl enable docker
            
    
    *   If you are using Docker as a non-root user, you must add your user to the "docker" group:
        
            $ sudo usermod -aG docker <user>
            
    
    *   You can test that your Docker build is properly installed with these commands:
        
            $ sudo service docker start 
            $ sudo docker ps
            

*   If you are using Docker Containerizer, you must have network access to a public Docker repository from the agent nodes or to an internal Docker registry.

## ZooKeeper for shared storage

Shared storage is required by DCOS during installation and runtime for bootstrapping and managing the internal DCOS ZooKeeper cluster. The ZooKeeper for shared storage instance should only be used for bootstrapping the DCOS Exhibitor instance. Do not use this bootstrap ZooKeeper instance in production. Multiple ZooKeeper instances are recommended for failover in production environments.

Exhibitor automatically configures your ZooKeeper installation on the master nodes during your DCOS installation. This ZooKeeper instance should be separate from your cluster. Consider using a separate directory path for the DCOS cluster so that it does not interfere with other services that use the ZooKeeper instance.

Temporary outages while the cluster is running are acceptable, but shared storage should generally be up and running to support replacing failed masters.

To start a ZooKeeper instance using Docker, run this command:

        $ sudo docker run -d -p 2181:2181 -p 2888:2888 -p 3888:3888 --name=dcos_int_zk jplock/zookeeper
    

**Tip:** If you've run the `usermod` Docker command in the previous step, you might have to log out and then back in to your boostrap node before starting Zookeeper.

## DCOS setup file

Download and save the DCOS setup file, `dcos_generate_config.sh`, to the `dcos` directory on your bootstrap node. This file is used to create your customized DCOS build file.

**Important:** Contact your sales representative or <sales@mesosphere.io> to obtain the DCOS setup file.

# Step 2: Cluster Prerequisites

Before installing DCOS you must prepare your cluster environment.

1.  **Linux distribution** A supported Linux distribution must be installed on your cluster:
    
    *   Enterprise Linux 7 kernel 3.10.0-327 or newer (RedHat or CentOS)
    *   CoreOS

2.  **Nodes Hardware Requirements**
    
    *   3 Mesos master nodes (2 Cores, 16 GB RAM, 60 GB HDD) <!-- A cluster of 8 or more machines with a supported Linux distribution. The recommended capacity for each node is 16 GB RAM/2 Cores and 16 GB disk space. The minimum size of a node is a machine with 10 GB of disk space and 1 GB of RAM. -->
    
    *   6 or more Mesos agent nodes (4 Cores, 32 GB RAM, 120 GB HDD)
    
    *   A /var directory with 10GB or more of free space. This is used by the sandbox for both Docker and [Mesos Containerizer][1].
    
    *   1 Workstation node (2 Cores, 16 GB RAM, 60 GB HDD)
        
        *   Python, pip, and virtualenv must be installed on the workstation node for the DCOS [CLI][2]. pip must be configured to pull packages from PyPI or your private PyPI, if applicable.
        *   A High-availability (HA) load balancer, such as HAProxy to balance the following TCP ports to all master nodes: 80, 443, 8080, 8181, 2181, 5050
    *   Network Time Protocol (NTP) for clock synchronization must be configured and enabled on all nodes.
    *   All of the nodes in your cluster must be able to send and receive traffic to each other.
    *   IPv6 must be disabled for all nodes. For more information see <a href="https://wiki.centos.org/FAQ/CentOS7#head-8984faf811faccca74c7bcdd74de7467f2fcd8ee" target="_blank">How do I disable IPv6</a>.

3.  **Port configuration**
    
    *   ICMP must be enabled between the master and the agent nodes.
    *   TCP and UDP enabled port 53 for DNS.
    *   Network Access to a public Docker repository from the agent nodes or to an internal Docker registry.
    *   These ports must be open for communication to the master nodes. <style type="text/css">
          .tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-e3zv{font-weight:bold}
.tg .tg-yw4l{vertical-align:top}
        </style>
        
        <table class="table">
          <tr>
            <th class="tg-e3zv">
              TCP Port
            </th>
            
            <th class="tg-e3zv">
              Description
            </th>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              2181
            </td>
            
            <td class="tg-yw4l">
              ZooKeeper, see the <a href="http://zookeeper.apache.org/doc/r3.1.2/zookeeperAdmin.html#sc_zkCommands" target="_blank">ZK Admin Guide</a>
            </td>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              2888
            </td>
            
            <td class="tg-yw4l">
              Exhibitor, see the <a href="https://github.com/Netflix/exhibitor/wiki/REST-Introduction" target="_blank">Exhibitor REST Documentation</a>
            </td>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              3888
            </td>
            
            <td class="tg-yw4l">
              Exhibitor, see the <a href="https://github.com/Netflix/exhibitor/wiki/REST-Introduction" target="_blank">Exhibitor REST Documentation</a>
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              5050
            </td>
            
            <td class="tg-031e">
              Mesos master nodes
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              5051
            </td>
            
            <td class="tg-031e">
              Mesos agent nodes
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              8080
            </td>
            
            <td class="tg-031e">
              Marathon
            </td>
          </tr>
          
          <tr>
            <td class="tg-031e">
              8123
            </td>
            
            <td class="tg-031e">
              Mesos-DNS API
            </td>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              8181
            </td>
            
            <td class="tg-yw4l">
              Exhibitor, see the <a href="https://github.com/Netflix/exhibitor/wiki/REST-Introduction" target="_blank">Exhibitor REST Documentation</a>
            </td>
          </tr>
        </table>
    
    *   These ports must be open for communication to the agent nodes from the master nodes: <style type="text/css">
          .tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-e3zv{font-weight:bold}
.tg .tg-yw4l{vertical-align:top}
        </style>
        
        <table class="table">
          <tr>
            <th class="tg-e3zv">
              TCP Port
            </th>
            
            <th class="tg-e3zv">
              Description
            </th>
          </tr>
          
          <tr>
            <td class="tg-yw4l">
              5051
            </td>
            
            <td class="tg-yw4l">
              Mesos agent nodes
            </td>
          </tr>
        </table>
    
    *   All DCOS cluster node hostnames (FQDN and short hostnames) must be resolvable in DNS, both forward and reverse lookups must succeed.

# Step 3: Create a Script for IP Address Discovery

In this step you create an IP detect script to broadcast the IP address of each node across the cluster. Each node in a DCOS cluster has a unique IP address that is used to communicate between nodes in the cluster. The IP detect script prints the unique IPv4 address of a node to STDOUT each time DCOS is started on the node.

**Important:** The IP address of a node must not change after DCOS is installed on the node. For example, the IP address must not change when a node is rebooted or if the DHCP lease is renewed. If the IP address of a node does change, the node must be [wiped and reinstalled][1].

1.  Create a directory named `genconf` on your bootstrap node and navigate to it.
    
        $ mkdir -p genconf && cd genconf
        

2.  Create an IP detection script for your environment and save as `genconf/ip-detect`. You can use the examples below.
    
    *   #### Use the AWS Metadata Server
        
        This method uses the AWS Metadata service to get the IP address:
        
            #!/bin/sh
            # Example ip-detect script using an external authority
            # Uses the AWS Metadata Service to get the node's internal
            # ipv4 address
            curl -fsSL http://169.254.169.254/latest/meta-data/local-ipv4
            
    
    *   #### Use the GCE Metadata Server
        
        This method uses the GCE Metadata Server to get the IP address:
        
            #!/bin/sh
            # Example ip-detect script using an external authority
            # Uses the GCE metadata server to get the node's internal
            # ipv4 address
            
            curl -fsSl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/network-interfaces/0/ip
            
    
    *   #### Use the IP address of an existing interface
        
        This method discovers the IP address of a particular interface of the node.
        
        If you have multiple generations of hardware with different internals, the interface names can change between hosts. The IP detection script must account for the interface name changes. The example script could also be confused if you attach multiple IP addresses to a single interface, or do complex Linux networking, etc.
        
            #!/usr/bin/env bash
            set -o nounset -o errexit
            export PATH=/usr/sbin:/usr/bin:$PATH
            echo $(ip addr show eth0 | grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | head -1)
            
    
    *   #### Use the network route to the Mesos master
        
        This method uses the route to a Mesos master to find the source IP address to then communicate with that node.
        
        In this example, we assume that the Mesos master has an IP address of `172.28.128.3`. You can use any language for this script. Your Shebang line must be pointed at the correct environment for the language used and the output must be the correct IP address.
        
            #!/usr/bin/env bash
            set -o nounset -o errexit
            
            MASTER_IP=172.28.128.3
            
            echo $(/usr/sbin/ip route show to match 172.28.128.3 | grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | tail -1)
            

# Step 4: Configure and Install DCOS

In this step you create a customized YAML configuration file and install DCOS across your cluster using SSH.

**Prerequisite:**

*   Encrypted SSH keys are not supported.

## <a name="config-json"></a>4\.1 Configure your cluster

In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.

1.  Run this command to create a hashed password for superuser authentication, where `<superuser_password>` is the superuser password. Use the hashed password key for the `superuser_password_hash` parameter in your `config.yaml` file.
    
        $ sudo bash dcos_generate_config.ee.sh --hash-password <superuser_password>
        Extracting image from this script and loading into docker daemon, this step can take a few minutes
        dcos-genconf.9eda4ae45de5488c0c-c40556fa73a00235f1.tar
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        00:42:10 dcos_installer.action_lib.prettyprint:: ====> HASHING PASSWORD TO SHA512
        00:42:11 root:: Hashed password for 'password' key:
        $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1   
        

2.  Customize this `config.yaml` template file for your environment.
    
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
          superuser_username: <username>
          superuser_password_hash: <hashed-password>
          agent_list:
          - <target-host-1>
          - <target-host-2>
          - <target-host-3>
          - <target-host-4>
          - <target-host-5>
        
    
    **Tip:** You can create a template `genconf/config.yaml` file by entering this command:
    
        $ sudo bash dcos_generate_config.ee.sh --validate-config
        
    
    Specify these configuration parameters. <!-- log_directory: /genconf/logs, process_timeout: 120, ssh_key_path: /genconf/ssh-key -->
    
    **bootstrap_url**
    
    :   This parameter specifies the URI path for the DCOS installer to store the customized DCOS build files. Use the default value of `bootstrap_url: file:///opt/dcos_install_tmp`, unless you have moved the installer assets from their default location.
    
    **cluster_name**
    :   Specify the name of your cluster.
    
    **exhibitor_storage_backend**
    :   This parameter specifies the type of storage backend for Exhibitor. By default this is set to `zookeeper` in the `config.yaml` template file. During DCOS installation, a storage system is required for configuring and orchestrating Zookeeper with Exhibitor on the master nodes. Exhibitor automatically configures your Zookeeper installation on the master nodes during your DCOS installation.
    
    **exhibitor_zk_hosts**
    :   Specify a comma-separated list of one or more ZooKeeper node IP addresses to use for configuring the internal Exhibitor instances. Exhibitor uses this ZooKeeper cluster to orchestrate it's configuration. Multiple ZooKeeper instances are recommended for failover in production environments.
    
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
    
    **superuser_password_hash**
    
    :   This parameter specifies the hashed Admin password. This password is required for using DCOS. For example:
        
            superuser_password_hash: $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1 
            
    
    **superuser_username**
    :   This parameter specifies the Admin username. This username is required for using DCOS.
    
    **agent_list**
    :   This parameter specifies a complete list of IPv4 addresses to your agent host names. This must be a YAML-formatted nested series (-).
    
    For more configuration examples and all available options, see the [configuration file options][1].

3.  Save as `genconf/config.yaml`.

4.  Move your private RSA key to `genconf/ssh_key`. For more information, see the [ssh_key_path][2] parameter.
    
        $ cp <path-to-key> genconf/ssh_key && chmod 0600 genconf/ssh_key
        

## <a name="install-bash"></a>4\.2 Install DCOS

In this step you create a custom DCOS build file on your bootstrap node and then install DCOS across your cluster nodes with SSH. With this installation method you create a bootstrap server that uses your SSH key and connects to every node to automate the deployment.

You can view all of the automated command line installer options with the `--help` flag:

    $ sudo bash dcos_generate_config.ee.sh --help
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
    

**Tip:** If something goes wrong and you want to rerun your setup, use these cluster <a href="https://docs.mesosphere.com/getting-started/installing/installing-enterprise-edition/dcos-cleanup-script/" target="_blank">cleanup instructions</a>.

To install DCOS:

1.  From the `dcos` directory, run the DCOS installer shell script on your bootstrapping master nodes to generate a customized DCOS build. The setup script extracts a Docker container that uses the generic DCOS install files to create customized DCOS build files for your cluster. The build files are output to `./genconf/serve/`.
    
        $ sudo bash dcos_generate_config.ee.sh --genconf
        Extracking docker container from this script
        dcos-genconf.4543c7745c7e-2af26a89fa52-cb932597d7b992.tar
        Loading container into Docker daemon
        ...
        
    
    At this point your directory structure should resemble:
    
        ├── dcos-genconf.c9722490f11019b692-cb6b6ea66f696912b0.tar
        ├── dcos_generate_config.ee.sh
        ├── genconf
        │   ├── config.yaml
        │   ├── ip-detect     
        

2.  Install the cluster prerequisites, including system updates, compression utilities (UnZip, GNU tar, and XZ Utils), and cluster permissions.
    
        $ sudo bash dcos_generate_config.ee.sh --install-prereqs
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        20:47:49 dcos_installer.action_lib.prettyprint:: ====> EXECUTING INSTALL PREREQUISITES
        20:47:49 dcos_installer.action_lib.prettyprint:: ====> START install_prereqs
        20:52:32 dcos_installer.action_lib.prettyprint:: ====> STAGE install_prereqs
        20:52:55 dcos_installer.action_lib.prettyprint:: ====> STAGE install_prereqs
        20:52:55 dcos_installer.action_lib.prettyprint:: ====> END install_prereqs with returncode: 0
        20:52:55 dcos_installer.action_lib.prettyprint:: ====> SUMMARY
        20:52:55 dcos_installer.action_lib.prettyprint:: 2 out of 2 hosts successfully completed install_prereqs stage.
        

3.  Run a preflight script to validate that your cluster is installable.
    
        $ sudo bash dcos_generate_config.ee.sh --preflight
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        20:54:02 dcos_installer.action_lib.prettyprint:: ====> EXECUTING PREFLIGHT
        20:54:02 dcos_installer.action_lib.prettyprint:: ====> START run_preflight
        20:54:03 dcos_installer.action_lib.prettyprint:: ====> STAGE preflight
        20:54:03 dcos_installer.action_lib.prettyprint:: ====> STAGE preflight
        20:54:03 dcos_installer.action_lib.prettyprint:: ====> STAGE preflight_cleanup
        20:54:03 dcos_installer.action_lib.prettyprint:: ====> STAGE preflight_cleanup
        20:54:03 dcos_installer.action_lib.prettyprint:: ====> END run_preflight with returncode: 0
        20:54:03 dcos_installer.action_lib.prettyprint:: ====> SUMMARY
        20:54:03 dcos_installer.action_lib.prettyprint:: 2 out of 2 hosts successfully completed run_preflight stage.
        
    
    **Tip:** For a detailed view, you can append log level debug (`-l debug`) to your command. For example `sudo bash dcos_generate_config.ee.sh --preflight -l debug`.

4.  Install DCOS on your cluster.
    
        $ sudo bash dcos_generate_config.ee.sh --deploy
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        20:55:00 dcos_installer.action_lib.prettyprint:: ====> EXECUTING DCOS INSTALLATION
        20:55:00 dcos_installer.action_lib.prettyprint:: ====> START deploy_master
        20:57:04 dcos_installer.action_lib.prettyprint:: ====> STAGE deploy_master
        20:57:04 dcos_installer.action_lib.prettyprint:: ====> STAGE deploy_master_cleanup
        20:57:04 dcos_installer.action_lib.prettyprint:: ====> END deploy_master with returncode: 0
        20:57:04 dcos_installer.action_lib.prettyprint:: ====> SUMMARY
        20:57:04 dcos_installer.action_lib.prettyprint:: 1 out of 1 hosts successfully completed deploy_master stage.
        20:57:04 dcos_installer.action_lib.prettyprint:: ====> START deploy_agent
        20:59:19 dcos_installer.action_lib.prettyprint:: ====> STAGE deploy_agent
        20:59:19 dcos_installer.action_lib.prettyprint:: ====> STAGE deploy_agent_cleanup
        20:59:19 dcos_installer.action_lib.prettyprint:: ====> END deploy_agent with returncode: 0
        20:59:19 dcos_installer.action_lib.prettyprint:: ====> SUMMARY
        20:59:19 dcos_installer.action_lib.prettyprint:: 1 out of 1 hosts successfully completed deploy_agent stage.
        
    
    **Tip:** For a detailed view, you can append log level debug (`-l debug`) to your command. For example `sudo bash dcos_generate_config.ee.sh --deploy -l debug`.

5.  Run the DCOS diagnostic script to verify that services are up and running.
    
        $ sudo bash dcos_generate_config.ee.sh --postflight
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        21:22:44 dcos_installer.action_lib.prettyprint:: ====> EXECUTING POSTFLIGHT
        21:22:44 dcos_installer.action_lib.prettyprint:: ====> START run_postflight
        21:22:45 dcos_installer.action_lib.prettyprint:: ====> STAGE postflight
        21:22:45 dcos_installer.action_lib.prettyprint:: ====> STAGE postflight
        21:22:45 dcos_installer.action_lib.prettyprint:: ====> STAGE postflight_cleanup
        21:22:45 dcos_installer.action_lib.prettyprint:: ====> STAGE postflight_cleanup
        21:22:45 dcos_installer.action_lib.prettyprint:: ====> END run_postflight with returncode: 0
        21:22:45 dcos_installer.action_lib.prettyprint:: ====> SUMMARY
        21:22:45 dcos_installer.action_lib.prettyprint:: 2 out of 2 hosts successfully completed run_postflight stage.
        
    
    **Tip:** For a detailed view, you can append log level debug (`-l debug`) to your command. For example `sudo bash dcos_generate_config.ee.sh --postflight -l debug`.

6.  Monitor Exhibitor and wait for it to converge at `http://<master-ip>:8181/exhibitor/v1/ui/index.html`.
    
    **Tip:** This process can take about 10 minutes. During this time you will see the Master nodes become visible on the Exhibitor consoles and come online, eventually showing a green light.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" rel="attachment wp-att-2112"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/chef-zk-status.png" alt="chef-zk-status" width="551" height="467" class="alignnone size-full wp-image-2112" /></a>
    
    When the status icons are green, you can access the DCOS web interface.

7.  Launch the DCOS web interface at: `http://<load-balanced-ip>/`.

8.  Click **Log In To DCOS**.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" rel="attachment wp-att-3198"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-success1.png" alt="ui-installer-success1" width="625" height="404" class="alignnone size-full wp-image-3198" /></a>

9.  Enter your administrator username and password.
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2.png" rel="attachment wp-att-3341"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-installer-auth2-800x513.png" alt="ui-installer-auth2" width="800" height="513" class="alignnone size-large wp-image-3341" /></a>
    
    You are done!
    
    <a href="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee.png" rel="attachment wp-att-3343"><img src="https://docs.mesosphere.com/wp-content/uploads/2016/02/ui-dashboard-ee-800x538.png" alt="ui-dashboard-ee" width="800" height="538" class="alignnone size-large wp-image-3343" /></a>

### Next Steps

Now you can [assign user roles][3].

 [1]: ../configuration-parameters-1-6/
 [2]: http://mesos.apache.org/documentation/latest/containerizer/
 [3]: ../security-and-authentication/managing-authorization/