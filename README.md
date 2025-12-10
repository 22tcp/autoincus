Scope  

A local incus provisioning host spawns multiple containers for testing purposes ( example here, redis with sentinel )
Deployment both of the container setup and redis shall be done by ansible playbooks.

Prerequisites
ansible ( installed version of the official ansible deb repo )
incus ( using zabbly lts )

Tested on pop!OS 22.04 LTS 

Basic flow 
using two templates:
  - user+ssh login
  - networking setup

the hardcoded image is taken from incus images:debian/13/cloud  <- which has cloud-init preinstalled,
that's used for the alterations needed to be able to login with key exchange.
The vault contains the public ssh key that is copied to the new user's ~./ssh folder.

The cloud images feature a  "all users are locked because no password" default for security, good.

Ansible loops through the given hostgroup (redis)  and  hands the quoted yaml config for cloud-init to the incus launch --config options.

Todos
  - code second part: redis server and sentinel nodes
  - more items dynamic eg transition to ansible vars  -> create a role of it

Disclaimer. 
This is just a half-baked simulacrum of the whole idea right now. Lots of static nonsense that should be dynamically defined.
Still, I wanted and do get a working number of reliably reachable instances spawned from an ansible inventory with FIXED IPs to ssh into with my user.
Also, 0% of this text was written by AI. It rather was with VI.
