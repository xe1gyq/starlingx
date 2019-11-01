# openSUSE

## DevStack Installation

```sh
stack@linux-0ibz:~/devstack> vi local.conf 
[[local|localrc]]
ADMIN_PASSWORD=user
MYSQL_PASSWORD=user
RABBIT_PASSWORD=user
SERVICE_PASSWORD=user
SERVICE_TOKEN=user
FLAT_INTERFACE=br100
LOGFILE=$DEST/logs/stack.sh.log
HOST_IP=127.0.0.1
```

## Build ha

```sh
stack@linux-0ibz:~/devstack> git clone https://opendev.org/starlingx/ha.git
stack@linux-0ibz:~/devstack> cd ha/devstack/
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo zypper install glib2-devel libuuid-devel sqlite3-devel libjson-c-devel
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> git clone https://opendev.org/starlingx/metal
stack@linux-0ibz:~/devstack/ha/devstack> git clone https://opendev.org/starlingx/fault
```

Compilation failed:

```sh
diff --git a/devstack/lib/ha b/devstack/lib/ha
index 12ee724..ba0ce4d 100644
--- a/devstack/lib/ha
+++ b/devstack/lib/ha
@@ -52,7 +52,7 @@ function build_sm {
     # CCFLAGS= -g -O2 -Wall -Werror -Wformat  -std=c++11
     make \
         CCFLAGS="-g -O2 -Wall -Wformat -Wunused-result -std=c++11" \
-        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include" \
+        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include -I/opt/stack/devstack/ha/devstack/metal/mtce/src/heartbeat/ -I/opt/stack/devstack/ha/devstack/fault/fm-common/sources/" \
         LDLIBS="-L $STX_HA_DIR/service-mgmt/sm-common/src -L $STX_HA_DIR/service-mgmt/sm-db/src  -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl" \
         build
```

Compilation fixed but compilation failed again:

```sh
diff --git a/service-mgmt/sm/src/Makefile b/service-mgmt/sm/src/Makefile
index 171c292..c11e881 100644
--- a/service-mgmt/sm/src/Makefile
+++ b/service-mgmt/sm/src/Makefile
@@ -123,7 +123,7 @@ OBJS= $(SRCS:.c=.o)
 CCFLAGS= -g -O2 -Wall -Werror -Wformat  -std=c++11
 EXTRACCFLAGS= -D__STDC_FORMAT_MACROS -Wformat -Wformat-security
 LDLIBS= -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl
-LDFLAGS = -rdynamic
+LDFLAGS = -rdynamic -I/opt/stack/devstack/ha/service-mgmt/sm-db/src/ -I/opt/stack/devstack/ha/devstack/metal/mtce/src/heartbeat/
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> cd fault/fm-common/sources/
stack@linux-0ibz:~/devstack/ha/devstack/fault/fm-common/sources> make
stack@linux-0ibz:~/devstack/ha/devstack/fault/fm-common/sources> sudo cp libfmcommon.so /usr/lib64/
stack@linux-0ibz:~/devstack/ha/devstack/fault/fm-common/sources> cd -
```

### Warning Package Init File

> build_sm_api failing

```sh
stack@linux-0ibz:~/devstack/ha/devstack> bash build.sh 
~/devstack/ha/service-mgmt-api/sm-api ~/devstack/ha/devstack
+ python setup.py build
running build
running build_py
package init file 'sm_api/openstack/common/config/__init__.py' not found (or not a regular file)
```

## Install

### install_sm_common

```sh
+build.sh:main:42                          install_sm_common
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:339  pushd /opt/stack/devstack/ha/service-mgmt/sm-common
~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:341  install_sm_common_libs
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:310  pushd /opt/stack/devstack/ha/service-mgmt/sm-common
~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:312  build_sm_common
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_common:71  pushd /opt/stack/devstack/ha/service-mgmt/sm-common
~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_common:76  make VER=1.0.0 VER_MJR=1 'CCFLAGS=-fPIC -g -Wall -Werror' 'INCLUDES=-I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include' build
make[1]: Entering directory '/opt/stack/devstack/ha/service-mgmt/sm-common/src'
make[1]: Nothing to be done for 'build'.
make[1]: Leaving directory '/opt/stack/devstack/ha/service-mgmt/sm-common/src'
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_common:82  popd
~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:329  sudo install -d /usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:330  sudo install src/libsm_common.so.1.0.0 /usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:331  sudo cp -P src/libsm_common.so src/libsm_common.so.1 /usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:332  sudo install -d /usr/local/include
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:333  sudo install -m 644 src/sm_debug.h src/sm_debug_thread.h src/sm_eru_db.h src/sm_eru_process.h src/sm_hw.h src/sm_limits.h src/sm_list.h src/sm_netlink.h src/sm_node_stats.h src/sm_node_utils.h src/sm_selobj.h src/sm_sha512.h src/sm_thread_health.h src/sm_time.h src/sm_timer.h src/sm_trap.h src/sm_trap_thread.h src/sm_types.h src/sm_utils.h src/sm_util_types.h src/sm_uuid.h src/sm_watchdog_module.h src/sm_watchdog_nfs.h src/sm_watchdog_process.h /usr/local/include
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common_libs:335  popd
~/devstack/ha/service-mgmt/sm-common ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:343  sudo install -m 0755 -p -D -t /var/lib/sm/watchdog/modules src/libsm_watchdog_nfs.so.1.0.0
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:344  sudo cp -P src/libsm_watchdog_nfs.so src/libsm_watchdog_nfs.so.1 /var/lib/sm/watchdog/modules
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:347  cd scripts
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:347  sudo make DEST_DIR= UNIT_DIR=/etc/systemd/system install
install -d /etc/systemd/system
install -m 644 *.service /etc/systemd/system 
install -d /etc/init.d
install sm-watchdog sm-eru /etc/init.d
install -d /etc/pmon.d
install *.conf /etc/pmon.d
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:349  sudo install -m 750 -p -D src/sm_eru /usr/local/bin/sm-eru
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:350  sudo install -m 750 -p -D src/sm_eru_dump /usr/local/bin/sm-eru-dump
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:351  sudo install -m 750 -p -D src/sm_watchdog /usr/local/bin/sm-watchdog
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:353  echo /usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:353  sudo tee /etc/ld.so.conf.d/stx-ha.conf
/usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:354  sudo ldconfig
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_common:356  popd
~/devstack/ha/devstack
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo sm-eru
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo sm-eru-dump 
total-records: 0 (entry-size=288 bytes)
total-records: 0 (entry-size=288 bytes)
```
