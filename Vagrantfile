# coding: utf-8
# copyright Utrecht University
# -*- mode: ruby -*-
# vi: set ft=ruby :


# Configuration variables.
VAGRANTFILE_API_VERSION = "2"

BOX = 'centos/7'
GUI = false
CPU = 1
RAM = 1024

NETWORK = "192.168.51."
NETMASK = "255.255.255.0"

HOSTS = { "192.168.51.10"  => [ "192.168.51.10", CPU, RAM, GUI, BOX ] }

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.insert_key = false

  HOSTS.each do | (name, cfg) |
    ipaddr, cpu, ram, gui, box = cfg

    config.vm.define name do |machine|
      machine.vm.box = box

      machine.vm.provider "virtualbox" do |vbox|
        vbox.gui    = gui
        vbox.cpus   = cpu
        vbox.memory = ram
        vbox.name   = name
        vbox.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
      end

      machine.vm.hostname = "dokuwiki.local"
      machine.vm.network 'private_network', ip: ipaddr, netmask: NETMASK
      machine.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.provision "shell", privileged: true, inline: "hostnamectl set-hostname dokuwiki.local"
    config.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.inventory_path = "vagrant/config"
    end
 
    end
  end
end
