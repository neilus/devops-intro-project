# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below.

Vagrant.configure(2) do |config|

  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/trusty64" #"devops/app"
    app.vm.hostname = "app.devops.net"
    app.vm.network "forwarded_port", guest: 80, host: 8080
    app.vm.network "private_network", ip: "192.168.42.41"
    app.vm.synced_folder "./devops-kungfu",
      "/home/vagrant/devops-kungfu",
      create: true
    app.vm.provision "shell", name: "update",
      path: "./packer-templates/scripts/update.sh"
    app.vm.provision "shell", name: "application",
      path: "./packer-templates/scripts/application.sh"
#    app.vm.provision "shell", name: "cleanup",
#      path: "./packer-templates/scripts/cleanup.sh"
  end

  config.vm.define "control" do |ctrl|
    ctrl.vm.box = "ubuntu/trusty64" #"devops/control"
    ctrl.vm.hostname = "control.devops.net"
    ctrl.vm.network "forwarded_port", guest: 80, host: 8880
    ctrl.vm.network "private_network", ip: "192.168.42.42"
    ctrl.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
    ctrl.vm.synced_folder "./jenkins-config",
      "/home/vagrant/jenkins-config",
      create: true
    config.vm.provision "shell", name: "update",
      path: "./packer-templates/scripts/update.sh"
    config.vm.provision "file",
      source: "./jenkins-config",
      destination: "/tmp"
    config.vm.provision "shell", name: "jenkins-setup",
      path: "./packer-templates/scripts/jenkins.sh"
    config.vm.provision "shell", name: "graphite-setup",
      path: "./packer-templates/scripts/graphite.sh"
    config.vm.provision "shell", name: "nginx-setup",
      path: "./packer-templates/scripts/nginx.sh"
#    config.vm.provision "shell", name: "cleanup",
#      path: "./packer-templates/scripts/cleanup.sh"
    config.vm.provision "shell", name: "jenkins-update",
      run: 'never',
      privileged: true,
      inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -qqy --force-yes jeknins
        sudo service jenkins restart
    SHELL
  end

end
