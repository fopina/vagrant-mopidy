# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "debian/jessie64"
  config.vm.box_check_update = false
  config.vbguest.auto_update = false if Vagrant.has_plugin? "vagrant-vbguest"

  config.vm.network "forwarded_port", guest: 6600, host: 9998
  config.vm.network "forwarded_port", guest: 6680, host: 9999

  config.vm.provider "virtualbox" do |vb|
    vb.name = "mopidy-dev"
    vb.linked_clone = true if Vagrant::VERSION =~ /^1.8/
    if RUBY_PLATFORM =~ /darwin/
      vb.customize ["modifyvm", :id, '--audio', 'coreaudio', '--audiocontroller', 'ac97']
    else
      vb.customize ["modifyvm", :id, '--audio', 'pulse', '--audiocontroller', 'ac97']
    end
  end

  # prevent "no tty" warnings during apt operations...
  config.vm.provision "fix-no-tty", type: "shell", privileged: false, inline: <<-SHELL
    sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile
  SHELL

  config.vm.provision "locale", type: "shell", path: "other/scripts/set_locale", args: ENV['LC_NAME']
  config.vm.provision "mopidy", type: "shell", path: "other/scripts/install_mopidy"
  config.vm.provision "extensions", type: "shell", path: "other/scripts/install_mopidy_extensions"
  config.vm.provision "start", type: "shell", path: "other/scripts/start_mopidy"

  config.vm.post_up_message = "
  mopidy up and running.
  MPD and HTTP ports forwarded.
  MPD on port 9998
  HTTP on port 9999. Go ahead and open http://localhost:9999/ in your browser.
  "
end
