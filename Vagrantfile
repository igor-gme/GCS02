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
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/trusty64"

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    # NOTE: This will enable public access to the opened port
    db.vm.network "forwarded_port", guest: 5432, host: 2432

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    db.vm.network "private_network", ip: "192.168.1.11"

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network "public_network"

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    # config.vm.synced_folder "../data", "/vagrant_data

    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    db.vm.provider "virtualbox" do |vb1|
      vb1.name = "dj-databases"

      # Display the VirtualBox GUI when booting the machine
      vb1.gui = false

      #   # Customize the amount of memory on the VM:
      vb1.memory = "512"

      vb1.cpus = 1
    end

    # Enable provisioning with a shell script. Additional provisioners such as
    # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
    # documentation for more information about their specific syntax and use.

    db.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y vim git
      sudo apt-get install -y postgresql
      sudo su - postgres -c "psql -f /vagrant/db-config.sql"
      sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
      sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
      sudo service postgresql restart
    SHELL
  end


# Web
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/trusty64"
    web.vm.network "forwarded_port", guest:8000, host:8000
    web.vm.network "private_network", ip: "192.168.1.10"

    web.vm.provider "virtualbox" do |vb2|
      vb2.name = "dj"
      vb2.gui = false
      vb2.memory = "512"
      vb2.cpus = 1
    end

    web.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y vim git
      sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      sudo pip install django flake8 psycopg2
    SHELL
  end

end
