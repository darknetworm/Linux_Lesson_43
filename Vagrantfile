# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :source => {
        :box_name => "ubuntu/focal64",
        :vm_name => "source",
        :net => [
          {ip: '192.168.11.150', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "mysql-net"},
          {ip: '192.168.56.10', adapter: 8},
        ]
  },
  :replica => {
        :box_name => "ubuntu/focal64",
        :vm_name => "replica",
        :net => [
          {ip: '192.168.11.151', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "mysql-net"},
          {ip: '192.168.56.11', adapter: 8},
        ]
  }
}
Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxname.to_s
      boxconfig[:net].each do |ipconf|
        box.vm.network "private_network", ipconf
      end
      if boxconfig[:vm_name] == "replica"
        box.vm.provision "ansible" do |ansible|
          ansible.playbook = "provision.yml"
          ansible.inventory_path = "hosts"
          ansible.host_key_checking = "false"
          ansible.limit = "all"
        end
      end
    end
  end
end
