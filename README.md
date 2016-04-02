Vagrant VM to test mopidy extensions

HowTo
=====

Clone the repo then run `vagrant up` and wait!

```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
...
==> default: Machine 'default' has a post `vagrant up` message. This is a message
==> default: from the creator of the Vagrantfile, and not from Vagrant itself:
==> default:
==> default:
==> default:   mopidy up and running.
==> default:   MPD and HTTP ports forwarded.
==> default:   MPD on port 9998
==> default:   HTTP on port 9999. Go ahead and open http://localhost:9999/ in your browser.
==> default:
```

Install more extensions as easily as:

```
$ vagrant ssh
vagrant@debian-jessie:~$ sudo -s
root@debian-jessie:/home/vagrant# pip install mopidy-soundcloud
Collecting mopidy-soundcloud
  Downloading Mopidy_SoundCloud-2.0.2-py2.py3-none-any.whl
Successfully installed mopidy-soundcloud-2.0.2
root@debian-jessie:/home/vagrant# systemctl restart mopidy
```
