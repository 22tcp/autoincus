Scope  

A local incus provisioning host spawns multiple containers for testing purposes ( example here, redis with sentinel )
Deployment both of the container setup and redis shall be done by ansible playbooks.

Versions as of Jan 2026
ansible 2.16.3
incus 6.20 ( using zabbly lts on pop!OS 24.04 LTS)

News
ran and implemented ansible-lint suggestions

Basic flow  
using two templates:  
  - user+ssh login  
  - networking setup  

the hardcoded image is taken from incus images:debian/13/cloud  <- which has cloud-init preinstalled,
that's used for the alterations needed to be able to login with key exchange.
The vault contains the public ssh key that is copied to the new user's ~./ssh folder.  
  
The cloud images feature a  "all users are locked because no password" default for security, good.  

Ansible loops through the given hostgroup (redis)  and  hands the quoted yaml config for cloud-init to the incus launch --config options.

Future planning
  - replace play and in-situ tasks with roles -
  where it is making sense, the targeting is a lil' too specific -
  creating a "quick" solution in a larger project ecosystem must do for now

Todos  
  - even more items dynamic eg transition to ansible vars 
  - redis host/cluster deployment in relation to offered host count, 
    using a replication distribution rule ( a nice opportunity to make a deep-dive  )

Disclaimer.   
This is just a half-baked simulacrum of the whole idea right now. Lots of static nonsense that should be dynamically defined.
Still, I wanted and do get a working number of reliably reachable instances,
spawned from an ansible inventory with FIXED IPs to ssh into with a given user.
Addendum. Text was written with VI, not AI.
