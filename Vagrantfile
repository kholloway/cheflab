# -*- mode: ruby -*-
# vi: set ft=ruby :

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

  config.vm.network "private_network", type: "dhcp"
  config.ssh.forward_agent = true

  config.vm.define "chefserver" do |chefserver|
    chefserver.vm.box = "chef/centos-6.6"
    config.vm.provision "chef_solo" do |chef|
      chef.version = "latest"

      config.berkshelf.enabled = true

      # Make sure Vim is installed
      chef.add_recipe "vim"
      chef.add_recipe "chef-dk"
      chef.add_recipe "chef-server"
    end
  end

  config.vm.define "chefclient" do |chefclient|
    chefclient.vm.box = "chef/centos-6.6"
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
