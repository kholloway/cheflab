# -*- mode: ruby -*-
# vi: set ft=ruby :
#

Vagrant.configure(2) do |config|
  config.vm.box = "chef/centos-6.6"

  # Bootstrap for the vim cookbook
  #   mkdir -p ~/vagrant_stuff/cheflab/cookbooks
  #   cd !$
  #   git clone git://github.com/opscode-cookbooks/vim.git
  #
  # Now put this Vagrantfile in ~/vagrant_stuff/cheflab/Vagrantfile
  # and run 'vagrant up'
  # You should end up with Chef installed on the VM (version latest) and VIM package installed via Yum in this case

  #config.vm.network "private_network", type: "dhcp"
  #config.ssh.private_key_path = "~/.ssh/id_rsa"
  config.ssh.forward_agent = true

  # Host manager plugin
  # vagrant plugin install vagrant-hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  # Chef server machine
  config.vm.define "chefserver", primary: true do |node|
    node.vm.hostname = "chefserver"
    node.vm.network :private_network, ip: '172.28.128.10'
    node.hostmanager.aliases = %w(chefserver.test.dev)
    node.vm.box = "chef/centos-6.6"
    config.vm.provision "chef_solo" do |chef|
      chef.json = {
        :chef-server => { :api_fqdn => "chefserver.test.dev" }
      }
      chef.version = "latest"
      config.berkshelf.enabled = true
      # Make sure Vim is installed
      chef.add_recipe "vim"
      chef.add_recipe "chef-dk"
      chef.add_recipe "chef-server"
    end
  end

  # Chef client machine
  config.vm.define "chefclient" do |node|
    node.vm.hostname = "chefclient"
    node.vm.network :private_network, ip: '172.28.128.11'
    node.hostmanager.aliases = %w(chefclient.test.dev)
    node.vm.box = "chef/centos-6.6"
    # This should install chef omnibus packages
    config.vm.provision "chef_solo" do |chef|
      chef.version = "latest"
      config.berkshelf.enabled = true
      # Make sure Vim is installed
      chef.add_recipe "vim"
      chef.add_recipe "chef-dk"
    end
  end
end
