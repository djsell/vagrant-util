# -*- mode: ruby -*-
# vi: set ft=ruby :

# vagrant plugin install vagrant-lxc
# vagrant plugin install vagrant-berkshelf
# vagrant plugin install vagrant-omnibus

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.berkshelf.enabled = true
  #config.omnibus.chef_version = :latest

  config.vm.define "packer" do |packer|
    # if provider=lxc
    # vagrant lxc + docker seems broken
    # packer.vm.box = "vagrant-lxc-raring64-2013-10-23"
    # packer.vm.box_url = "http://bit.ly/vagrant-lxc-raring64-2013-10-23"

    packer.vm.box = "raring-server-cloudimg-amd64-vagrant-disk1"
    packer.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"

    packer.vm.provision :chef_solo do |packer_chef|
      packer_chef.roles_path = "chef/roles"
      packer_chef.run_list = [
        "role[packer]",
      ]
    end
  end
end
