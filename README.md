## Silly little chef lab setup using Vagrant

#### Install everything you need for OS X via Homebrew

    brew cask install virtualbox
    brew cask install vagrant
    brew cask install chefdk

#### Install Vagrant plugin for Berkshelf:

    vagrant plugin install vagrant-berkshelf

#### Clone this repo and vagrant up:

    mkdir -p ~/vagrant_stuff
    cd !$
    git clone git@github.com:kholloway/cheflab.git
    cd cheflab
    vagrant init chef/centos-6.6
    vagrant up

You can now ```vagrant ssh``` to your instance and start playing around with Chef.

This setup installs the Chef base RPM along with the ChefDK RPM for CentOS/RHEL and any other packages you specify in the Vagrantfile

