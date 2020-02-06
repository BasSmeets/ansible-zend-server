# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.ssh.forward_agent = true
  config.ssh.insert_key = false
  config.vm.network :private_network, ip: "192.168.33.10"
  config.hostsupdater.aliases = [
    "zendserver.dev.dynacommerce.io",
	]

  config.vm.provider "virtualbox" do |v|
    v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    v.customize ["modifyvm", :id, "--memory", 4096]
    v.customize ["modifyvm", :id, "--cpus", 2]
    v.customize ["modifyvm", :id, "--name", "ose-zendserver"]
    v.linked_clone = true if Vagrant::VERSION =~ /^1.(8|9)/
  end

  config.vm.synced_folder ".", "/vagrant",
    :type => "rsync",
    :rsync__exclude => [ ".git/", ".vagrant/" ],
    :rsync__args => ["--verbose", "--rsync-path='sudo rsync'", "--archive", "-z"],
    :owner => "vagrant",
    :group => "vagrant",
    :mount_options => ["dmode=775", "fmode=664"]

  config.vm.provision :ansible_local do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.install_mode = "default"
  end

end