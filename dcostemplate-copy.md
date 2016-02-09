---
layout: post
post_title: DCOS Cloud Template
published: true
---

The DCOS Community Edition (CE) cloud templates are optimized to run Mesosphere DCOS. The templates are a JSON-formatted text file that describes the resources and properties. 

AWS CloudFormation template 
: - 1 or 3 Mesos master nodes in the [admin](/overview/security#admin) zone
- 5 Mesos agent nodes in the [private](/overview/security#private) zone
- 1 Mesos agent node in the [public](/overview/security#public) zone
 - Amazon EC2 <a href="https://aws.amazon.com/ec2/pricing/" target="_blank">m3.xlarge</a> instance

Depending on the DCOS services that you install, you might have to modify the DCOS templates to suit your needs. For more information, see [Scaling the DCOS cluster in AWS](/install/templatescale/).

**Important:** You are allowed to modify the DCOS CE cloud template, but Mesosphere cannot assist in troubleshooting. If you require support, please consider the DCOS Enterprise Edition. See the [Cost & Limitations](/overview/limitations/) topic for more information on DCOS CE and DCOS EE.

{% include costdisclaimer.md %}
