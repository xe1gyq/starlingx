# DevStack

1. Create Ubuntu 18.04 based virtual machine:
   - 4 CPUs / 8GB / 160 GB SSD Disk
2. Install [DevStack](https://docs.openstack.org/devstack/latest) without Plugins
   - Review [plugins list](https://docs.openstack.org/devstack/latest/plugin-registry.html)
3. Add StarlingX High Availability Plugin and run again DevStack installation

## Sandbox

```
sudo /sbin/vboxconfig
cd ~/suse-starlingx/vagrant
<Modify Vagrant Image to bento/opensuse-leap-15.1>
vagrant up
vagrant ssh
```
