# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "base"

  # General options
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "512"
  end

=begin
  # Ansible control machine
  # We could do this from localhost unless its windows so lets just
  # Include an ansible VM for windows users. 
  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/xenial64"
    ansible.vm.network "private_network", virtualbox__intnet: "management",
            ip: "10.0.0.1/24", auto_config: true
    ansible.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y python-dev python-pip ansible libxml2-dev build-essential git tmux
      sudo pip install junos-eznc
      sudo pip install cryptography==1.2.1
      sudo ansible-galaxy --force install Juniper.junos
      git clone https://github.com/crutcha/dotfiles.git ~/dotfiles/
      git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
      cp ~/dotfiles/.bashrc ~/
      cp ~/dotfiles/.tmux.conf ~/
      cp ~/dotfiles/.vimrc ~/
      vim -c 'PluginInstall' -c 'qa!'
    SHELL
  end 
=end

  config.vm.define "H1" do |h1|
    h1.vm.box = "ubuntu/xenial64"
    h1.vm.network "private_network", virtualbox__intnet: "H1-CE",
            ip: "10.1.12.10/24", auto_config: true
  end

  config.vm.define "CE1" do |ce1|
    ce1.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    ce1.vm.host_name = "CE1"
    ce1.vm.network "forwarded_port", guest: 22, host: 2001, id: "ssh"
    ce1.vm.network "private_network", virtualbox__intnet: "H1-CE"
    ce1.vm.network "private_network", virtualbox__intnet: "CE1-PE1"
    ce1.vm.network "private_network", virtualbox__intnet: "CE1-PE2"
  end

  config.vm.define "CE2" do |ce2|
    ce2.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    ce2.vm.host_name = "CE2"
    ce2.vm.network "forwarded_port", guest: 22, host: 2002, id: "ssh"
    ce2.vm.network "private_network", virtualbox__intnet: "H1-CE"
    ce2.vm.network "private_network", virtualbox__intnet: "CE2-PE1"
    ce2.vm.network "private_network", virtualbox__intnet: "CE2-PE2"
  end

  config.vm.define "PE1" do |pe1|
    pe1.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    pe1.vm.host_name = "PE1"
    pe1.vm.network "forwarded_port", guest: 22, host: 2003, id: "ssh"
    pe1.vm.network "private_network", virtualbox__intnet: "CE1-PE1"
    pe1.vm.network "private_network", virtualbox__intnet: "CE1-PE2"
    pe1.vm.network "private_network", virtualbox__intnet: "PE1-PE2"
    pe1.vm.network "private_network", virtualbox__intnet: "PE1-P1"
  end

  config.vm.define "PE2" do |pe2|
    pe2.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    pe2.vm.host_name = "PE2"
    pe2.vm.network "forwarded_port", guest: 22, host: 2004, id: "ssh"
    pe2.vm.network "private_network", virtualbox__intnet: "PE2-CE2"
    pe2.vm.network "private_network", virtualbox__intnet: "PE2-CE1"
    pe2.vm.network "private_network", virtualbox__intnet: "PE2-PE1"
    pe2.vm.network "private_network", virtualbox__intnet: "PE2-P2"
  end

  config.vm.define "P1" do |p1|
    p1.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    p1.vm.host_name = "P1"
    p1.vm.network "forwarded_port", guest: 22, host: 2005, id: "ssh"
    p1.vm.network "private_network", virtualbox__intnet: "PE1-P1"
    p1.vm.network "private_network", virtualbox__intnet: "RR1-P1"
    p1.vm.network "private_network", virtualbox__intnet: "P1-P2-1"
    p1.vm.network "private_network", virtualbox__intnet: "P1-P2-2"
    p1.vm.network "private_network", virtualbox__intnet: "P1-RR2"
    p1.vm.network "private_network", virtualbox__intnet: "P1-PE3"
  end

  config.vm.define "P2" do |p2|
    p2.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    p2.vm.host_name = "P2"
    p2.vm.network "forwarded_port", guest: 22, host: 2006, id: "ssh"
    p2.vm.network "private_network", virtualbox__intnet: "PE2-P2"
    p2.vm.network "private_network", virtualbox__intnet: "RR2-P2"
    p2.vm.network "private_network", virtualbox__intnet: "P1-P2-1"
    p2.vm.network "private_network", virtualbox__intnet: "P1-P2-2"
    p2.vm.network "private_network", virtualbox__intnet: "P2-RR2"
    p2.vm.network "private_network", virtualbox__intnet: "P2-PE4"
  end  

  config.vm.define "RR1" do |rr1|
    rr1.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    rr1.vm.host_name = "RR1"
    rr1.vm.network "forwarded_port", guest: 22, host: 2007, id: "ssh"
    rr1.vm.network "private_network", virtualbox__intnet: "RR1-P1"
    rr1.vm.network "private_network", virtualbox__intnet: "RR1-RR2"
    rr1.vm.network "private_network", virtualbox__intnet: "RR1-P2"
  end 
 
  config.vm.define "RR2" do |rr2|
    rr2.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    rr2.vm.host_name = "RR2"
    rr2.vm.network "forwarded_port", guest: 22, host: 2008, id: "ssh"
    rr2.vm.network "private_network", virtualbox__intnet: "P1-RR2"
    rr2.vm.network "private_network", virtualbox__intnet: "RR1-RR2"
    rr2.vm.network "private_network", virtualbox__intnet: "P2-RR2"
  end

  config.vm.define "PE3" do |pe3|
    pe3.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    pe3.vm.host_name = "PE3"
    pe3.vm.network "forwarded_port", guest: 22, host: 2009, id: "ssh"
    pe3.vm.network "private_network", virtualbox__intnet: "P1-PE3"
    pe3.vm.network "private_network", virtualbox__intnet: "PE3-PE4"
    pe3.vm.network "private_network", virtualbox__intnet: "PE3-BR3"
  end
 
  config.vm.define "PE4" do |pe4|
    pe4.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    pe4.vm.host_name = "PE4"
    pe4.vm.network "forwarded_port", guest: 22, host: 2010, id: "ssh"
    pe4.vm.network "private_network", virtualbox__intnet: "P2-PE4"
    pe4.vm.network "private_network", virtualbox__intnet: "PE3-PE4"
    pe4.vm.network "private_network", virtualbox__intnet: "PE4-BR4"
  end 
 
  config.vm.define "BR3" do |br3|
    br3.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    br3.vm.host_name = "BR3"
    br3.vm.network "forwarded_port", guest: 22, host: 2011, id: "ssh"
    br3.vm.network "private_network", virtualbox__intnet: "PE3-BR3"
    br3.vm.network "private_network", virtualbox__intnet: "BR-H3"
  end
 
  config.vm.define "BR4" do |br4|
    br4.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    br4.vm.host_name = "BR4"
    br4.vm.network "forwarded_port", guest: 22, host: 2012, id: "ssh"
    br4.vm.network "private_network", virtualbox__intnet: "PE4-BR4"
    br4.vm.network "private_network", virtualbox__intnet: "BR-H3"
  end 
  
  config.vm.define "H3" do |h3|
    h3.vm.box = "ubuntu/xenial64"
    h3.vm.network "private_network", virtualbox__intnet: "BR-H3",
            ip: "10.2.34.30/24", auto_config: true
  end

  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
                "P_Nodes" => ["P1", "P2"],
                "PE_Nodes" => ["PE1", "PE2", "PE3", "PE4"],
                "CE_Nodes" => ["CE1", "CE2", "BR3", "BR4"],
                "all:children" => ["P Nodes", "PE Nodes", "CE Nodes"]
    }
    ansible.playbook = "baseconfig.yml"
    ansible.verbose = true
  end



end
