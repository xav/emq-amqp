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
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 1883, host: 1883 # MQTT
  config.vm.network "forwarded_port", guest: 18083, host: 18083 # MQTT Dashboard
  config.vm.network "forwarded_port", guest: 5672, host: 5672 # AMQP
  config.vm.network "forwarded_port", guest: 15672, host: 15672 # AMQP Dashboard

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
  # config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.synced_folder '.', '/vagrant', type: "rsync", rsync__exclude: "priv", rsync__args: ["--verbose", "--archive", "--delete", "-z", "--copy-links", "-a", "-L"]
  # config.vm.synced_folder './_build/dev/rel', '/rel', type: "rsync", rsync__exclude: "priv", rsync__args: ["--verbose", "--archive", "--delete", "-z", "--copy-links", "-a", "-L"]

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    echo "LANG=en_US.utf-8" | sudo tee /etc/environment
    echo "LC_ALL=en_US.utf-8" | sudo tee /etc/environment

    yum -y update
    yum -y install epel-release
    # yum -y update
    yum -y install erlang socat
    erl -version

    rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
    curl -L -O https://www.rabbitmq.com/releases/rabbitmq-server/v3.6.10/rabbitmq-server-3.6.10-1.el7.noarch.rpm
    # curl -L -O https://dl.bintray.com/rabbitmq/all/rabbitmq-server/3.7.5/rabbitmq-server-3.7.5-1.el7.noarch.rpm
    sudo rpm -Uvh rabbitmq-server-3.6.10-1.el7.noarch.rpm
    systemctl start rabbitmq-server
    systemctl enable rabbitmq-server
    systemctl status rabbitmq-server

    yum -y install unzip
    curl -L -o emqttd-centos7-v2.2.0.zip http://emqtt.io/downloads/stable/centos7
    unzip emqttd-centos7-v2.2.0.zip

    curl -O https://s3.amazonaws.com/rebar3/rebar3 && chmod +x rebar3

    # curl -L -o emqttd-centos7.x86_64.rpm http://emqtt.com/downloads/latest/centos7-rpm
    # rpm -ivh emqttd-centos7.x86_64.rpm
    # systemctl start emqttd.service
    # systemctl enable emqttd.service
    # systemctl status emqttd.service
  SHELL
end
