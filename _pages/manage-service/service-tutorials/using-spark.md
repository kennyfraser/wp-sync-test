---
ID: 1702
post_title: Customizing Your Spark Installation
post_date: 2015-12-29 14:45:49
post_excerpt: ""
layout: page
permalink: >
<<<<<<< HEAD
  https://test-mesosphere-documentation.pantheon.io/manage-service/service-tutorials/using-spark/
=======
  https://dev-mesosphere-documentation.pantheon.io/manage-service/service-tutorials/using-spark/
>>>>>>> staging
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
# Installing the Spark app only, not CLI

In this example, the Spark app is installed without the Spark CLI:

    $ dcos package install —app spark
    

# Installing Spark with A Custom Name

In this example, Spark is installed with the explicit application ID `alice-spark` specified:

    $ dcos package install --app-id=alices-spark spark
    Installing package [spark] version [1.4.0-SNAPSHOT] with app id [alices-spark]
    Installing CLI subcommand for package [spark]
    Spark cluster mode on Mesos is ready!