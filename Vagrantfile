# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|

     # Customize the amount of memory on the VM:
    vb.memory = "3000"
    vb.cpus = "2"
   end

  config.vm.define "node" do |node|
    node.vm.box = "ubuntu/xenial64"
    node.vm.hostname = "node"
    node.vm.network "private_network", ip: "10.0.3.16"
    node.vm.network "public_network"
    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get upgrade
      apt-get install \
      apt-transport-https \
      ca-certificates \
      curl \
      software-properties-common
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      apt-key fingerprint 0EBFCD88
      add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
      cat <<HERE >/etc/apt/sources.list.d/kubernetes.list
      deb http://apt.kubernetes.io/ kubernetes-xenial main
HERE
      apt-get update
      apt-get install -y docker-ce
      apt-get install -y kubelet kubeadm

    SHELL
  end
  config.vm.define "master" do |master|
    master.vm.box = "ubuntu/xenial64"
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "10.0.3.15"
    master.vm.network "public_network"
    master.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get upgrade
      apt-get install \
      apt-transport-https \
      ca-certificates \
      curl \
      software-properties-common
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      apt-key fingerprint 0EBFCD88
      add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
      cat <<HERE >/etc/apt/sources.list.d/kubernetes.list
      deb http://apt.kubernetes.io/ kubernetes-xenial main
HERE
      apt-get update
      apt-get install -y docker-ce
      apt-get install -y kubelet kubeadm

    SHELL
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.

end
