Hello, Folks.

Pre-alpha note. This is just a half-baked simulacrum of the whole idea right now. Lots of static nonsense that should be dynamically defined.
Still, I wanted and do get a working number of reliably reachable instances spawned from an ansible inventory with FIXED IPs to ssh into with my user.

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
  - make more items dynamic eg transition to ansible vars, 
  - create a role of it

Disclaimer. 0% AI text.
