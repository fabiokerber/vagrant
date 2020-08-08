# -*- mode: ruby -*-
# vi: set ft=ruby :

machines = {
  "servidor" => {"memory" => "4096", "cpu" => "4", "ip" => "10", "image" => "centos/7"}
}

Vagrant.configure("2") do |config|

  config.vm.box_check_update = false
  config.vm.boot_timeout = 600
  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}"
      machine.vm.network "private_network", ip: "192.168.99.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        vb.customize ["modifyvm", :id, "--groups", "/Samba"]
	vb.customize ["storageattach", :id,
                "--storagectl", "IDE",
                "--port", "0", "--device", "1",
                "--type", "dvddrive",
                "--medium", "/usr/share/virtualbox/VBoxGuestAdditions.iso"]
      end
    end
  end

#SAMBA
  config.vm.define "servidor" do |servidor|
    servidor.vm.network "forwarded_port", guest: 137, host: 137
    servidor.vm.network "forwarded_port", guest: 138, host: 138
    servidor.vm.network "forwarded_port", guest: 139, host: 139
    servidor.vm.network "forwarded_port", guest: 445, host: 445

  servidor.vm.provision "shell", inline: <<-SHELL
    yum update -y
    yum install kernel-devel kernel-headers gcc make perl -y
  SHELL

end

end
