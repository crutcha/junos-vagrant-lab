# junos-vagrant-lab

This is a JunOS lab using Vagrant specifically built with the topology from [MPLS In The SDN Era](http://shop.oreilly.com/product/0636920033905.do). The vagrantfile will spin up the environment with a barebones configuration with only IS-IS enabled for routing. Additional configuration needed for other chapters are done with Ansible playbooks. Since this uses ansible provisioner within Vagrant, it can only run on a linux/macOS host with ansible installed.  
