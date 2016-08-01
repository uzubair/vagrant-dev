# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version.
VAGRANTFILE_API_VERSION = "2"
VAGRANT_MACHINE_IP = "192.168.50.100"

Vagrant.require_version '>= 1.7.2'

if !Vagrant.has_plugin?("vagrant-docker-compose")
  raise "Missing plugin! Run `vagrant plugin install vagrant-docker-compose`."
end

def mount_folder(server, source, target, create = false)
  if Vagrant::Util::Platform.windows? || ENV['VAGRANT_RSYNC']
    server.vm.synced_folder source, target, create: create, type: "rsync",
      rsync__args: ["--verbose", "--archive", "--delete", "-z"],
      rsync__auto: true,
      rsync__exclude: [".git/", ".vagrant/", ".tmp/", "node_modules/", "logs/"]
  else
    server.vm.synced_folder source, target, create: create, type: "nfs"
  end
end

$script = <<SCRIPT
  sudo apt-get update
  sudo apt-get -y install vim git curl
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "phusion/ubuntu-14.04-amd64"
  config.vm.box_check_update = false

  config.vm.define :dev do |server|
    server.vm.hostname = "dev"
    server.vm.network "forwarded_port", guest: 80, host: 9090

    # Private network, which allows host-only access to the machine
    # using a specific IP.
    server.vm.network "private_network", ip: "#{VAGRANT_MACHINE_IP}"

    server.vm.provision "docker"
    server.vm.provision :docker_compose, compose_version: '1.5.2'
    server.vm.provision "shell", run: "once", inline: $script

    server.vm.provider "virtualbox" do |vb|
       vb.name = "dev"
       vb.cpus = 2
    
       vb.memory = "4096"
    end

    # Remove default synced folder
    server.vm.synced_folder ".", "/vagrant", disabled: true

    # Mount directories
    mount_folder server, "/home/username/workspace/git", "/home/vagrant/git", true
  end    
end