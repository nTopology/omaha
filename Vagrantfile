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
  config.vm.define "host" do |host|
    config.vm.box = "Microsoft/EdgeOnWindows10"
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

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
  config.vm.define "host" do |host|

    # Share Omaha directory with the virtualbox
    host.vm.synced_folder "../omaha", "/omaha", type: "virtualbox"

    # Copy the contents into the virtualbox via rsync for faster build times
    # Refresh rsync folder while the box is still running with `vagrant rsync reload`
    # host.vm.synced_folder "../omaha", "/omaha_rsync",
    #   type: "rsync",
    #   rsync__exclude: ".git/",
    #   rsync__auto: true
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true

    host = RbConfig::CONFIG['host_os']
    return unless host.eql?('mingw32')

    # Customize the amount of memory on the VM:
    # Find total memory on the host machine and give 1/4 access
    host_mem_total = (`systeminfo |find "Total Physical Memory"`).to_i
    puts "Total host mem is #{host_mem_total}" unless host_mem_total = 0

    if host_mem_total = 0
      puts "Host mem is 0! Using defaul 8GB"
      host_mem_total = 8192
    end

    mem = host_mem_total.to_i/4
    puts "Using #{host_mem_total/4} memory"
    # Use half the cores available for building
    host_cpu_cores = (ENV['NUMBER_OF_PROCESSORS']).to_i
    cpus = host_cpu_cores.to_i/2
    puts "Using #{host_cpu_cores/2} cores"

    vb.customize ["modifyvm", :id, "--memory", mem]
    vb.customize ["modifyvm", :id, "--cpus", cpus]
  end

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
  config.vm.provision "shell", inline: <<-SHELL
    hammer
  SHELL
end
