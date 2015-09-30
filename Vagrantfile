# -*- mode: ruby -*-
# vi: set ft=ruby :
#

$domain                   = "lab.local"
$master_hostname          = "puppet"
$master_ip                = "192.168.100.100"

$peinstaller_url          = "https://pm.puppetlabs.com/puppet-enterprise/3.8.2/puppet-enterprise-3.8.2-el-7-x86_64.tar.gz"
#$peinstaller_url          = "http://192.168.0.5/puppet-enterprise-3.8.2-el-7-x86_64.tar"

$peinstaller_url_windows  = "http://pm.puppetlabs.com/puppet-enterprise/3.8.2/puppet-enterprise-3.8.2-x64.msi"

$peanswers_url            = "https://raw.githubusercontent.com/zoojar/openstack-lab/master/puppet.lab.local.answers"
$r10kyaml_url             = "https://raw.githubusercontent.com/zoojar/openstack-lab/master/r10k.yaml"
$autosign_these_nodes     = "*"

load 'roosters'

nodes = [
  { 
    :hostname        => $master_hostname, 
    :domain          => $domain,
    :ip              => $master_ip, 
    :ip_2            => '192.168.200.100', 
    :box             => 'puppetlabs/centos-7.0-64-nocm', 
    :ram             => 8000,
    :cpus            => 4,
    :cpuexecutioncap => 80,
    :shell_script    => $install_puppet_master_centos7, 
    :shell_args      => [$peinstaller_url, $peanswers_url, $r10kyaml_url, $master_hostname, $domain, $master_ip] 
  },
  { 
    :hostname        => 'openstack-aio-01',
    :domain          => $domain,
    :ip              => '192.168.100.12', 
    :ip_2            => '192.168.200.12',
    :box             => 'puppetlabs/ubuntu-14.04-64-nocm', 
    :ram             => 8000,
    :cpus            => 4,
    :cpuexecutioncap => 50,
    :shell_script    => $install_puppet_agent_linux, 
    :shell_args      => [$master_ip, $master_hostname, $domain] 
  }
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box      = node[:box]
      nodeconfig.vm.hostname = node[:domain] ? "#{node[:hostname]}.#{node[:domain]}" : "#{node[:hostname]}" ;
      memory                 = node[:ram] ? node[:ram] : 1000 ; 
      cpus                   = node[:cpus] ? node[:cpus] : 2 ;
      cpuexecutioncap        = node[:cpuexecutioncap] ? node[:cpuexecutioncap] : 50 ;
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.network :private_network, ip: node[:ip_2]
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [
          "modifyvm", :id,
          "--memory", memory.to_s,
          "--cpus", cpus.to_s,
          "--cpuexecutioncap", cpuexecutioncap.to_s,
        ]
      end
      nodeconfig.vm.provision :reload
      nodeconfig.vm.provision "shell" do | s |
        s.inline = node[:shell_script]
        s.args   = node[:shell_args]
      end
    end
  end
end
