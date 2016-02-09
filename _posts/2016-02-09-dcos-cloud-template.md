---
ID: 3007
post_title: DCOS Cloud Template
author:
  - Joel Hamill
post_date:
  - 2016-02-09 07:22:10
post_excerpt:
  - ""
layout: post
permalink:
  - >
    http://local.mesodocs.com/2016/02/09/dcos-cloud-template/
published: true
---
Test 6 - The DCOS Community Edition (CE) cloud templates are optimized to run Mesosphere DCOS. The templates are a JSON-formatted text file that describes the resources and properties.

AWS CloudFormation template
:   - 1 or 3 Mesos master nodes in the [admin][1] zone - 5 Mesos agent nodes in the [private][2] zone - 1 Mesos agent node in the [public][3] zone - Amazon EC2 <a href="https://aws.amazon.com/ec2/pricing/" target="_blank">m3.xlarge</a> instance

Depending on the DCOS services that you install, you might have to modify the DCOS templates to suit your needs. For more information, see [Scaling the DCOS cluster in AWS][4].

**Important:** You are allowed to modify the DCOS CE cloud template, but Mesosphere cannot assist in troubleshooting. If you require support, please consider the DCOS Enterprise Edition. See the [Cost & Limitations][5] topic for more information on DCOS CE and DCOS EE.

{% include costdisclaimer.md %}

 [1]: /overview/security#admin
 [2]: /overview/security#private
 [3]: /overview/security#public
 [4]: /install/templatescale/
 [5]: /overview/limitations/
