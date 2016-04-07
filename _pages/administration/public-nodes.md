---
UID: 56f98449df041
post_title: Public Nodes
post_excerpt: ""
layout: page
published: true
menu_order: 12
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
One way to secure your cluster is to use the concept of a DMZ or [demilitarized zone][1]. With this network configuration, some nodes are private and some nodes are public (accessible from the internet). These networking constraints are usually controlled by firewalls or infrastructure level networking (beyond the scope of this document).

If your DCOS cluster was installed with one of the cloud provisioning templates (e.g. AWS CloudFormation), you probably already have public and private nodes with pre-configured network zones. If not, you can configure DCOS on installation to create both public and private nodes.

Usually, these network zones are represented with [Mesos roles][2]. Public nodes are configured with a specific role (`role=slave_public`), while all other nodes are private nodes (`role=*`).

Marathon apps may then be configured to run on a public node by including `slave_public` in the [acceptedResourceRoles][3] list.

*   To run on only public nodes:
    
        "acceptedResourceRoles":["slave_public"]
        

*   To run on any node type:
    
        "acceptedResourceRoles":["slave_public", "*"]
        

*   To run on only private nodes (default behavior):
    
        "acceptedResourceRoles":["*"]
        

For a more comprehensive example of deploying an app in the public zone to route HTTP requests, see [Deploying a Containerized App on a Public Node][4].

 [1]: https://en.wikipedia.org/wiki/DMZ_(computing)
 [2]: http://mesos.apache.org/documentation/latest/roles/
 [3]: https://mesosphere.github.io/marathon/docs/rest-api.html#acceptedresourceroles-v0-9-0
 [4]: /usage/tutorials/deploy-containerized-app/