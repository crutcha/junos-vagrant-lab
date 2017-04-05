# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "base"

  # General options
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
  end

  # Ansible control machine
  # We could do this from localhost unless its windows so lets just
  # Include an ansible VM for windows users. 
  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/xenial64"
    ansible.vm.network "private_network", virtualbox__intnet: "management",
            ip: "10.0.0.1/24", auto_config: true
    ansible.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y python-dev python-pip ansible libxml2-dev build-essential git
      sudo pip install junos-eznc
      sudo pip install cryptography==1.2.1
      sudo ansible-galaxy --force install Juniper.junos
    SHELL
  end 

  config.vm.define "CE1" do |ce1|
    ce1.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    ce1.vm.host_name = "CE1"
    ce1.vm.network "private_network", virtualbox__intnet: "HE1-CE1"
    ce1.vm.network "private_network", virtualbox__intnet: "CE1-PE1"
    ce1.vm.network "private_network", virtualbox__intnet: "CE1-PE2"
  end



end
