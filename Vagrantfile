# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :server => {
        :box_name => "Fedora",
        :vm_name => "server",
        :net => [
                    ["192.168.57.2",  2, "255.255.255.0",  "ipa"],
                    ["192.168.50.15",  8, "255.255.255.0"],
                ]
  },
  :client1 => {
        :box_name => "Fedora",
        :vm_name => "client1",
        :net => [
                  ["192.168.57.3",  2, "255.255.255.0",  "ipa"],
                  ["192.168.50.16",  8, "255.255.255.0"],
                ]
  },
:client2 => {
        :box_name => "Fedora",
        :vm_name => "client2",
        :net => [
                  ["192.168.57.4",  2, "255.255.255.0",  "ipa"],
                  ["192.168.50.17",  8, "255.255.255.0"],
                ]
  },

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
    
    config.vm.define boxname do |box|
   
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      box.vm.box_version = boxconfig[:box_version]

      config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
       end
        

        boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end
      
      if boxconfig[:vm_name] == "client2"
       box.vm.provision "ansible" do |ansible|
        ansible.playbook = "provision.yml"
        ansible.inventory_path = "Inventory/hosts.ini"
        ansible.host_key_checking = "false"
        ansible.become = "true"
        ansible.limit = "all"
       end
      end
    end
  end
end
