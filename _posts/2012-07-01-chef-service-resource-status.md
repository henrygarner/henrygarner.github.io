---
layout: post
title:  "Chef service resource status"
date:   2012-07-01
categories: chef
---

I use [Chef](http://www.opscode.com/chef/) to manage my server configuration. Chef defines abstractions which allow you to be declarative about your setup at a high level (e.g. recipes, nodes, resources...) without getting caught up in precise implementation details.

One of the great things about using Chef compared to, say, a bunch of shell scripts, is that it's quite easy to make your Chef configuration [idempotent](http://en.wikipedia.org/wiki/Idempotence). In other words, running the same script 10 times should be no different to running it once.

The reason this is good is that you don't need to maintain separate copies of your deployment scripts for setting up new machines and for updating existing ones. All machines have the same code applied to them at every iteration and end up looking identical. 

Some care can be required to ensure this is the case. For example Chef defines an [Execute Resource](http://wiki.opscode.com/display/chef/Resources#Resources-Execute). This allows you to enter any arbitrary code you like to run on all machines every time Chef is called. The only protection you get against violating the principle of idempotence are the `not_if` or `only_if` parameters.

## Service Resource ##

I was surprised to discover that another resource, [Service](http://wiki.opscode.com/display/chef/Resources#Resources-Service), also appeared to violate this principle. The Service resource handles starting and stopping system services via Init, Upstart, etc.
    
    # typically this will run /etc/init.d/example_service start
    service "example_service" do
      action :start
    end

Once the service was already running, if I tried to provision the node a second time, I would get an error.

    FATAL: Chef::Exceptions::Exec: service[example_service] had an error: /etc/init.d/example_service start returned 1, expected 0
    
The reason for this is that Chef was attempting to start the service whilst it was already running. This caused the init.d script to fail and return a non-zero exit code.

Isn't this *exactly* what Chef is supposed to avoid?

## Supports ##

Chef's default assumption is that it has no way of querying whether the service is running. Not all init.d scripts implement the `status` option so Chef errs on the side of caution. Unfortunately this means that it will attempt to start the service every time it is run.

The fix is to reassure Chef that it can discover the service's status using the `supports` parameter.

    # typically this will run /etc/init.d/example_service start
    service "example_service" do
      supports :status => true
      action :start
    end
    
If your service script doesn't support the `status` command and you don't feel like adding it, you can still teach chef how to query the status. The `status_command` parameter allows you to specify any arbitrary command you like. Return 0 if the service is running, and anything else if it isn't.

    # typically this will run /etc/init.d/example_service start
    service "example_service" do
      supports :status => true
      status_command 'command goes here;'
      action :start
    end

Idempotence restored.

