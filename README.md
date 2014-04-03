```
<!--
# README.md for the cluster automation with Ansible
#
# Copyright (c) 2014 PP
#
# author: piotr.pachalko@tieto.com
-->
```
---
# Overview #


---
# Getting started

Sources are hosted in the [Git repository](https://...).

## Prerequisities
You will need:

-  [Git](http://git-scm.com)

First of all, if you are on Windows, switch to Linux (or Cygwin) before proceeding with this tutorial. It will save you lot of nerves, as Windows is not a devops-friendly system. As for Linux users, it has been tested on Ubuntu Precise. I dont know about OS X, though.

## Install Ansible tool
That should take no more than 5 minutes. We will use bleeding edge version from Ansible repository.

    $ git clone git://github.com/ansible/ansible.git
    $ cd ./ansible
    $ source ./hacking/env-setup
    $ sudo easy_install pip
    $ sudo apt-get install python-dev
    $ sudo pip install paramiko PyYAML jinja2 httplib2  

Test that all is good by ansible-pinging own machine

    $ echo "127.0.0.1" > ~/ansible_hosts
    $ export ANSIBLE_HOSTS=~/ansible_hosts
    $ ansible all -m ping --ask-pass

You may want to install Ansible for all users

    $ sudo make install

## Get the source repo
Assuming that git is already available, clone the entire project

    $ git clone https://.../ansible-clusters
    $ cd ansible-clusters
    
## Provision the cluster in the cloud

### Select your target cloud and credentials
Edit `ansible.cfg` and edit location of hostfile, if necessary, e.g. if you work with Amazon AWS

    hostfile = ./clouds/amazon-aws/ec2.py

Export credentials as variables

    $ source ./clouds/amazon-aws/credentials.sh

Look what's already there in the cloud

    $ ./clouds/amazon-aws/ec2.py --list

### Launch entire cluster
Select playbook containing the cluster definition and see what nodes would be affected and how

    $ ansible-playbook cluster-foo.yml --list-hosts
    $ ansible-playbook cluster-foo.yml --list-tasks

Now, run the playbook to execute all steps to bring up the cluster

    $ ansible-playbook cluster-foo.yml

Stay patient. *Ansible* will launch necessary instances in the cloud and start configuring them. It takes time on the first run, because *Ansible* must run all the steps that were not yet executed. Subsequent runs will go faster, unless you destroy the entire cluster or part of it.



After the nodes are up, use `ssh` to connect to the selected node

    ssh {node_name}

- - -
