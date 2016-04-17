# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?("landrush")
    config.landrush.enabled = true
    config.landrush.tld = 'dev'
  end

  config.vm.hostname = "agency-loan.dev"

  config.vm.box = "ubuntu/trusty64"
  config.vm.network :private_network, ip: "192.168.56.163"
  config.vm.synced_folder ".", "/vagrant", nfs: true

  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.customize ["modifyvm", :id, "--name", "agency loan"]
    virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    virtualbox.memory = 2048
    virtualbox.cpus = 2
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision/agency-loan.yml"
    ansible.inventory_path = "provision/hosts"

    # bug: https://github.com/maxehmookau/neo4j-ansible/issues/3
    ansible.limit = 'all'

    # Enable verbosity while debugging (http://docs.vagrantup.com/v2/provisioning/ansible.html)
    # ansible.verbose = 'vvvv'
  end

  config.vagrant.host = :detect
end
