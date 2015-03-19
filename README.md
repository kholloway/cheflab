## Silly little chef lab setup using Vagrant

#### Install everything you need for OS X via Homebrew

    brew cask install virtualbox
    brew cask install vagrant
    brew cask install chefdk

#### Install Vagrant plugins:

    vagrant plugin install vagrant-omnibus
    vagrant plugin install vagrant-chef-zero
    vagrant plugin install vagrant-berkshelf
    vagrant plugin install vagrant-hostmanager

#### Clone this repo and vagrant up:

    mkdir -p ~/vagrant_stuff
    cd !$
    git clone git@github.com:kholloway/cheflab.git
    cd cheflab
    vagrant up

You can now ```vagrant ssh``` to your instance and start playing around with Chef.

This setup installs the Chef base RPM along with the ChefDK RPM for CentOS/RHEL and any other packages you specify in the Vagrantfile

#### Fix for Virtualbox networking error

If you get an error message like the one below:

```
A host only network interface you're attempting to configure via DHCP
already has a conflicting host only adapter with DHCP enabled.
```

Then run the command below list and then remove your host interfaces, ideally you should remove all of them.
I ended up with one that looks like this:

```
NetworkName:    HostInterfaceNetworking-vboxnet0
IP:             172.28.128.2
NetworkMask:    255.255.255.0
lowerIPAddress: 172.28.128.3
upperIPAddress: 172.28.128.254
Enabled:        Yes
```

After that do a ```vagrant destroy``` followed by a ```vagrant up``` to get things working again.



