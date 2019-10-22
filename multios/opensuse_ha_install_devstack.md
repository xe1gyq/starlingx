# DevStack

1. Create Ubuntu 18.04 based virtual machine:
   - 2 CPUs / 24 GB RAM / 64 GB SSD Disk
2. Install [DevStack](https://docs.openstack.org/devstack/latest) without Plugins
   - Review [plugins list](https://docs.openstack.org/devstack/latest/plugin-registry.html)
3. Add StarlingX High Availability Plugin and run again DevStack installation

```
$ sudo zypper -n addrepo -G https://download.opensuse.org/repositories/home:/xe1gyq/Cloud_StarlingX_2.0_openSUSE_Leap_15.1/home:xe1gyq.repo  &> /dev/null
$ sudo zypper refresh
```

```sh
$ sudo zypper search sm
```
```sh
stack@linux-qwyc:~/devstack> sudo zypper search sm | grep "Service Management"
  | libsm_db1                              | Service Management Databases                                                      | srcpackage
  | libsm_db1                              | Service Management Databases                                                      | package   
  | libsm_db1-devel                        | Service Management Databases - Development files                                  | package   
  | sm                                     | Service Management                                                                | srcpackage
  | sm                                     | Service Management                                                                | package   
  | sm-api                                 | Service Management API                                                            | srcpackage
  | sm-api                                 | Service Management API                                                            | package   
  | sm-client                              | Service Management Client and CLI                                                 | package   
  | sm-client                              | Service Management Client and CLI                                                 | srcpackage
  | sm-common                              | Service Management Common                                                         | srcpackage
  | sm-common                              | Service Management Common                                                         | package   
  | sm-common-devel                        | Service Management Common - Development files                                     | package   
  | sm-common-libs                         | Service Management Common - shared library fles                                   | package   
  | sm-eru                                 | Service Management ERU                                                            | package   
  | sm-tools                               | Service Management Tools                                                          | package   
  | sm-tools                               | Service Management Tools                                                          | srcpackage
```

## Installation Story

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install sm-common
Loading repository data...
Reading installed packages...
Resolving package dependencies...

Problem: nothing provides libamon1 needed by sm-common-1.0.0-lp151.2.2.x86_64
 Solution 1: do not install sm-common-1.0.0-lp151.2.2.x86_64
 Solution 2: break sm-common-1.0.0-lp151.2.2.x86_64 by ignoring some of its dependencies
```

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install libsm_db1
The following 2 NEW packages are going to be installed:
  libsm_db1 sm-common-libs
(1/2) Installing: sm-common-libs-1.0.0-lp151.2.2.x86_64
(2/2) Installing: libsm_db1-1.0.0-lp151.3.1.x86_64 
```

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install sm-common
Problem: nothing provides libamon1 needed by sm-common-1.0.0-lp151.2.2.x86_64
 Solution 1: do not install sm-common-1.0.0-lp151.2.2.x86_64
 Solution 2: break sm-common-1.0.0-lp151.2.2.x86_64 by ignoring some of its dependencies
```

```sh
sqlite> .database
main: /var/lib/sm/sm.db
sqlite> .tables
CONFIGURATION               SERVICE_DOMAIN_ASSIGNMENTS
NODES                       SERVICE_DOMAIN_INTERFACES 
NODE_HISTORY                SERVICE_DOMAIN_MEMBERS    
SCHEMA_VERSION              SERVICE_DOMAIN_NEIGHBORS  
SERVICES                    SERVICE_GROUPS            
SERVICE_ACTIONS             SERVICE_GROUP_MEMBERS     
SERVICE_ACTION_RESULTS      SERVICE_HEARTBEAT         
SERVICE_DEPENDENCY          SERVICE_INSTANCES         
SERVICE_DOMAINS           
```

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install sm
The following 2 NEW packages are going to be installed:
  fm-common sm
(1/2) Installing: fm-common-1.0.0-lp151.1.1.x86_64 
(2/2) Installing: sm-1.0.0-lp151.13.1.x86_64 
```

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install sm-tools
The following NEW package is going to be installed:
  sm-tools
(1/1) Installing: sm-tools-1.0.0-lp151.11.1.noarch 
```

```sh
stack@linux-qwyc:~/devstack> sm-dump
/var/run/sm/sm.db not available.
stack@linux-qwyc:~/devstack> sudo cp -r /var/lib/sm/ /var/run/
stack@linux-qwyc:~/devstack> sm-dump

-Service_Groups------------------------------------------------------------------------
oam-services                     initial              initial                        
controller-services              initial              initial                        
cloud-services                   initial              initial                        
patching-services                initial              initial                        
directory-services               initial              initial                        
web-services                     initial              initial                        
vim-services                     initial              initial                        
---------------------------------------------------------------------------------------

-Services------------------------------------------------------------------------------
oam-ip                           initial              initial              none       
management-ip                    initial              initial              none       
drbd-pg                          initial              initial              none       
drbd-rabbit                      initial              initial              none       
drbd-platform                    initial              initial              none       
pg-fs                            initial              initial              none       
rabbit-fs                        initial              initial              none       
nfs-mgmt                         initial              initial              none       
platform-fs                      initial              initial              none       
postgres                         initial              initial              none       
rabbit                           initial              initial              none       
platform-export-fs               initial              initial              none       
platform-nfs-ip                  initial              initial              none       
sysinv-inv                       initial              initial              none       
sysinv-conductor                 initial              initial              none       
mtc-agent                        initial              initial              none       
hw-mon                           initial              initial              none       
dnsmasq                          initial              initial              none       
fm-mgr                           initial              initial              none       
keystone                         initial              initial              none       
open-ldap                        initial              initial              none       
snmp                             initial              initial              none       
lighttpd                         initial              initial              none       
horizon                          initial              initial              none       
patch-alarm-manager              initial              initial              none       
vim                              initial              initial              none       
vim-api                          initial              initial              none       
vim-webserver                    initial              initial              none       
guest-agent                      initial              initial              none       
haproxy                          initial              initial              none       
drbd-extension                   initial              initial              none       
extension-fs                     initial              initial              none       
extension-export-fs              initial              initial              none       
etcd                             initial              initial              none       
barbican-api                     initial              initial              none       
barbican-keystone-listener       initial              initial              none       
barbican-worker                  initial              initial              none       
cluster-host-ip                  initial              initial              none       
------------------------------------------------------------------------------
```

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install sm-client
The following 47 NEW packages are going to be installed:
  python2-asn1crypto python2-Babel python2-blinker python2-certifi python2-cffi python2-chardet python2-cryptography python2-cssselect python2-debtcollector python2-ecdsa
  python2-httplib2 python2-idna python2-iso8601 python2-keystoneauth1 python2-keystoneclient python2-lxml python2-monotonic python2-msgpack python2-ndg-httpsclient
  python2-netaddr python2-netifaces python2-oauthlib python2-oslo.config python2-oslo.i18n python2-oslo.serialization python2-oslo.utils python2-pbr python2-positional
  python2-PrettyTable python2-py python2-pyasn1 python2-pycparser python2-PyJWT python2-pykerberos python2-pyOpenSSL python2-PySocks python2-pytz python2-PyYAML
  python2-requests python2-requests-kerberos python2-rfc3986 python2-stevedore python2-urllib3 python2-wrapt python-enum34 python-funcsigs sm-client

The following 5 recommended packages were automatically selected:
  python2-cryptography python2-idna python2-ndg-httpsclient python2-pyOpenSSL python2-PySocks
```

```sh
stack@linux-qwyc:~/devstack> smc
usage: smc [--version] [--debug] [-v] [-k] [--cert-file CERT_FILE]
```

```sh
stack@linux-qwyc:~/devstack> sudo zypper -n --no-gpg-checks install sm-api
Loading repository data...
Reading installed packages...
Resolving package dependencies...

Problem: nothing provides libamon1 needed by sm-api-1.0.0-lp151.3.1.x86_64
 Solution 1: do not install sm-api-1.0.0-lp151.3.1.x86_64
 Solution 2: break sm-api-1.0.0-lp151.3.1.x86_64 by ignoring some of its dependencies
```

```sh
$ export OS_USERNAME=admin
$ export OS_PASSWORD=user
$ export OS_AUTH_URL=http://10.0.2.15/identity/v3
$ smc service-list
```

```sh
$ source openrc
$ nova flavor-list
$ smc service-list
```

## Errors To Look At

```sh
stack@linux-qwyc:~/devstack> sm
ERROR: : sm_db_nodes.c(153): Failed to read, sql=SELECT * FROM NODES WHERE name <> 'linux-qwyc';, rc=21, error=(null).
ERROR: : sm_failover_ss.c(77): Failed to get peername, error=FAILED.
Trap thread (1095) created.
Failed to open scheduler log file (/var/log/sm-scheduler.log).
Failed to start debug thread, error=FAILED.
Debug initialization failed, error=FAILED.
```

