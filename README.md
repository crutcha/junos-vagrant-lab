# junos-vagrant-lab

This is a JunOS lab using Vagrant specifically built with the topology from [MPLS In The SDN Era](http://shop.oreilly.com/product/0636920033905.do). This can be run either from a local linux machine or a VPS provider(DigitalOcean cloud-init YAML included). The vagrantfile will spin up the environment with a barebones configuration with only IS-IS enabled for routing. Additional configuration needed for other chapters are done with Ansible playbooks. 
