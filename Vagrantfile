# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  # Copy angel.service, and angel.conf
  config.vm.provision "file", source: "angel.service", destination: "/home/vagrant/angel.service"
  config.vm.provision "file", source: "angel.conf", destination: "/home/vagrant/angel.conf"  
  
  # Share the current folder with the guest OS.
  config.vm.synced_folder ".", "/home/vagrant/app"

  # Run provisioning script.
  config.vm.provision "shell", path: "provision.sh"

  # Run user-provisioning script.
  config.vm.provision "shell", path: "user-provision.sh", privileged: false, keep_color: true

  # Start the service, after running `pub get`
  config.vm.provision "shell", inline: "service angel start"
end
