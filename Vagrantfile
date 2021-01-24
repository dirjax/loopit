# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = [
  { :last => true, :hostname => 'play-01', :ip => '192.168.56.210', :forwards => [{:guest => "22", :host => "2210", :id => "ssh" }]},
  { :last => true, :hostname => 'play-02', :ip => '192.168.56.209', :forwards => [{:guest => "22", :host => "2209", :id => "ssh" }]},
  { :last => true, :hostname => 'play-03', :ip => '192.168.56.208', :forwards => [{:guest => "22", :host => "2208", :id => "ssh" }]}
]


Vagrant.configure(2) do |config|

  nodes.each do |node|

    config.vm.define node[:hostname] do |debiannode_config|
      debiannode_config.vm.box = "debian/buster64";

      config.vm.provider "virtualbox" do |v|
        v.memory = node[:ram] ? node[:ram] : 1024;
        v.cpus = 2
      end

      debiannode_config.vm.host_name = node[:hostname]
      debiannode_config.vm.network :private_network, ip: node[:ip]

      if node[:forwards]
        node[:forwards].each do |forward|
          debiannode_config.vm.network :forwarded_port, guest: forward[:guest], host: forward[:host], id: forward[:id]
        end
      end

      if node[:mounts]
        node[:mounts].each do |mount|
          debiannode_config.vm.synced_folder mount[:src], mount[:mnt], owner: "root", group: "root", create: true
        end
      end

    end


  end

end

