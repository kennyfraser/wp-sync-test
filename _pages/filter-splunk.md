---
ID: 526
post_title: Filtering Task Logs with Splunk
post_date: 2015-12-08 08:56:41
post_excerpt: ""
layout: page
permalink: https://gitsync.mmdev2.ca/filter-splunk/
published: true
menu_order: 6
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The file system paths of DCOS task logs contain information such as the agent ID, framework ID, and executor ID. You can use this information to filter the log output for specific tasks, applications, or agents.

**Prerequisite**

*   [A Splunk installation that aggregates DCOS logs][1]

# <a name="configuration"></a>Configuration

You can configure Splunk either by using the Splunk [Web UI][2] or by editing the [props.conf file][3].

#### <a name="splunkui"></a>Splunk Web UI

1.  Navigate to Settings > Fields > Field Extractions > New.
2.  Fill out the form with the following:
    
    *   Destination app: `search`
    *   Name: `dcos_task` (or any unique, meaningful name for the extraction)
    *   Apply to `source` named `/var/lib/mesos/slave/...`
    *   Type: `Inline`
    *   Extraction/Transform:
        
            /var/lib/mesos/slave/slaves/(?<agent>[^/]+)/frameworks/(?<framework>[^/]+)/executors/(?<executor>[^/]+)/runs/(?<run>[^/]+)/.* in source
            

3.  Click "Save".

4.  In the "Field Extractions" view, find the extraction you just created and set the permissions appropriately.

The `agent`, `framework`, `executor`, and `run` fields should now be available to use in search queries and appear in the fields associated with Mesos task log events.

#### <a name="propsconf"></a>props.conf

1.  Add the following entry to `props.conf` (see the [Splunk documentation][4] for details):
    
        [source::/var/lib/mesos/slave/...]
        EXTRACT = /var/lib/mesos/slave/slaves/(?<agent>[^/]+)/frameworks/(?<framework>[^/]+)/executors/(?<executor>[^/]+)/runs/(?<run>[^/]+)/.* in source
        

2.  Run the following search in the Splunk Web UI to ensure the changes take effect:
    
        extract reload=true
        

The `agent`, `framework`, `executor`, and `run` fields should now be available to use in search queries and appear in the fields associated with Mesos task log events.

# <a name="usage"></a>Usage Example

For example, in the Splunk web UI, you can type `framework=*` into the Search field. This will show all of the events where the `framework` field is defined:

<a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-exists.png" rel="attachment wp-att-1592"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-exists.png" alt="splunk-framework-exists" width="2826" height="1246" class="alignnone size-full wp-image-1592" /></a>

Click the disclosure triangle next to one of these events to view the details. This will show all of the fields extracted from the task log file path:

<a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-fields.png" rel="attachment wp-att-1591"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-fields.png" alt="splunk-fields" width="2297" height="727" class="alignnone size-full wp-image-1591" /></a>

Finally, let's search for all of the events that reference the framework ID of the event shown in the screenshot above, but that do not contain the chosen `framework` field. This will show us only non-task results:

<a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-search.png" rel="attachment wp-att-1593"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/splunk-framework-search.png" alt="splunk-framework-search" width="2832" height="1327" class="alignnone size-full wp-image-1593" /></a>

# <a name="templates"></a>Template Examples

Here are example query templates for aggregating the DCOS logs with Splunk. Replace the template parameters `$executor1`, `$framework2`, and any others with actual values from your cluster.

**Important:** Do not change the quotation marks in these examples or the queries will not work. If you create custom queries, be careful with the placement of quotation marks.

*   Logs related to a specific executor `$executor1`, including logs for tasks run from that executor:
    
        "$executor1"
        

*   Non-task logs related to a specific executor `$executor1`:
    
        "$executor1" AND NOT executor=$executor1
        

*   Logs (including task logs) for a framework `$framework1`, if `$executor1` and `$executor2` are that framework's executors:
    
        "$framework1" OR "$executor1" OR "$executor2"
        

*   Non-task logs for a framework `$framework1`, if `$executor1` and `$executor2` are that framework's executors:
    
        ("$framework1" OR "$executor1" OR "$executor2") AND NOT (framework=$framework1 OR executor=$executor1 OR executor=$executor2)
        

*   Logs for a framework `$framework1` on a specific agent host `$agent_host1`:
    
        host=$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2")
        

*   Non-task logs for a framework `$framework1` on a specific agent `$agent1` with host `$agent_host1`:
    
        host=$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2") AND NOT agent=$agent

 [1]: /logging/splunk/
 [2]: #splunkui
 [3]: #propsconf
 [4]: http://docs.splunk.com/Documentation/Splunk/latest/admin/Propsconf