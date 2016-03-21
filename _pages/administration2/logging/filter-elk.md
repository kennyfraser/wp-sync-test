---
UID: 56eac56b30a1c
post_title: Filtering Task Logs with ELK
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
The file system paths of DCOS task logs contain information such as the agent ID, framework ID, and executor ID. You can use this information to filter the log output for specific tasks, applications, or agents.

**Prerequisite**

*   [An ELK stack that aggregates DCOS logs][1]

# <a name="configuration"></a>Configuration

1.  Create the following `dcos` pattern file in your custom patterns directory located at `$PATTERNS_DIR`:
    
        PATHELEM [^/]+
        TASKPATH ^/var/lib/mesos/slave/slaves/%{PATHELEM:agent}/frameworks/%{PATHELEM:framework}/executors/%{PATHELEM:executor}/runs/%{PATHELEM:run}
        

2.  Update the configuration file for your Logstash instance to include the following `grok` filter, where `$PATTERNS_DIR` is replaced with your custom patterns directory:
    
        filter {
            grok {
                patterns_dir => "$PATTERNS_DIR"
                match => { "file" => "%{TASKPATH}" }
            }
        }
        

3.  Restart Logstash.
    
    Logstash will extract the `agent`, `framework`, `executor`, and `run` fields. These fields are shown in the metadata of all Mesos task log events. Elasticsearch queries will also show results from those fields.

# <a name="usage"></a>Usage Example

In the screenshots below, we are using Kibana hosted by [logz.io][2], but your Kibana interface will look similar.

For example, you can type `framework:*` into the Search field. This will show all of the events where the `framework` field is defined:

<a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-exists.png" rel="attachment wp-att-1530"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-exists.png" alt="logstash-framework-exists" width="2839" height="1246" class="alignnone size-full wp-image-1530" /></a>

Click the disclosure triangle next to one of these events to view the details. This will show all of the fields extracted from the task log file path:

<a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-fields.png" rel="attachment wp-att-1529"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-fields.png" alt="logstash-fields" width="2318" height="922" class="alignnone size-full wp-image-1529" /></a>

Finally, let's search for all of the events that reference the framework ID of the event shown in the screenshot above, but that do not contain the chosen `framework` field. This will show only non-task results:

<a href="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-search.png" rel="attachment wp-att-1531"><img src="http://live-mesosphere-documentation.pantheon.io/wp-content/uploads/2015/12/logstash-framework-search.png" alt="logstash-framework-search" width="2844" height="1361" class="alignnone size-full wp-image-1531" /></a>

# <a name="templates"></a>Template Examples

Here are some example query templates. Replace the template parameters `$executor1`, `$framework2`, and any others with the actual values from your cluster.

**Important:** Do not change the quotation marks in these examples or the queries will not work. If you create custom queries, be careful with the placement of quotation marks.

*   Logs related to a specific executor `$executor1`, including logs for tasks run from that executor:
    
        "$executor1"
        

*   Non-task logs related to a specific executor `$executor1`:
    
        "$executor1" AND NOT executor:$executor1
        

*   Logs (including task logs) for a framework `$framework1`, if `$executor1` and `$executor2` are that framework's executors:
    
        "$framework1" OR "$executor1" OR "$executor2"
        

*   Non-task logs for a framework `$framework1`, if `$executor1` and `$executor2` are that framework's executors:
    
        ("$framework1" OR "$executor1" OR "$executor2") AND NOT (framework:$framework1 OR executor:$executor1 OR executor:$executor2)
        

*   Logs for a framework `$framework1` on a specific agent host `$agent_host1`:
    
        host:$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2")
        

*   Non-task logs for a framework `$framework1` on a specific agent `$agent1` with host `$agent_host1`:
    
        host:$agent_host1 AND ("$framework1" OR "$executor1" OR "$executor2") AND NOT agent:$agent

 [1]: /logging/elk/
 [2]: http://logz.io