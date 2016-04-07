---
UID: 56fdda285d588
post_title: 'Web UI: Nodes'
post_excerpt: ""
layout: page
published: true
menu_order: 3
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The Nodes page provides a comprehensive view of all of the nodes that are used across your cluster. You can view a graph that shows the allocation percentage rate for CPU, memory, or disk.

<a href="/wp-content/uploads/2015/12/ui-interface-nodes.gif" rel="attachment wp-att-4124"><img src="/wp-content/uploads/2015/12/ui-interface-nodes-800x429.gif" alt="ui-interface-nodes" width="800" height="429" class="alignnone size-large wp-image-4124" /></a>

By default all of your nodes are displayed in **List** view, sorted by hostname. You can filter nodes by service type or hostname. You can also sort the nodes by number of tasks or percentage of CPU, memory, or disk space allocated.

You can switch to **Grid** view to see a "donuts" percentage visualization.

<a href="/wp-content/uploads/2015/12/nodedonutsonly.png" rel="attachment wp-att-1129"><img src="/wp-content/uploads/2015/12/nodedonutsonly-600x163.png" alt="nodedonutsonly" width="300" height="82" class="alignnone size-medium wp-image-1129" /></a>

Clicking on a node opens the Nodes side panel, which provides CPU, memory, and disk usage graphs and lists all tasks on the node. Use the dropdown or a custom filter to sort tasks and click on details for more information. Click on a task listed on the Nodes side panel to see detailed information about the task’s CPU, memory, and disk usage and the task’s files and directory tree.