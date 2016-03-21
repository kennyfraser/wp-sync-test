---
UID: 56f0076347e51
post_title: 'Part 3: Using blue/green deployment'
post_excerpt: ""
layout: page
published: true
menu_order: 4
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
This tutorial shows how to deploy a new version of Oinker while keeping the current version online.

Blue-green deployment is a way to safely deploy applications that are serving live traffic by creating two versions of an application (BLUE and GREEN). To deploy a new version of the application, you will drain all traffic, requests, and pending operations from the current version of the application, switch to the new version, and then turn off the old version. Blue-green deployment eliminates application downtime and allows you to quickly roll back to the BLUE version of the application if necessary.

For an overview of the process, here's [a great article by Martin Fowler][1].

In a production environment, you would typically script this process and integrate it into your existing deployment system. Below, we provide an example of the steps necessary to perform a safe deployment using the DCOS CLI. (The DCOS CLI works with both DCOS and open source Marathon.)

## Requirements

*   The \[jq\] (https://stedolan.github.io/jq/) command-line JSON processor. ]

## Procedure

We will replace the current app version (BLUE) with a new version (GREEN).

1.  Create a Marathon app definition file named `green-oinker.json` by using nano, or another text editor of your choice. A Marathon app definition file specifies the required parameters for Marathon.
    
        $ nano green-oinker.json
        

2.  Add this content to your `oinker.json` file. Specified here is Oinker Docker container, the resources required by Oinker, Marathon health checks, and to initialize the Cassandra database.
    
        {
            "id": "/green-oinker",
            "instances": 3,
            "container": {
                "type": "DOCKER",
                "docker": {
                    "image": "mesosphere/oinker:shakespeare",
                    "network": "BRIDGE",
                    "portMappings": [
                        {
                            "containerPort": 3000,
                            "hostPort": 0,
                            "protocol": "tcp"
                        }
                    ]
                }
            },
            "healthChecks": [{
                "protocol": "TCP",
                "gracePeriodSeconds": 600,
                "intervalSeconds": 30,
                "portIndex": 0,
                "timeoutSeconds": 10,
                "maxConsecutiveFailures": 2
            }],
            "cmd": "until rake cassandra:setup; do sleep 5; done && rails server",
            "ports": [0],
            "cpus": 0.25,
            "mem": 256.0
        }
        

3.  Launch the new version of the app on Marathon with this command:
    
        $ dcos marathon app add green-oinker.json  
        

4.  Scale GREEN app instances by 1 or more. Initially (starting from 0 instances), set the number of app instances to the minimum required to serve traffic. Remember, no traffic will arrive yet: we haven't registered at the load balancer.
    
        $ dcos marathon app update /green-oinker instances=1
        

5.  Wait until all tasks from the GREEN app have passed health checks. This step requires \[jq\] (https://stedolan.github.io/jq/). <!-- This command doesn't seem to do anything. JSH -->
    
    $ dcos marathon app show /green-oinker | jq '.tasks[].healthCheckResults[] | select (.alive == false)'

6.  Use the code snippet above to check that all instances of GREEN are still healthy. Abort the deployment and begin rollback if you see unexpected behavior.

7.  Add the new task instances from the `green-oinker` app to the load balancer pool. <!-- What does this mean? JSH -->

8.  Pick one or more task instances from the current (BLUE) version of the app.
    
    dcos marathon task list /oinker

9.  Update the load balancer configuration to remove the task instances above from the BLUE app pool. <!-- What does this mean? JSH -->

10. Wait until the task instances from the BLUE app have 0 pending operations. Use the metrics endpoint in the application to determine the number of pending operations. <!-- What does this mean? JSH -->

11. Once all operations are complete from the `oinker` tasks, kill and scale the `oinker` app using \[the API\] (https://mesosphere.github.io/marathon/docs/rest-api.html#post-v2-tasks-delete). In the snippet below, `<hosturl>` is the hostname of your master node prefixed with `http://`. <!-- Does this mean that we cannot do this in DCOS Marathon? JSH -->
    
    echo "{\"ids\":[\"<task_id>\"]}" | curl -H "Content-Type: application/json" -X POST -d @- <hosturl>/marathon/v2/tasks/delete?scale=true
    
    This Marathon operation will remove specific instances (the ones with 0 pending operations) and prevent them from being restarted.

12. Repeat steps 2-9 until there are no more `oinker` tasks.

13. Remove the `oinker` app from Marathon.
    
    dcos marathon app remove /blue-myapp

 [1]: http://martinfowler.com/bliki/BlueGreenDeployment.html