## To setup Knife/Chef client on client node

Do all this as the Vagrant user.

Create base dir and trusted_certs dir:

    mkdir -p ~/.chef/trusted_certs

Fetch the currently not trusted SSL cert from the server:

    knife ssl fetch https://chefserver.test.dev:443

Configure Knife:

```
[vagrant@chefclient]$ knife configure --initial
Overwrite /home/vagrant/.chef/knife.rb? (Y/N) y
Please enter the chef server URL: [https://chefclient:443] https://chefserver.test.dev:443
Please enter a name for the new user: [vagrant]
Please enter the existing admin name: [admin]
Please enter the location of the existing admins private key: [/etc/chef-server/admin.pem] /home/vagrant/.chef/admin.pem
Please enter the validation clientname: [chef-validator]
Please enter the location of the validation key: [/etc/chef-server/chef-validator.pem] /home/vagrant/.chef/chef-validator.pem
Please enter the path to a chef repository (or leave blank):
Creating initial API user...
Please enter a password for the new user:
Created user[vagrant]
Configuration file written to /home/vagrant/.chef/knife.rb
```

Resulting Knife.rb config:

```
log_level                :info
log_location             STDOUT
node_name                'vagrant'
client_key               '/home/vagrant/.chef/vagrant.pem'
validation_client_name   'chef-validator'
validation_key           '/home/vagrant/.chef/chef-validator.pem'
chef_server_url          'https://chefserver.test.dev:443'
syntax_check_cache_path  '/home/vagrant/.chef/syntax_check_cache'
```

Make sure knife commands work:

    knife user list

Should return some users instead of some error message


Setup base repo:

    cd ~
    chef generate repo chef-repo
    cd chef-repo
    git init .

Download a cookbook from the Opscode Chef site:

    cd ~
    knife cookbook site download vim
    cd chef-repo/cookbooks
    tar -zxvf ~/vim*.tar.gz

Upload the VIM cookbook to your local Chef server:

    knife upload --chef-repo-path ~/chef-repo cookbooks/



Setup your Chef-client (as root):

    cd /etc/chef
    cp /home/vagrant/.chef/chef-validator.pem /etc/chef/validation.pem
    cp -aR /home/vagrant/.chef/trusted_keys /etc/chef/

Create your base client.rb file for Chef-client to use:

    vim /etc/chef/client.rb

Contents:

````
log_level                :info
log_location             STDOUT
chef_server_url          'https://chefserver.test.dev:443'
````




