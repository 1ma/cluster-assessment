# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Machine-specific provider-specific config (names)
  config.vm.define "kv2" do |machineconfig|
    machineconfig.vm.provider "virtualbox" do |vb|
      vb.name = "kv2"
    end
    machineconfig.vm.network "private_network", ip: "192.168.33.10"
  end

  config.vm.define "c0-master" do |machineconfig|
    machineconfig.vm.provider "virtualbox" do |vb|
      vb.name = "c0-master"
    end
    machineconfig.vm.network "private_network", ip: "192.168.33.11"
  end

  config.vm.define "c0-n1" do |machineconfig|
    machineconfig.vm.provider "virtualbox" do |vb|
      vb.name = "c0-n1"
    end
    machineconfig.vm.network "private_network", ip: "192.168.33.12"
  end

  config.vm.define "c0-n2" do |machineconfig|
    machineconfig.vm.provider "virtualbox" do |vb|
      vb.name = "c0-n2"
    end
    machineconfig.vm.network "private_network", ip: "192.168.33.13"
  end

  # Common provider-specific config
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
    vb.cpus = 1
    vb.customize ["modifyvm", :id, "--vram", "8"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    sudo sh -c 'echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list'
    sudo apt-get update
    sudo apt-get -y install linux-image-extra-$(uname -r)
    sudo apt-get -y install docker-engine
    usermod -a -G docker vagrant   ## add vagrant user to docker group
    sudo sh -c 'echo DOCKER_OPTS=\\"--cluster-store=consul://192.168.33.10:8500 --cluster-advertise=eth1:2375 -H=tcp://0.0.0.0:2375 -H=unix:///var/run/docker.sock\\" >> /etc/default/docker'
    sudo service docker restart
  SHELL
end
