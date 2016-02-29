---
ID: 3410
post_title: Automated command line installation
author: Joel Hamill
post_date: 2016-02-17 11:23:16
post_excerpt: ""
layout: page
permalink: >
  https://docs.mesosphere.com/installing-enterprise-edition-1-6/auto-command-line/
published: true
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: true
page_options_show_link_unauthenticated: false
---
The automated command line installation method provides a guided installation of DCOS Enterprise Edition.

This installation method uses a bootstrap node to administer the DCOS installation across your cluster. The bootstrap node uses an SSH key to connect to each node in your cluster to automate the DCOS installation.

To use the automated command-line installation method:

*   Cluster nodes must be network accessible from the bootstrap node 
*   Cluster nodes must have SSH enabled and ports open from the bootstrap node
*   The bootstrap node must have an unencrypted SSH key that can be used to authenticate with the cluster nodes over SSH 

[installing-enterprise-edition-hardware] [installing-enterprise-edition-software-ssh] [installing-enterprise-edition-ip-detect]

# Step 2: Configure and Install DCOS

In this step you create a customized YAML configuration file and install DCOS across your cluster using SSH.

**Prerequisite:**

*   Encrypted SSH keys are not supported.

## <a name="config-json"></a>4\.1 Configure your cluster

In this step you create a YAML configuration file that is customized for your environment. DCOS uses this configuration file during installation to generate your cluster installation files. In these instructions we assume that you are using ZooKeeper for shared storage.

1.  Run this command to create a hashed password for superuser authentication, where `<superuser_password>` is the superuser password. Use the hashed password key for the `superuser_password_hash` parameter in your `config.yaml` file.
    
        $ sudo bash dcos_generate_config.ee.sh --hash-password <superuser_password>
        
    
    Here is an example of a hashed password output where the hashed password is: `$6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1`.
    
        Extracting image from this script and loading into docker daemon, this step can take a few minutes
        dcos-genconf.9eda4ae45de5488c0c-c40556fa73a00235f1.tar
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        00:42:10 dcos_installer.action_lib.prettyprint:: ====> HASHING PASSWORD TO SHA512
        00:42:11 root:: Hashed password for 'password' key:
        $6$rounds=656000$v55tdnlMGNoSEgYH$1JAznj58MR.Bft2wd05KviSUUfZe45nsYsjlEl84w34pp48A9U2GoKzlycm3g6MBmg4cQW9k7iY4tpZdkWy9t1   
        

2.  Create a template `config.yaml` file by entering this command:
    
        $ sudo bash dcos_generate_config.sh --validate-config
        
    
    Here is an example of the output. You can ignore the warnings.
    
        Running mesosphere/dcos-genconf docker with BUILD_DIR set to /home/centos/genconf
        23:54:54 dcos_installer.action_lib.prettyprint:: ====> VALIDATING CONFIGURATION
        23:54:54 dcos_installer.config:: Configuration file not found, /genconf/config.yaml. Writing new one with all defaults.
        23:54:54 dcos_installer.validate.onprem:: ssh_user: Please enter a valid string
        23:54:54 dcos_installer.validate.onprem:: ssh_key_path: File does not exist genconf/ssh_key
        23:54:54 dcos_installer.validate.onprem:: superuser_username: Please enter a valid string
        23:54:54 dcos_installer.validate.onprem:: agent_list: Please enter a valid IPv4 address.
        23:54:54 dcos_installer.validate.onprem:: superuser_password_hash: Please enter a valid string
        23:54:54 dcos_installer.validate.onprem:: exhibitor_zk_hosts: None is not a valid Exhibitor Zookeeper host
        23:54:54 dcos_installer.validate.onprem:: master_list: Please enter a valid IPv4 address.
        23:54:54 dcos_installer.validate.onprem:: ip_detect_script: Please provide a valid executable script. Script must start with #!/
        23:54:54 dcos_installer.validate.onprem:: ssh_key: SSH key must be an unencrypted (no passphrase) SSH key which is not empty.
        23:54:54 dcos_installer.validate.onprem:: ip_detect_path: File does not exist genconf/ip-detect
        

3.  Edit your template `genconf/config.yaml` file and customize for your environment.
    
        $ sudo vi config.yaml
        
    
    Your file should resemble this:
    
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

4.  Save as `genconf/config.yaml`.

5.  Move your private RSA key to `genconf/ssh_key`. For more information, see the [ssh_key_path][2] parameter.
    
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
        

2.  Install the cluster prerequisites, including system updates, compression utilities (UnZip, GNU tar, and XZ Utils), and cluster permissions. For a full list of cluster prerequisites, see this [documentation][3].
    
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

Now you can [assign user roles][4].

 [1]: ../configuration-parameters-1-6/
 [2]: http://mesos.apache.org/documentation/latest/containerizer/
 [3]: ../step-2-cluster-prerequisites/
 [4]: ../security-and-authentication/managing-authorization/