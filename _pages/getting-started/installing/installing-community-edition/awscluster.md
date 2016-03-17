---
UID: 56df3defcb7cd
post_title: Installing on Amazon Web Services
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can create a DCOS cluster for Amazon Web Services (AWS) by using the Mesosphere DCOS template on AWS CloudFormation.</p>

<p><strong>Important:</strong> The DCOS Community Edition (CE) is available at no cost, but must be run on a supported cloud provider (for example, Amazon Web Services). While there are no licensing fees for DCOS CE, your cloud provider charges for their service. For an estimate of the cost of running DCOS CE on AWS, see <a href="https://support.mesosphere.com/hc/en-us/articles/205314895-How-much-does-running-a-default-DCOS-cluster-configuration-cost-" target="_blank">this FAQ entry</a>.</p>

<p>The DCOS cloud templates are optimized to run Mesosphere DCOS. Depending on the DCOS services that you install, you might have to modify the DCOS templates to suit your needs. You can modify the cloud templates, but Mesosphere cannot assist in troubleshooting. If you require support, please consider the DCOS Enterprise Edition. For more information, see <a href="../administration/managing-a-dcos-cluster-in-aws/#scrollNav-1">Scaling the DCOS cluster in AWS</a>.</p>

<h3>AWS CloudFormation template</h3>

<ul>
<li>1 or 3 Mesos master nodes in the <a href="../administration/dcosarchitecture/security/#scrollNav-1">admin</a> zone</li>
<li>5 Mesos agent nodes in the <a href="../administration/dcosarchitecture/security/#scrollNav-2">private</a> zone</li>
<li>1 Mesos agent node in the <a href="https://docs.mesosphere.com/administration/dcosarchitecture/security/#scrollNav-3">public</a> zone</li>
<li>Amazon EC2 <a href="https://aws.amazon.com/ec2/pricing/" target="_blank">m3.xlarge</a> instance</li>
</ul>

<h1>Create a DCOS cluster</h1>

<p><strong>Prerequisite:</strong></p>

<p><a href="http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair" target="_blank">Amazon EC2 Key Pair</a></p>

<ol>
<li><p>Launch the <a href="http://mesosphere.com/amazon/setup" target="_blank">DCOS template</a> on CloudFormation and select the region and number of masters. You must have a key pair for your selected region.</p>

<!-- a href="http://mesosphere.com/amazon/setup" target="_blank"&gt;DCOS template</a>: The current stable release. -->

&lt;!-- - <a href="https://downloads.mesosphere.com/dcos/EarlyAccess/aws.html">DCOS 1.3 early access template</a> --&gt;

<p><strong>Important:</strong> The Mesosphere template is configured for running DCOS. If you modify the template you might be unable to run certain packages on your DCOS cluster. For more information, see the <a href="https://support.mesosphere.com/hc/en-us/articles/205674655-How-can-I-modify-the-DCOS-template-on-AWS-CloudFormation-" target="_blank">Knowledge Base</a>.</p></li>
<li><p>On the <strong>Select Template</strong> page, accept the template specified for you and click <strong>Next</strong>.</p></li>
<li><p>On the <strong>Specify Details</strong> page, specify a cluster name, SSH key, accept the <a href="../community-edition-eula/" target="_blank">EULA</a>, and click <strong>Next</strong>. The other parameters are optional.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/awstemplateparms.png" rel="attachment wp-att-1218"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/awstemplateparms.png" alt="awstemplateparms" width="551" height="326" class="alignnone size-full wp-image-1218" /></a></p></li>
<li><p>On the <strong>Options</strong> page, accept the defaults and click <strong>Next</strong>.</p>

<p><strong>Tip:</strong> You can choose whether to rollback on failure. By default this option is set to <strong>Yes</strong>.</p></li>
<li><p>On the <strong>Review</strong> page, check the acknowledgement box and then click <strong>Create</strong>.</p>

<p><strong>Tip:</strong> If the <strong>Create New Stack</strong> page is shown, either AWS is still processing your request or you’re looking at a different region. Navigate to the correct region and refresh the page to see your stack.</p></li>
</ol>

<h1>Monitor the DCOS cluster convergence process</h1>

<p>In <a href="https://console.aws.amazon.com/cloudformation/home" target="_blank">CloudFormation</a> you should see:</p>

<ul>
<li><p>The cluster stack spins up over a period of 10 to 15 minutes.</p></li>
<li><p>The status changes from CREATE_IN_PROGRESS to CREATE_COMPLETE.</p></li>
</ul>

<p><strong>Troubleshooting:</strong> A ROLLBACK_COMPLETE status means the deployment has failed. See the <strong>Events</strong> tab for useful information about failures. For more information, see the <a href="https://support.mesosphere.com/hc/en-us/articles/205316535-Why-did-my-AWS-cluster-Rollback-" target="_blank">Mesosphere Knowledge Base</a>.</p>

<h1><a name="launchdcos"></a>Launch DCOS</h1>

<p>Launch the DCOS web interface by entering the Mesos Master hostname:</p>

<ol>
<li><p>From the <a href="https://console.aws.amazon.com/cloudformation/home" target="_blank">Amazon CloudFormation Management</a> page, click to check the box next to your stack.</p></li>
<li><p>Click on the <strong>Outputs</strong> tab and copy/paste the Mesos Master hostname into your browser to open the DCOS web interface. The interface runs on the standard HTTP port 80, so you do not need to specify a port number after the hostname.</p>

<p><strong>Tip:</strong> You might need to resize your window to see this tab. You can find your DCOS hostname any time from the <a href="https://console.aws.amazon.com/cloudformation/home" target="_blank">Amazon CloudFormation Management</a> page.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/awsscreenshot2.png" rel="attachment wp-att-1167"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/awsscreenshot2.png" alt="awsscreenshot2" width="621" height="146" class="alignnone size-full wp-image-1167" /></a></p></li>
<li><p><a href="https://docs.mesosphere.com/administration/introcli/cli/">Install the DCOS Command-Line Interface (CLI)</a>. You must install the CLI to administer your DCOS cluster.</p>

<p>You also have the option to take a brief tutorial that walks you through the basics of using the Mesosphere DCOS. You can restart this tutorial anytime by clicking the signpost icon in the lower left corner.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall.png" rel="attachment wp-att-1120"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/dashboardsmall-800x495.png" alt="dashboardsmall" width="800" height="495" class="alignnone size-large wp-image-1120" /></a></p></li>
</ol>

<h1>Next steps</h1>

<ul>
<li>Try out the <a href="../getting-started/tutorials/deploy-containerized-app/">Deploying a Containerized App on a Public Node</a> tutorial.</li>
</ul>