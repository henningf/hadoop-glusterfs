Vagrant setup for Hadoop with gluster-storage.
==============================================

This repo contains Ansible code for Hadoop and glusterFS,
[Hadoop](https://hadoop.apache.org/)
[GlusterFS](https://www.gluster.org/)
This was made to test hadoop with glusterFS storage, the glusterFS plugin
for Hadoop was built from:
[GlusterFS Hadoop plugin](https://github.com/gluster/glusterfs-hadoop)

NB! The Vagrant config file has been written for libvirt, it has
not been testet against Virtualbox or other providers.

Running Hadoop as the hadoop user is still work in progress.

I took the ntp module from:
[ansible-role-ntp](https://github.com/resmo/ansible-role-ntp)
thanks :)

Getting Started
---------------

# Installation
* Install Vagrant and Ansible
[Vagrant](https://docs.vagrantup.com/v2/installation/)
[Ansible](http://docs.ansible.com/ansible/intro_installation.html)
* run: vagrant up --no-provision
* run: vagrant provision

There should now be three virtual machines, ssh into the hmaster machine
and run start-yarn.sh as root, you should now reach the hadoop cluster on:
http://hmaster.example.com:8088/
