# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
    config.vm.box_check_update = "false"
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
    end

    config.vm.define "smarthome" do |sb|
        sb.vm.hostname = "smarthome.local"
        sb.vm.box = "ubuntu/bionic64"
        sb.vm.network :private_network, ip: "10.0.0.20"
    end
end