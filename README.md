The idea  

this is meant to tackle a typical lab situation for programming automation  
of different applications/tools/infrastructures  

A local incus provisioning host spawns multiple containers for testing purposes  
Example here: redis with sentinel (wip)  
Deployment both of the container setup and redis gets realized with ansible plays  

Versions as of Jan 2026    

ansible core 2.19.5  
incus 6.20 ( using zabbly lts on pop!OS 24.04 LTS)  
ansible-lint ~ ubuntu/noble  

Incus Image  
src images:debian/13/cloud  ( has cloud-init preinstalled )  


plays  

testing  
deploy-redis.yml  // quick prototype for group instancing  

preparation for role construction  
mkhost.yml  // works with inventory,  using --limit strongly recommended  

Basic flow   
incus creates all given instances, static IP comes from inventory  
preparing for further automated configuration is done as follows.  
After container init ansible is slurping two templates, it inserts variables to feed cloud-init with  

  - user+ssh login   
  - networking setup   


Security  
  attention - ssh credentials  
  The vault contains a testing only public ssh key that is copied to the newly enabled user's ~./ssh folder  You need to add your own vault if you want to use this code.  

The cloud images feature an "all users are locked because no password" default for security which needs some configuring to prep for the following ansible configuration.  

Ansible loops through the given hostgroup (redis)  and  hands the quoted yaml config for cloud-init to the incus launch --config options.  

Future planning  
  - replace play and in-situ tasks with roles  
    creating a "quick" solution in a larger project ecosystem only goes so far  

Generic Todos  
  - even more items dynamic eg transition to ansible vars   
  - structure this readme better, summarize more  

Special tasks  
  - compute redis host/cluster deployment in relation to host count,   
    using a replication distribution rule ( a nice opportunity to make a deep-dive  )  

Disclaimer.   
This is a quick and dirty simulacrum of the whole idea right now.  
Lots of static nonsense that should be dynamically defined.  
I'm on it - in my spare time.  

Supplemental  
  
Rough shell script to remove incus instances, testing  

incus list  
for i in ` incus list | grep 'RUNNING' | awk '{ print $2 }' `   
do  
	incus stop $i  
	incus delete $i  
done  
incus list  

source config examples for apt, remember to install their signing keys appropriately  

file: ansible-ubuntu-ansible-noble.list  
      deb https://ppa.launchpadcontent.net/ansible/ansible/ubuntu/ noble main  
      deb-src https://ppa.launchpadcontent.net/ansible/ansible/ubuntu/ noble main  

file: zabbly-incus-stable.sources  
      Enabled: yes  
      Types: deb  
      URIs: https://pkgs.zabbly.com/incus/stable  
      Suites: noble  
      Components: main  
      Architectures: amd64  
      Signed-By: /etc/apt/keyrings/zabbly.asc  

This text is 100% organic meaning it contains 0% LLM
