# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# <ubuntu/trusty64|bento/ubuntu-16.04
OS_IMAGE = "bento/ubuntu-16.04"

SERVERS = ["mongo1", "mongo2", "mongo3"]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use the same key for each machine
  config.ssh.insert_key = false

  N = 3

  (1..N).each do |node_id|
    nid = (node_id - 1)

    config.vm.define SERVERS[nid] do |mongo|
      # https://atlas.hashicorp.com/ubuntu/
      mongo.vm.hostname = SERVERS[nid]
      mongo.vm.box = OS_IMAGE
      mongo.vm.network "private_network", ip: "192.168.33.#{20 + nid}"
      mongo.ssh.forward_agent = true
      
      mongo.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 1
      end
    end
  end
end