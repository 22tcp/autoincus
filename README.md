Hello, Folks.

Pre-alpha note. This is just a half-baked simulacrum of the idea right now. Lots of static nonsense that should be dynamically resolved.
Still, I needed a working number of reliably reachable instances spawned from an ansible inventory with FIXED IPs becoming reachable from the outside.

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
  - do not rely on /tmp
  - make things dynamic, move stuff to ansible vars, 
  - create a role of it
  - human linting

Disclaimer. 0% AI text.
