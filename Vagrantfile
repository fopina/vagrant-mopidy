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

  config.vm.provision "fix-no-tty", type: "shell", privileged: false, inline: <<-SHELL
    sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile
  SHELL

  # install mopidy from source as per http://mopidy.readthedocs.org/en/latest/installation/source/

  config.vm.provision "mopidy", type: "shell", inline: <<-SHELL
    apt-get -qq update
    apt-get -qq install build-essential python-dev python-pip
    apt-get -qq install python-gst-1.0 gir1.2-gstreamer-1.0 gir1.2-gst-plugins-base-1.0 gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-tools
    pip install mopidy

    mkdir -p /root/.config/mopidy/
    cp /vagrant/files/mopidy.conf /root/.config/mopidy/mopidy.conf
    cp /vagrant/files/rc.local /etc/rc.local
    nohup /usr/local/bin/mopidy &
  SHELL

  config.vm.post_up_message = "
  mopidy up and running.
  MPD and HTTP ports forwarded.
  MPD on port 9998
  HTTP on port 9999. Go ahead and open http://localhost:9999/ in your browser.
  "
end
