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
 
