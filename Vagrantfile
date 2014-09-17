# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box       = "precise32"
  config.vm.box_url   = "http://files.vagrantup.com/precise32.box"
  config.vm.host_name = "vagrant-nrportal"
  config.vm.network "private_network", ip: "10.11.12.13"
end
