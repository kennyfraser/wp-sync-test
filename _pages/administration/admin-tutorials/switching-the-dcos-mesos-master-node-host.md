---
UID: 56eaafe31a709
post_title: Changing the DCOS master node
post_excerpt: ""
layout: page
published: true
menu_order: 100
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: false
hide_from_related: false
---
In this example, the DCOS cluster Mesos master hostname is changed.

1.  Show the current configuration, where `core.dcos_url` is the hostname of your DCOS installation:
    
        $ dcos config show
        core.dcos_url=http://ab-test-2.us-west-2.elb.cloudhost.com
        core.email=youremail@email.com
        core.reporting=True
        core.token=375b30cd25c7c1f7952066098a8860c6d9d52e04e87d4dbff3ee1ea8b8fdac80
        package.cache=/Users/username/.dcos/cache
        package.sources=['https://github.com/mesosphere/universe/archive/version-1.x.zip']
        

2.  Set the hostname (`core.dcos_url`) to the new DCOS master `http://ab-test-3.us-west-2.elb.cloudhost.com`:
    
        $ dcos config set core.dcos_url http://ab-test-2.us-west-2.elb.cloudhost.com
        

3.  Verify that the hostname value has been updated:
    
        $ dcos config show core.dcos_url
        http://ab-test-3.us-west-2.elb.cloudhost.com