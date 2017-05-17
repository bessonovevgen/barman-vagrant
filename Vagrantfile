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

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update = false

  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # Example for multi machine with array 
  # http://sysadm.pp.ua/linux/sistemy-virtualizacii/vagrantfile.html
  # http://help.ubuntu.ru/wiki/vagrant
  config.vm.define "pg-master" do |pgmaster|
#      pgmaster.vm.network  "nat_network", ip: "192.168.1.160"
      pgmaster.vm.hostname = "pg-master"
      pgmaster.vm.network "forwarded_port", guest: 5432, host: 5432
      pgmaster.vm.provider "virtualbox" do |vb|
         vb.memory = "2048"
         vb.name = "pg-master"
      end
   pgmaster.vm.provision "shell", inline: <<-SHELL
     sudo sh -c 'echo "deb http://1c.postgrespro.ru/deb/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/postgrespro-1c.list'
     wget --quiet -O - http://1c.postgrespro.ru/keys/GPG-KEY-POSTGRESPRO-1C | sudo apt-key add - && sudo apt-get update 
     sudo apt-get install -y locales wget  
     sudo locale-gen ru_RU.utf8
     sudo update-locale LANG=ru_RU.UTF8
     sudo apt-get install -y postgresql-pro-1c-9.6 htop mc atop
     sudo service postgresql start
     sudo -u postgres psql -c "alter user postgres with password 'postgres';"
   SHELL
  end

  config.vm.define "barman-host" do |barmanhost|
#      barmanhost.vm.network  "public_network", ip: "192.168.1.160"
      barmanhost.vm.hostname = "barman-host"
#      barmanhost.vm.network "forwarded_port", guest: 22, host: 18022
      barmanhost.vm.provider "virtualbox" do |vb|
         vb.memory = "2048"
         vb.name = "barman-host"
      end
   barmanhost.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update 
     sudo apt-get install -y htop mc atop barman
   SHELL
  end
  

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end
