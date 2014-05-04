---
layout: post
title:  "SSH Agent Forwarding on Snow Leopard"
date:   2012-07-01
categories: os-x
---

[This post](http://www.schmidp.com/2009/06/23/enable-ssh-agent-key-forwarding-on-snow-leopard/) fairly accurately describes the frustration I had authenticating myself with remote services once I had shelled into another machine. For example, trying to connect to a Github private repo from within a Vagrant virtual machine would yield `Permission denied (publickey)`.

## I'm me! Can't you see? ##

[Agent forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html) should have been taking care of this for me, but it's turned off by default in Snow Leopard. To switch it back on edit the file `/etc/ssh_config` so that `ForwardAgent` is set to 'yes'.

    Host *
    ForwardAgent yes

A further complication was that I needed to let the ssh authentication agent know about my identity file. 

    ssh-add
    
By default this will add your `~/.ssh/id_rsa` keyfile. If you have others to add you will need to name them explicitly.

    ssh-add -K [path/to/keyfile]

Now, whether I'm logging in to a local Vagrant virtual machine, or an EC2 instance, I no longer need to copy across or generate a key in order to authenticate with Github.
