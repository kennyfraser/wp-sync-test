---
UID: 56df3ded86c7e
post_title: Managing DCOS Clusters in AWS
post_excerpt: ""
layout: page
published: true
menu_order: 12
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<h1>Finding your public node hostname</h1>

<p>The DCOS AWS CloudFormation template creates 1 Mesos agent node in the <a href="../administration/dcosarchitecture/security/#scrollNav-3">public zone</a>.</p>

<p>To find your your public node IP:</p>

<ol>
<li><p>Select your stack from the <a href="https://console.aws.amazon.com/cloudformation/home" target="_blank">Amazon CloudFormation Management</a> page.</p></li>
<li><p>Click on the <strong>Outputs</strong> tab and copy/paste the Public slaves hostname into your browser to open the Oinker web interface.</p>

<p><a href="https://docs.mesosphere.com/wp-content/uploads/2015/12/awsec2privatedns.png" rel="attachment wp-att-1496"><img src="https://docs.mesosphere.com/wp-content/uploads/2015/12/awsec2privatedns-800x197.png" alt="awsec2privatedns" width="800" height="197" class="alignnone size-large wp-image-1496" /></a></p>

<p><strong>Tip:</strong> You might have to refresh your browser to see your deployed app.</p></li>
</ol>

<h1>Scaling an AWS cluster</h1>

<p>The DCOS AWS CloudFormation template is optimized to run the DCOS, but you might want to change the number of agent nodes based on your needs.</p>

<p><strong>Important:</strong> Scaling down your AWS cluster could result in data loss. It is recommended that you scale down by 1 node at a time, letting the DCOS service recover. For example, if you are running a DCOS service and you scale down from 10 to 5 nodes, this could result in losing all Mesos name nodes or journals at once, taking your entire cluster down.</p>

<p>To change the number of agent nodes with AWS:</p>

<ol>
<li>From <a href="https://console.aws.amazon.com/cloudformation/home" target="blank">AWS CloudFormation Management</a> page, select your DCOS cluster and click <strong>Update Stack</strong>.</li>
<li>Click through to the <strong>Specify Parameters</strong> page, and you can specify new values for the <strong>PublicSlaveInstanceCount</strong> and <strong>SlaveInstanceCount</strong>.</li>
<li>On the <strong>Options</strong> page, accept the defaults and click <strong>Next</strong>. <strong>Tip:</strong> You can choose whether to rollback on failure. By default this option is set to <strong>Yes</strong>.</li>
<li>On the <strong>Review</strong> page, check the acknowledgement box and then click <strong>Create</strong>.</li>
</ol>

<p>Your new machines will take a few minutes to initialize; you can watch them in the EC2 console. The DCOS web interface will update as soon as the new nodes register.</p>

<h1>Upgrading a DCOS cluster in AWS</h1>

<p>You can update an existing DCOS Community Edition (CE) cluster or services to use the latest DCOS template.</p>

<p>To upgrade a DCOS cluster:</p>

<ol>
<li><p>Create a new DCOS cluster by using the latest <a href="/installing/installing-community-edition/awscluster/">DCOS template</a> for AWS.</p></li>
<li><p>Migrate your active DCOS services and apps to the new DCOS cluster:</p>

<ol>
<li><p>Migrate, Extract, Transform and Load (ETL) the app data to the new cluster.</p></li>
<li><p>Migrate your DCOS services and apps to the new cluster.</p></li>
<li><p>Change the DNS so that it points to the DCOS services running in the new cluster.</p></li>
</ol></li>
<li><p>Shutdown your existing DCOS cluster.</p></li>
</ol>