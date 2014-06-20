# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.
  
  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true
  
  config.vm.define :postgres do |postgres|
	# Every Vagrant virtual environment requires a box to build off of.
	postgres.vm.box = "opscode_ubuntu-12.04_chef-provisionerless"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
	postgres.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-12.04_chef-provisionerless.box"
 
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
	postgres.vm.network :forwarded_port, guest: 5432, host: 5432

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
   postgres.vm.network :private_network, ip: "192.168.33.12"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   postgres.vm.provider :virtualbox do |vb|
     # boot with headless mode
     vb.gui = false
     vb.name = "PostgreSQL"
     # Use VBoxManage to customize the VM. For example to change memory:
     vb.customize ["modifyvm", :id, "--memory", "1024"]
   end
  #
  # View the documentation for the provider you're using for more
  # information on available options.
  
  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  postgres.vm.provision :chef_solo do |chef|
	 chef.add_recipe "apt"
	 chef.add_recipe "postgresql"
	 chef.add_recipe "postgresql::server"
	 chef.add_recipe "database"
	 chef.add_recipe "build-essential"
		
	postgresql = {	
	  "listen_addresses" => "*",#don't go into production like this
	  "pg_hba" =>  [
		{ "type" =>  "local", "db" =>  "all", "user" =>  "postgres",   "addr" =>  "",             "method" =>  "ident" },
		{ "type" =>  "local", "db" =>  "all", "user" =>  "all",        "addr" =>  "",             "method" =>  "trust" },
		{ "type" =>  "host",  "db" =>  "all", "user" =>  "all",        "addr" =>  "127.0.0.1/32", "method" =>  "trust" },
		{ "type" =>  "host",  "db" =>  "all", "user" =>  "all",        "addr" =>  "::1/128",      "method" =>  "trust" },
		{ "type" =>  "host",  "db" =>  "all", "user" =>  "postgres",   "addr" =>  "127.0.0.1/32", "method" =>  "trust" },
		{ "type" =>  "host",  "db" =>  "all", "user" =>  "username",   "addr" =>  "127.0.0.1/32", "method" =>  "trust" },
		{ "type" => "host", "db" => "all", "user" => "all", "addr" => "0.0.0.0/0", "method" => "trust" }, #don't go into production like this
        { "type" => "host", "db" => "all", "user" => "all", "addr" => "::1/0", "method" => "trust" } #don't go into production like this
	  ],
	  "databases" =>  [
		{
		  "name" =>  "tomee",
		  "owner" =>  "tomee",
		  "template" =>  "template0",
		  "encoding" =>  "utf8",
		  "locale" =>  "en_US.UTF8"
		}
	  ],
	  "users" =>  [
		{
			"username" => "postgres",
			"password" => "pr0gr3ss",
			"superuser" => true,
			"createdb" => true,
			"login" => true
		},
		{
		  "username" =>  "tomee",
		  "password" =>  "tomee",
		  "superuser" =>  false,
		  "createdb" =>  false,
		  "login" =>  true
		}
	 ]
	
	}
	
     chef.json["postgresql"] = postgresql
	end
end
  
  
end
