# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
HOST_NAME = "UbuntuQeoSDK"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu-14.04"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  
  
  config.vm.host_name = HOST_NAME

	# Boot with a GUI so you can see the screen. (Default is headless)
	# config.vm.boot_mode = :gui
  config.vm.provider "virtualbox" do |v|
    v.name = HOST_NAME
    v.gui = true
    v.memory = 2048
	v.cpus = 2
	v.customize ["modifyvm", :id, "--vram", "80"]
  end
  
  
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network :forwarded_port, guest: 5037, host: 5037, auto_correct: true # Android ADB port
  

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.  
  config.berkshelf.enabled = true
  config.berkshelf.berksfile_path = "Berksfile"
  
  config.omnibus.chef_version = :latest
  
  config.vm.provision :chef_solo do |chef|
    
    chef.add_recipe "apt"
    chef.add_recipe "git"
    chef.add_recipe "build-essential" 
    chef.add_recipe "java" 
    chef.add_recipe "vim"

  end

  config.vm.provision "shell", inline: "echo Installing Android ADT bundle and NDK..."
  config.vm.provision "shell", path: "provision.sh"
  config.vm.provision "shell", inline: "echo Installation complete"
  
  config.vm.synced_folder "./", "/vagrant", owner: 'vagrant', group: 'vagrant'
end
