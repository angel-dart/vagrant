# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  
  # Install Dart, nginx
  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y apt-transport-https
    sh -c 'curl https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -'
    sh -c 'curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list'
    apt-get update
    apt-get install -y dart
    apt-get install -y nginx
  SHELL

  # Create a privileged user, named `ops`
  # Also create an unprivileged `web` account
  config.vm.provision "shell", inline: <<-SHELL
    adduser --disabled-password --gecos "" ops
    adduser --disabled-password --gecos "" web
    usermod -aG sudo ops
  SHELL

  # Share the current folder with the guest OS, owned by web
  config.vm.synced_folder "..", "/home/web/app", group: "web", owner: "web"

  # Copy angel.service, and angel.conf
  config.vm.provision "file", source: "angel.service", destination: "/etc/systemd/system/angel.service"
  config.vm.provision "file", source: "angel.conf", destination: "/etc/nginx/sites-enabled/angel.conf"

  # Start the service
  config.vm.provision "shell", inline: <<-SHELL
    systemctl daemon-reload
    service angel start
  SHELL
end
