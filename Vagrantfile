# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir = File.dirname(File.expand_path(__FILE__))
begin
  config = YAML.load_file("#{current_dir}/config.yaml")
  config = {} unless config.is_a?(Hash)
rescue
  config = {}
end

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

  config.vm.provision "hostname-motd", type: "shell", path: "other/scripts/hostname_motd"
  config.vm.provision "locale", type: "shell", path: "other/scripts/set_locale", args: ENV['LC_NAME']
  config.vm.provision "mopidy", type: "shell", path: "other/scripts/install_mopidy"
  config.vm.provision "mopidy_webclient", type: "shell", inline: <<-SHELL
    pip install Mopidy-MusicBox-Webclient
  SHELL
  config.vm.provision "mopidy_youtube", type: "shell", path: "other/scripts/install_mopidy_youtube"
  config.vm.provision "restart", type: "shell", inline: <<-SHELL
    systemctl restart mopidy
  SHELL


  config.vm.post_up_message = "
  mopidy up and running.
  MPD and HTTP ports forwarded.
  MPD on port 9998
  HTTP on port 9999. Go ahead and open http://localhost:9999/ in your browser.
  "
end
