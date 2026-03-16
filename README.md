The idea  

this is meant to tackle a typical lab situation for programming automation  
of different applications/tools/infrastructures  

A local incus provisioning host spawns multiple containers for testing purposes  
  Emphasis really is on local, experimentally extents the .ssh/config with an include  
  and write the include according to given hosts - not ideal, I might switch to  
  incus list => include file content  

Example here: redis with sentinel (wip)  
Deployment both of the container setup and redis gets realized with ansible plays  



Versions as of March 2026   

ansible core 2.20.2  
incus 6.21 ( using zabbly lts on pop!OS 24.04 LTS)  
ansible-lint ~ ubuntu/noble  

Incus Image  
src images:  
* debian/12/cloud*  
* debian/13/cloud*  

*incus cloud = cloud-init preinstalled  

Usage:
install given prerequisites then run   
ansible-playbook mkhost.yml --limit redis ( or other hosts )  
as example, in group vars redis vars.yml the os image is redefined,  
for now the provisioning config is aiming for incus debian 12/13 only, feel free to extend  


ansible plays 

Examples/deploy-redis.yml  // quick prototype instancing for groups of hostsi

mkhost.yml  // works with inventory,  use --limit
build_redis_on_dev2.yml // example for an existing host -   
redis_cluster.yml   // installs redis and redis-sentinel +  systemd units, config, starts service
  
redis_builder, uses a given host to compile the stable Version ( 8.6 at the moment )  
Attention : the build process eats up a lot   
of container RAM or diskspace depending on your additional cloud-init storage definitions, reserve at least 6GB (rust dev gets auto-installed by the src )   
   
Basic flow   
incus creates all given instances, static IP comes from inventory  
preparing for further automated configuration is done as follows.  
After container init ansible is slurping two templates, it inserts variables to feed cloud-init with  

  - user+ssh login   
  - networking setup   

redis_cluster   -  takes given hosts, artefact - versions ( variable is 8.6 as of march  26) 
                   templates for server and sentinel

Security  
  attention - ssh credentials  
  The vault contains a testing only public ssh key that is copied to the newly enabled user's ~./ssh folder  You need to add your own vault if you want to use this code.  

The cloud images feature an "all users are locked because no password" default for security which needs some configuring to prep for the following ansible configuration.  




Disclaimer.   
This is a quick and dirty simulacrum of the whole idea right now.  
Lots of static nonsense that should be dynamically defined.  
I'm on it - in my spare time.  

Supplemental  
  
Rough shell script to remove incus instances, testing  
```bash
incus list  
for i in ` incus list | grep 'RUNNING' | awk '{ print $2 }' `   
do  
	incus stop $i  
	incus delete $i  
done  
incus list  
```


source config examples for apt, remember to install their signing keys appropriately  

file: ansible-ubuntu-ansible-noble.list  
```apt
      deb https://ppa.launchpadcontent.net/ansible/ansible/ubuntu/ noble main  
      deb-src https://ppa.launchpadcontent.net/ansible/ansible/ubuntu/ noble main  
```
file: zabbly-incus-stable.sources  
```apt
      Enabled: yes  
      Types: deb  
      URIs: https://pkgs.zabbly.com/incus/stable  
      Suites: noble  
      Components: main  
      Architectures: amd64  
      Signed-By: /etc/apt/keyrings/zabbly.asc  
```
---

tmux conf for convenience
```bash
set -g mouse on
bind -n PageUp copy-mode -e \; send-keys -X page-up
bind -n PageDown copy-mode -e \; send-keys -X page-down
bind v split-window -h
bind h split-window -v
bind x kill-pane
bind-key -n F5 select-pane -t -
bind-key -n F8 select-pane -t +
```



This text is 100% organic, it contains 0% LLM
