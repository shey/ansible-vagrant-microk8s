# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/jammy64"

  config.ssh.insert_key = true
  config.ssh.forward_agent = true

  ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
  config.vm.provision "shell", inline: <<-SHELL
    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
  SHELL

  # General VirtualBox VM configuration.
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--cpus", 2]
    v.customize ["modifyvm", :id, "--memory", 1024]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "primary" do |primary|
    primary.vm.hostname = "vagrant-primary"
    primary.vm.network :private_network, ip: "192.168.56.21"
  end

  config.vm.define "node01" do |node01|
    node01.vm.hostname = "vagrant-node01"
    node01.vm.network :private_network, ip: "192.168.56.22"
  end

  config.vm.define "node02" do |node02|
    node02.vm.hostname = "vagrant-node02"
    node02.vm.network :private_network, ip: "192.168.56.23"
  end

end
