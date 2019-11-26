# openSUSE

## sm-manage | sm-unmanage

Question? How to define where a process is managed? _On AIO-DX ceph-osd processes are monitored by SM & on other deployments they are pmon managed._

- cgcs-root/stx/stx-puppet/puppet-manifests/src/modules/platform/manifests/ceph.pp
  - sm-manage service ceph-osd
  - sm-manage service  <%= @shutdown_drbd_resource %>
- cgcs-root/stx/stx-puppet/puppet-manifests/src/modules/platform/templates/partitions.manage.erb
  - sm-manage service cinder-lvm
  - sm-unmanage service ceph-osd
  - sm-unmanage service  <%= @shutdown_drbd_resource %>
  - sm-unmanage service cinder-lvm
  
  ## sm-provision
  
  - cgcs-root/stx/stx-puppet/puppet-manifests/src/modules/platform/manifests/sm.pp
 

## Init

- See [this](https://github.com/xe1gyq/starlingx/blob/master/multios/openSUSE_sm_tools.md) page for more information how this section was created.

1. The starting point is:
   - cgcs-root/stx/config/sysinv/sysinv/sysinv/setup.cfg
   - cgcs-root/stx/stx-puppet/puppet-manifests/src/modules/platform/manifests/sm.pp
2. We start by looking at "platform::sm"
   - config/sysinv/../conductor/
   - config/sysinv/../puppet/
   - stx-puppet/puppet-manifests/../manifests/
   - stx-puppet/puppet-manifests/../openstack/
   - stx-puppet/puppet-manifests/../platform/
3. Use _031_smapi_
   - 031_smapi = sysinv.puppet.smapi:SmPuppet
     - cgcs-root/stx/config/sysinv/sysinv/sysinv/setup.cfg
   - class SmPuppet(openstack.OpenstackBasePuppet):
     - cgcs-root/stx/config/sysinv/sysinv/sysinv/sysinv/puppet/smapi.py


### Puppet

These following files from source code

```sh
[user@6e7a95b2ad6e starlingx]$ ls ./cgcs-root/stx/stx-puppet/puppet-manifests/src/manifests/
ansible_bootstrap.pp  bootstrap.pp  controller.pp  runtime.pp  storage.pp  upgrade.pp  worker.pp
```

Are found at

```
controller-0:~$ ls /etc/puppet/manifests/
ansible_bootstrap.pp  bootstrap.pp  controller.pp  runtime.pp  storage.pp  upgrade.pp  worker.pp
```

#### Experimental

all start here

```sh
controller-0:~$ puppet cert list
Notice: 2019-11-26 11:51:17 +0000 Signed certificate request for ca
```
