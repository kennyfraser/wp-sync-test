---
UID: 56df3defb016f
post_title: Uninstalling on Amazon Web Services
post_excerpt: ""
layout: page
published: true
menu_order: 2
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
<p>You can uninstall DCOS Community Edition from AWS with these instructions.</p>

<p><strong>You will continue to be charged AWS fees if:</strong></p>

<ul>
<li>You delete only the individual EC2 instances, not the entire stack. If you delete only the individual instances, AWS will restart your DCOS cluster.</li>
<li>Your stack fails to delete. You must monitor the stack deletion process to ensure it completes successfully. For more information see this <a href="https://support.mesosphere.com/hc/en-us/articles/204623889-Why-is-AWS-failing-to-delete-my-cluster-" target="_blank">DCOS Knowledge Base</a> article. </li>
<li>Your S3 bucket is not empty.</li>
</ul>

<p>To uninstall DCOS on AWS:</p>

<ol>
<li><p>Select your cluster on the <a href="https://console.aws.amazon.com/cloudformation/home" target="_blank">Cloud Formation Management</a> page and click <strong>Delete Stack</strong>. For more information, see <a href="http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html" target="_blank">Deleting a Stack on the AWS CloudFormation Console</a>.</p></li>
<li><p>Navigate to the <a href="https://console.aws.amazon.com/s3/home" target="_blank">S3 Management Console</a> and delete your DCOS buckets. For more information, see <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/delete-or-empty-bucket.html" target="_blank">Deleting or Emptying a Bucket</a>.</p></li>
</ol>