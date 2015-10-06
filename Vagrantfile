# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--nictype1", "virtio", "--nicpromisc1", "allow-all"]
    v.customize ["modifyvm", :id, "--nictype2", "virtio", "--nicpromisc2", "allow-all"] 
    v.customize ["modifyvm", :id, "--nictype3", "virtio", "--nicpromisc3", "allow-all"] 
    v.customize ["modifyvm", :id, "--nictype4", "virtio", "--nicpromisc4", "allow-all"] 
  end
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "jayunit100/centos7"
  config.vbguest.auto_update = false
  config.vm.define "syslog-ng" do |sl|
    sl.vm.hostname = "syslog-ng"
    sl.vm.network "private_network", virtualbox__intnet: 'swp1', ip: "192.168.100.2", nictype: "virtio", :adapter => 2
    sl.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end

  config.vm.box = "jayunit100/centos7"
  config.vbguest.auto_update = false
  config.vm.define "graylog" do |gl|
    gl.vm.hostname = "graylog"
    gl.vm.network "private_network", virtualbox__intnet: 'swp1', ip: "192.168.100.3", nictype: "virtio", :adapter => 2
    gl.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end

  config.vm.define "client" do |cl|
    cl.vm.hostname = "client"
    cl.vm.network "private_network", virtualbox__intnet: 'swp2', ip: "192.168.100.3", nictype: "virtio", :adapter => 2
    cl.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end

  config.vm.define "switch" do |sw|
    sw.vm.hostname = "switch"
    sw.vm.box = "CumulusVX-2.5.3-4"
    # Internal network for swp* interfaces.
    sw.vm.network 'private_network', virtualbox__intnet: 'swp1', cumulus__intname: 'swp1', :adapter => 2
    sw.vm.network 'private_network', virtualbox__intnet: 'swp2', cumulus__intname: 'swp2', :adapter => 3
    sw.vm.network 'private_network', virtualbox__intnet: 'swp3', cumulus__intname: 'swp3', :adapter => 4
    sw.vm.network 'private_network', virtualbox__intnet: 'swp4', cumulus__intname: 'swp4', :adapter => 5
    # Internal management network
    sw.vm.network 'private_network', virtualbox__intnet: 'mgmt', cumulus__intname: 'eth4', type: 'dhcp', :adapter => 6
    
    sw.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end
end
