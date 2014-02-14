# -*- mode: ruby -*-
# vi: set ft=ruby :

# vagrant plugin install vagrant-lxc
# vagrant plugin install vagrant-berkshelf
# vagrant plugin install vagrant-omnibus

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

  config.vm.define "packer" do |packer|
    # if provider=lxc
    # vagrant lxc + docker seems broken
    # packer.vm.box = "vagrant-lxc-raring64-2013-10-23"
    # packer.vm.box_url = "http://bit.ly/vagrant-lxc-raring64-2013-10-23"

    packer.vm.box = "raring-server-cloudimg-amd64-vagrant-disk1"
    packer.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"

    # follow sym links for the shared folder
    # https://github.com/mitchellh/vagrant/issues/713
    packer.vm.provider :virtualbox do |v|
      v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/vagrant", "1"]
    end

    # install aufs support
    # http://docs.docker.io/en/latest/installation/ubuntulinux/#ubuntu-raring-13-04-and-saucy-13-10-64-bit
    packer.vm.provision :shell, :inline => "apt-get update && apt-get install linux-image-extra-`uname -r` -y"

    packer.vm.provision :chef_solo do |packer_chef|
      packer_chef.roles_path = "chef/roles"
      packer_chef.run_list = [
        "role[packer]",
      ]
    end

    packer.vm.provision :shell, :inline => "usermod -a -G docker vagrant"
  end
end
