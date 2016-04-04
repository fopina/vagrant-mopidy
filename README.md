Vagrant VM to test mopidy extensions

HowTo
=====

* Clone the repo
* Rename `config.yaml.example` to `config.yaml`
* Update `config.yaml` with the configuration you want
* Run `vagrant up` and wait!

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

Install other extensions as easily as:

```
$ vagrant ssh
vagrant@debian-jessie:~$ sudo -s
root@debian-jessie:/home/vagrant# pip search mopidy
...
Mopidy-Party (0.1.0)                - Mopidy web extension designed for party
Mopidy-Beets (2.0.0)                - Beets extension for Mopidy
Mopidy-Subsonic (1.0.0)             - Subsonic extension for Mopidy
...
root@debian-jessie:/home/vagrant# pip install mopidy-party
Collecting mopidy-party
  Downloading Mopidy-Party-0.1.0.tar.gz
...
  Running setup.py install for mopidy-party ... done
Successfully installed mopidy-party-0.1.0
root@debian-jessie:/home/vagrant# systemctl restart mopidy
```
