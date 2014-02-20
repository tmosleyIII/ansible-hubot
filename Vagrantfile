# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile for bootstrapping a Hubot instance on Mac OS X with Vagrant
# provider and Ansible provisioiner on Ubuntu virtual machine

VAGRANTFILE_API_VERSION = "2"
BOX_MEM = ENV['BOX_MEM'] || "1536"
BOX_NAME =  ENV['BOX_NAME'] || "salamander64"
BOX_URI = ENV['BOX_URI'] || "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-13.10_chef-provisionerless.box"
BOT_HOST = ENV['BOT_HOST'] || "vagrant_hosts"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :hubot do |hubot_config|
    hubot_config.vm.box = BOX_NAME
    hubot_config.vm.box_url = BOX_URI
    hubot_config.vm.network :private_network, ip: "10.1.1.42"
    hubot_config.vm.hostname = "hubot.local"
    hubot_config.ssh.forward_agent = true
    hubot_config.vm.provider "virtualbox" do |v|
      v.name = "hubot"
      v.customize ["modifyvm", :id, "--memory", BOX_MEM]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "1"]
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end
    hubot_config.vm.provision :ansible do |ansible|
      ansible.inventory_path = BOT_HOST
      ansible.playbook = "site.yml"
    end
  end
end

