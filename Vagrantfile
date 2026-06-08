# -*- mode: ruby -*-
# vi: set ft=ruby :
NETWORK_BASE = "192.168.47"
NODES = [
  { name: "test",  ip: "#{NETWORK_BASE}.10", memory: 8192, cpus: 4 },
]

Vagrant.configure("2") do |config|
  config.vm.box = "packer-ubuntu-cloud-24.04"
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  NODES.each do |node|
    config.vm.define node[:name] do |vm_cfg|
      config.vm.cloud_init do |cloud_init|
        cloud_init.content_type = "text/cloud-config"
        cloud_init.inline = <<-EOF
          #cloud-config
          timezone: Asia/Tehran
          package_update: true
        EOF
      end
      vm_cfg.vm.hostname = node[:name]
      vm_cfg.vm.network "private_network", ip: node[:ip]
      vm_cfg.vm.provider "libvirt" do |lv|
        lv.title  = node[:name]
        lv.driver = "kvm"
        lv.uri    = "qemu:///system"
        lv.memory = node[:memory]
        lv.cpus   = node[:cpus]
      end
    end
  end
end
