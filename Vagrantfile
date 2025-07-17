# -*- mode: ruby -*-
# vi: set ft=ruby :

# image = "generic-x64/alpine319"
image = "roboxes/ubuntu2204"

vms = [
  {:title => "r1", :ip => "192.168.22.100", :box => image, :router => false, :nets => [
    { :ip => "10.220.2.10", :name => "r-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.2.0" }
  ]},
  {:title => "r2", :ip => "192.168.22.101", :box => image, :router => false, :nets => [
    { :ip => "10.220.2.11", :name => "r-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.2.0" },
    { :ip => "10.220.3.10", :name => "r2-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.3.0" }
  ]},
  {:title => "r3", :ip => "192.168.22.102", :box => image, :router => false, :nets => [
    { :ip => "10.220.2.12", :name => "r-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.2.0" },
    { :ip => "10.220.4.10", :name => "r3-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.4.0" }
  ]},
  {:title => "r4", :ip => "192.168.22.103", :box => image, :router => false, :nets => [
    { :ip => "10.220.2.13", :name => "r-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.2.0" },
    { :ip => "10.220.5.10", :name => "r4-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.5.0" }
  ]},
  {:title => "r5", :ip => "192.168.22.104", :box => image, :router => false, :nets => [
    { :ip => "10.220.2.14", :name => "r-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.2.0" },
    { :ip => "10.220.6.10", :name => "r5-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.6.0" }
  ]},

  {:title => "pc1", :ip => "192.168.22.105", :box => image, :router => false, :nets => [
    { :ip => "10.220.3.11", :name => "r2-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.3.0" }
  ]},
  {:title => "pc2", :ip => "192.168.22.106", :box => image, :router => false, :nets => [
    { :ip => "10.220.4.11", :name => "r3-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.4.0" }
  ]},
  {:title => "pc3", :ip => "192.168.22.107", :box => image, :router => false, :nets => [
    { :ip => "10.220.5.11", :name => "r4-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.5.0" }
  ]},
  {:title => "pc4", :ip => "192.168.22.108", :box => image, :router => false, :nets => [
    { :ip => "10.220.6.11", :name => "r5-lan", :mask => "255.255.255.0", :maskSuf => "24", :net => "10.220.6.0" }
  ]}
]

Vagrant.configure("2") do |config|
  vms.each do |vmData|
    config.vm.define vmData[:title] do |cfg|
      cfg.vm.hostname = "#{vmData[:title]}"
      cfg.vm.box = vmData[:box]
      cfg.vm.network :private_network, :ip => vmData[:ip]
      vmData[:nets].each do |net|
        cfg.vm.network :private_network,
                       :ip => "#{net[:ip]}",
                       :libvirt__network_name => "#{net[:name]}",
                       :libvirt__netmask => "#{net[:mask]}"
      end

      cfg.vm.provider "libvirt" do |vb|
        vb.driver   = "kvm"
        vb.cpus     = 2
        vb.memory   = "4096"
        vb.storage :file, :size => '3G'
      end

#       cfg.vm.provision "shell", inline: <<-SCRIPT
# apk add --no-cache python3 py3-pip
#       SCRIPT
    end
  end
end

# sudo apk add wireguard-tools frr
# sudo touch /etc/frr/ospfd.conf.j2
# sudo chown frr:frr /etc/frr/ospfd.conf.j2
# sudo touch /etc/frr/vtysh.conf
# sudo chown frr:frr /etc/frr/vtysh.conf