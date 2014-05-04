---
layout: post
title:  "Chef cookbook for Java on Ubuntu"
date:   2012-06-11
categories: chef
---

When [Oracle revoked Ubuntu's partner licence](https://lists.ubuntu.com/archives/ubuntu-security-announce/2011-December/001528.html) last year, installing Oracle Java on Ubuntu became a hassle. Instead of simply running `apt-get` or `aptitude` to grab the packages from Ubuntu's partner repository, sysadmins either had to migrate to OpenJDK or set up their own private repositories from Oracle's website. Neither alternative was particularly attractive to me.

Fortunately [Martin Wimpress](http://blog.flexion.org/2012/01/16/install-sun-java-6-jre-jdk-from-deb-packages/) has developed a great [script for setting up a local Java apt repository on Ubuntu](https://github.com/flexiondotorg/oab-java6) using Oracle's Java binaries without breaking the licensing terms. This makes installing Oracle Java as simple as executing a single command-line script again.

## Chef Java Cookbook ##

I use [Chef](http://www.opscode.com/chef/) to manage server my server configuration so I created a Chef cookbook that wraps Martin's script to install Java. You can download it from [https://github.com/henrygarner/java](https://github.com/henrygarner/java).

The default configuration will install the `sun-java6-jdk` package from the local repository. If you'd like to install `oracle-java7-jdk` instead, set the `node[:java][:jdk_version]` to '7'.

## Vagrant and Hadoop ##

Some of the work I do involves writing code for Hadoop clusters. I use [Vagrant](http://vagrantup.com/) for local development so that I can test code in an environment closely resembling production. Vagrant works with Chef to configure and run a virtual machine from each project directory containing a Vagrantfile.

I download the [Java](https://github.com/henrygarner/java) and [Hadoop](https://github.com/henrygarner/hadoop) cookbooks into a `chef/cookbooks` directory relative to this Vagrantfile:

<script src="http://gist.github.com/2907213.js"></script>

Running `vagrant up` from the directory containing the Vagrantfile initiates the configuration process, downloading the base virtual machine if it isn't already locally installed, and then executing the cookbook code on top. After about 10 minutes I have a local development Hadoop installation accessible via `vagrant ssh`.

