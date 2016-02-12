---
ID: 518
post_title: Uninstalling on Amazon Web Services
author: Joel Hamill
post_date: 2015-12-08 08:56:30
post_excerpt: ""
layout: page
permalink: >
  http://local.mesodocs.com/getting-started/installing/installing-community-edition/removeaws/
published: true
import_src:
  - 'a:1:{i:0;s:36:"mesosphere-docs/install/removeaws.md";}'
page_options_topic_page:
  - 'a:1:{i:0;s:0:"";}'
page_options_require_authentication: false
hide_from_navigation: false
hide_from_related: false
page_options_show_link_unauthenticated: false
---
You can uninstall DCOS Community Edition from AWS with these instructions.

1.  Select your cluster on the <a href="https://console.aws.amazon.com/cloudformation/home" target="_blank">Cloud Formation Management</a> page and click **Delete Stack**. For more information, see <a href="http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html" target="_blank">Deleting a Stack on the AWS CloudFormation Console</a>.
    
    **Warning:** You must delete your entire stack, not the individual EC2 instances. If you delete only the individual instances, AWS will restart your DCOS cluster and you will continue to be charged standard AWS fees.
    
    **Warning:** Under certain conditions, stack deletion may fail. Watch the stack deletion process to ensure it completes successfully. If the stacks are not deleted, your AWS cluster will continue to run and you will continue to be charged standard AWS fees.

2.  Navigate to the <a href="https://console.aws.amazon.com/s3/home" target="_blank">S3 Management Console</a> and delete your DCOS buckets. For more information, see <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/delete-or-empty-bucket.html" target="_blank">Deleting or Emptying a Bucket</a>.
    
    **Warning:** If your S3 bucket is not empty, AWS will continue to charge standard S3 storage fees.

If your cluster fails to delete, see the <a href="https://support.mesosphere.com/hc/en-us/articles/204623889-Why-is-AWS-failing-to-delete-my-cluster-" target="_blank">Mesosphere Knowledge Base</a>.