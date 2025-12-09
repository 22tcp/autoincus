Folks.

It's a mess right now. Many things have to be turned into varibles, yet,
but I needed a working range of reliably reachable instances spawned from an ansible inventory with FIXED IPs.
Basic flow is
define two yaml files:
  - user+ssh login
  - networking setup

the hardcoded image is taken from incus images:debian/13/cloud  <- which has cloud-init preinstalled,
that's used for the alterations needed to be able to login with key exchange.
The vault contains the public ssh key that is copied to the new user's ~./ssh folder.

The cloud images also have a "all users are locked because no password" default.

Ansible runs through the given hostgroup and creates aforementioned files per each instance at the moment in /tmp
then uses these as passed arguments for incus launch config.

Todos
  - cleanup /tmp after run
  - make things dynamic, move stuff to ansible vars, 
  - create a role of it
