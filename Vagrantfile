# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "debian/jessie64"
  config.vm.box_check_update = false

  config.vm.provision "fix-no-tty", type: "shell", privileged: false, inline: <<-SHELL
    sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile
  SHELL

  config.vm.provider "virtualbox" do |vb|
    vb.name = "mopidy-dev"
    vb.linked_clone = true if Vagrant::VERSION =~ /^1.8/
  end

  config.vm.post_up_message = "
  mopidy up and running
  "
end
