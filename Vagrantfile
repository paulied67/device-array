# -*- mode: ruby -*-
# vi: set ft=ruby :

require File.join(File.dirname(__FILE__), 'lib/cpucount.rb')

# Check if we're running on Windows.
def windows?
	RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/
end

# Get VirtualBox's version string by capturing the output of 'VBoxManage -v'.
# Returns empty string if unable to determine version.
def get_virtualbox_version
	begin
		if windows?
			ver = `"%ProgramFiles%\\Oracle\\VirtualBox\\VBoxManage" -v 2>NULL`
		else
			ver = `VBoxManage -v 2>/dev/null`
		end
	rescue
		ver = ''
	end
	ver.gsub(/r.*/m, '')
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # For PostgreSql
  config.vm.network :forwarded_port,
    guest: 5432,
    host: 5432,
    auto_correct: true

  # For ApacheMQ web administration
  config.vm.network :forwarded_port,
    guest: 8161,
    host: 28161,
    auto_correct: true

  # For ApacheMQ JMX 
  config.vm.network :forwarded_port,
    guest: 1099,
    host: 21099,
    auto_correct: true

  # For ApacheMQ protocols 
  config.vm.network :forwarded_port,
    guest: 61616,
    host: 61616,
    id: 'tcp',
    # The auto_correct option set to true tells Vagrant to 
    # handle port collisions automatically.
    auto_correct: true

  config.vm.network :forwarded_port, 
    guest: 1833,
    host: 1833,
    id: 'mqtt',
    auto_correct: true

  config.vm.network :forwarded_port, 
    guest: 61613,
    host: 61613,
    id: 'stomp',
    auto_correct: true

  config.vm.network :forwarded_port, 
    guest: 61614,
    host: 61614,
    id: 'ws',
    auto_correct: true

  config.vm.network :forwarded_port, 
    guest: 5672,
    host: 5672,
    id: 'ampq',
    auto_correct: true

  # For Apollo 1.7.1
  config.vm.network :forwarded_port,
    guest: 61680,
    host: 61680,
    auto_correct: true

  config.vm.network :forwarded_port,
    guest: 61681,
    host: 61681,
    auto_correct: true


  #<connector id="tcp" bind="tcp://0.0.0.0:61613" connection_limit="2000"/>
  #<connector id="tls" bind="tls://0.0.0.0:61614" connection_limit="2000"/>
  #<connector id="ws"  bind="ws://0.0.0.0:61623"  connection_limit="2000"/>
  #<connector id="wss" bind="wss://0.0.0.0:61624" connection_limit="2000"/>

  config.ssh.forward_x11=true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port,, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

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
  config.vm.provider :virtualbox do |vb|
  # See http://www.virtualbox.org/manual/ch08.html for additional options.
     # Display the VirtualBox GUI when booting the machine
     #  vb.gui = true
     vb.customize ['modifyvm', :id, '--ostype', 'Ubuntu_64']
     # Use however many cpus are available on the host
     vb.customize ["modifyvm", :id, '--cpus', Cpu.count.to_s]
     # Customize the amount of memory on the VM:
     vb.customize ["modifyvm", :id, '--memory', '4096']
     #vb.customize ["modifyvm", :id, '--tracing-enabled', 'on']
     #vb.customize ["modifyvm", :id, '--nictrace1', 'on']
     #vb.customize ["modifyvm", :id, '--nictracefile1', 'c:\temp\nictrace1.pcap']
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end


#postgres$ vim /usr/local/pgsql/data/postgres.conf

# Change this `listen_address='localhost'` to
#listen_address='*'
#Save & exit.

#Enable access from local network

#postgres$ nano /usr/local/pgsql/data/pg_hba.conf

# Vagrant uses 33.33.33.10 like addresses
#host all all 33.33.33.0/24 trust