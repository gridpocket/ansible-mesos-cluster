# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  # Change bridge value with one of your bridge to avoid having to enter it manually for each machine creation
  # ex: bridge = "en0: Wi-Fi (AirPort)"
  bridge = nil

  config.vm.define "master1" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "public_network", ip:"192.168.1.211", bridge: bridge
  end
  config.vm.define "master2" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "public_network", ip:"192.168.1.212", bridge: bridge
  end
  config.vm.define "master3" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "public_network", ip:"192.168.1.213", bridge: bridge
  end
  config.vm.define "slave1" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "public_network", ip:"192.168.1.214", bridge: bridge
  end
  config.vm.define "slave2" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "public_network", ip:"192.168.1.215", bridge: bridge
  end
  config.vm.define "slave3" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "public_network", ip:"192.168.1.216", bridge: bridge
  end
end
