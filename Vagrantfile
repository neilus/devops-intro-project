# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. 

Vagrant.configure(2) do |config|

  config.vm.define "app" do |app|
    app.vm.box = "devops/appserver"
    app.vm.hostname = "app.devops.net"
    app.vm.network "forwarded_port", guest: 80, host: 8080
    app.vm.synced_folder "./devops-kungfu", "/home/vagrant/devops-kungfu", create: true
  end

  config.vm.define "control" do |ctrl|
    ctrl.vm.box = "devops/controlserver"
    ctrl.vm.hostname = "control.devops.net"
    ctrl.vm.network "forwarded_port", guest: 80, host: 8880
    ctrl.vm.synced_folder "./jenkins-config", "/home/vagrant/jenkins-config", create: true
  end

end
