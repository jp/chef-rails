# -*- mode: ruby -*-
# vi: set ft=ruby :
 
VAGRANTFILE_API_VERSION = "2"
$script = <<SCRIPT
# Silly Ubuntu 12.04 doesn't have the
# --stdin option in the passwd utility
echo root:vagrant | chpasswd
cat << EOF >> /etc/hosts
192.168.10.10 chef
192.168.10.11 rails
EOF
SCRIPT
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  config.vm.box = "precise64"
 
  # Turn off shared folders
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
 
  # Begin chef
  config.vm.define "chef" do |chef_config|
    chef_config.vm.hostname = "chef"
    chef_config.vm.provision "shell", inline: $script
    # eth1 configured in the 192.168.236.0/24 network
    chef_config.vm.network "private_network", ip: "192.168.236.10"
    chef_config.vm.provider "vmware_workstation" do |v|
        v.vmx["memsize"] = "2048"
        v.vmx["numvcpus"] = "1"
    end
    chef_config.vm.provider "vmware_fusion" do |v|
        v.vmx["memsize"] = "2048"
        v.vmx["numvcpus"] = "1"
    end
    chef_config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", "2048"]
        v.customize ["modifyvm", :id, "--cpus", "1"]
    end
    chef_config.vm.network "forwarded_port", guest: 443, host: 1080

  end
  # End chef
 
  # Begin rails
  config.vm.define "rails" do |rails_config|
    rails_config.vm.hostname = "rails"
    rails_config.vm.provision "shell", inline: $script
    rails_config.vm.box = "precise64"
    # eth1 configured in the 192.168.236.0/24 network
    rails_config.vm.network "private_network", ip: "192.168.236.11"
    rails_config.vm.provider "vmware_workstation" do |v|
        v.vmx["memsize"] = "2048"
        v.vmx["numvcpus"] = "2"
    end
 
    rails_config.vm.provider "vmware_fusion" do |v|
        v.vmx["memsize"] = "2048"
        v.vmx["numvcpus"] = "2"
    end
    rails_config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", "2048"]
        v.customize ["modifyvm", :id, "--cpus", "2"]
    end
    rails_config.vm.network "forwarded_port", guest: 443, host: 1081
  end
  # End rails
end
