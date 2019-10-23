# openSUSE HA Install DevStack

## sm

How about looking how devstack initialize services and execute in the order and with the arguments?

```
stack@linux-qwyc:~/devstack> /etc/init.d/sm
/etc/init.d/sm: line 24: /etc/init.d/functions: No such file or directory
```

Commented out this line

```sh
stack@linux-qwyc:~/devstack/ha> sudo vi /etc/init.d/sm 
#. /etc/init.d/functions
```

Start service

```sh
stack@linux-qwyc:~/devstack/ha> /etc/init.d/sm start
/etc/init.d/sm: line 52: /usr/bin/facter: No such file or directory
Starting sm: /etc/init.d/sm: line 63: start-stop-daemon: command not found
FAIL
```

```sh
stack@linux-qwyc:~/devstack/ha> vi devstack/files/debs/ha
facter
libglib2.0-dev
libjson-c-dev
libsqlite3-dev
```

```sh
stack@linux-qwyc:~/devstack/ha> git grep facter
devstack/files/debs/ha:facter
service-mgmt/sm/scripts/sm:        if [ "`/usr/bin/facter is_virtual`" = "true" ]
stx-ocf-scripts/src/ocf/dbmon:    # "facter" will return "true" or "false" 
stx-ocf-scripts/src/ocf/dbmon:    eval $(FACTERLIB=/usr/share/puppet/modules/platform/lib/facter/ facter is_controller_active)
```

ToDo: Add facter as a runtime dependency

## DevStack Build

### Runtime dependencies

```sh
stack@linux-qwyc:~/devstack/ha/devstack> ./build.sh 
sm_types.c:10:10: fatal error: glib.h: No such file or directory
 #include <glib.h>
          ^~~~~~~~
compilation terminated.
sm_uuid.c:9:10: fatal error: uuid/uuid.h: No such file or directory
 #include <uuid/uuid.h>
          ^~~~~~~~~~~~~
/usr/lib64/gcc/x86_64-suse-linux/7/../../../../x86_64-suse-linux/bin/ld: cannot find -lsqlite3
collect2: error: ld returned 1 exit status
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> sudo zypper install glib2-devel libuuid-devel sqlite3-devel
```

ToDo: Add above rpms as a runtime dependencies

### Include libraries

Error with build_sm

```
# This works for Zuul jobs using OpenStack's DevStack roles
plugin_requires ha metal
plugin_requires ha config
```

```sh
sm_cluster_hbs_info_msg.h:11:10: fatal error: mtceHbsCluster.h: No such file or directory
 #include "mtceHbsCluster.h"
          ^~~~~~~~~~~~~~~~~~
```

Error with build_sm_api

```sh
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:63  pushd /opt/stack/devstack/ha/service-mgmt-api/sm-api
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:65  python setup.py build
package init file 'sm_api/openstack/common/config/__init__.py' not found (or not a regular file)
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:67  popd
```
