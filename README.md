#openstack-lab  
##Vagrant lab setup for PE 3.8.2 & Openstack  
1x Puppet Master Server  
1x All-in-one Openstack Server   

###Requirements
Virtualbox 5.0.0  
Vagrant 1.7.4  
Vagrant-Reload Vagrant plugin 0.0.1  


###Getting started  
1. Clone this repo  
2. Run:   
```  
vagrant up  

```



###Config

####Initial Provisioning
Vagrant uses the provisioning script 'roosters':  
https://github.com/zoojar/openstack-lab/blob/master/roosters  
  

This script configures the following:
- '/etc/hosts' file
- Disable firewall
- Download PE installer tarball
- Download PE answers file from this repo
- Install puppet using the answers file
- Download hiera.yaml config file to 'hiera_config' filepath
- Configures r10k using puppet module 'zack/r10k'
- Deploys environment modules using r10k and the control-repo specified in 'r10k.pp'
- Refreshes the classes on the master (post r10k run)
- Creates nodes groups specified in the puppet module 'zj/classifier' (deployed by r10k) using the puppet module 'prosvcs/node_manager'
- Sets up autosigning using 'auutosign.sh' and a global psk to allow auto-signing of the openstack node




