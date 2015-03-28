# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

if ENV['PROJECT']
  project_name = ENV['PROJECT']
elsif File.exists? (".tmp/config.yml")
  vconfig = YAML::load_file(".tmp/config.yml")
  project_name = vconfig['project']
else
  project_name = 'site'
end

ip_address = "10.1.1.10"

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
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.box = "ubuntu/trusty64"
  config.vm.synced_folder ".", "/var/www",
                          mount_options: ['dmode=777','fmode=777'],
                          owner: "www-data",
                          group: "www-data"

  config.vm.define project_name do |node|
    node.vm.hostname = project_name + ".local"
    node.vm.network :private_network, ip: ip_address
    node.vm.network :forwarded_port, host: 8080, guest: 80
  end

  config.vm.provision :hostmanager

  # Deploy app code
  config.vm.provision :shell, :path => "env/bootstrap", :args => "#{project_name}"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "768"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
end
