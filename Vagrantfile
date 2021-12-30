# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
    
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/root/docker"
  config.vm.provider "virtualbox" do |v|
      v.memory = "2048"
  end
end
