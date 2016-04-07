---
UID: 56fdda2831397
post_title: 'Web UI: Dashboard'
post_excerpt: ""
layout: page
published: true
menu_order: 1
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The dashboard is the home page of the DCOS web interface and provides an overview of your DCOS cluster.

<a href="/wp-content/uploads/2015/12/ui-dashboard.gif" rel="attachment wp-att-4122"><img src="/wp-content/uploads/2015/12/ui-dashboard-800x431.gif" alt="ui-dashboard" width="800" height="431" class="alignnone size-large wp-image-4122" /></a>

From the dashboard you can easily monitor the health of your cluster.

*   The CPU Allocation panel provides a graph of the current percentage of available general compute units that are being used by your cluster.

*   The Memory Allocation panel provides a graph of the current percentage of available memory that is being used by your cluster.

*   The Task Failure Rate panel provides a graph of the current percentage of tasks that are failing in your cluster.

*   The Services Health panel provides an overview of the health of your services. Each service provides a healthcheck, run at intervals. This indicator shows the current status according to that healthcheck. A maximum of 5 services are displayed, sorted by priority of the most unhealthy. You can click the View all Services button for detailed information and a complete list of your services.

*   The Tasks panel provides the current number of tasks that are staged and running.

*   The Nodes panel provides a view of the nodes in your cluster.