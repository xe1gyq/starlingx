# openSUSE

- [Build DevStack 01](#build-devstack-01)

## Links

- [Option 01](https://github.com/lorin/devstack-vm/blob/master/devstack.yml)

## DevStack Installation

```sh
user@linux-0ibz:~> git clone https://github.com/openstack/devstack.git
user@linux-0ibz:~> cd devstack/
```

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

```sh
user@linux-0ibz:~/devstack> bash stack.sh
```

## Build

```sh
stack@linux-0ibz:~/devstack> git clone https://opendev.org/starlingx/ha.git
stack@linux-0ibz:~/devstack> cd ha/devstack/
```

## Build DevStack 01

> Gerrit Review: [openSUSE: System Packages Devstack based](https://review.opendev.org/#/c/691905/)

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo zypper install glib2-devel libuuid-devel sqlite3-devel libjson-c-devel
```

## Build DevStack 02

> StarlingX metal and fault

```sh
stack@linux-0ibz:~/devstack/ha/devstack> git clone https://opendev.org/starlingx/metal
stack@linux-0ibz:~/devstack/ha/devstack> git clone https://opendev.org/starlingx/fault
```

## Build DevStack 03

> LDFLAGS

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

## Install install_ha

Missing for all systemd files:

```
#. /etc/init.d/functions
```

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

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-watchdog
stack@linux-0ibz:~/devstack/ha/devstack> echo $?
5
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-eru
stack@linux-0ibz:~/devstack/ha/devstack> echo $?
5
```

> __Learning__ ldconfig - configure dynamic linker run-time bindings

### install_sm_db

```sh
+build.sh:main:43                          install_sm_db
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:360  pushd /opt/stack/devstack/ha/service-mgmt/sm-db
~/devstack/ha/service-mgmt/sm-db ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:362  build_sm_db
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_db:86  pushd /opt/stack/devstack/ha/service-mgmt/sm-db
~/devstack/ha/service-mgmt/sm-db ~/devstack/ha/service-mgmt/sm-db ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_db:90  make VER=1.0.0 VER_MJR=1 'INCLUDES=-I/opt/stack/devstack/ha/service-mgmt/sm-common/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include' 'LDLIBS=-L /opt/stack/devstack/ha/service-mgmt/sm-common/src -lsqlite3 -lglib-2.0 -lrt -lsm_common -luuid' 'EXTRACCFLAGS=-D__STDC_FORMAT_MACROS -fpermissive' build
make[1]: Entering directory '/opt/stack/devstack/ha/service-mgmt/sm-db/src'
make[1]: Nothing to be done for 'build'.
make[1]: Leaving directory '/opt/stack/devstack/ha/service-mgmt/sm-db/src'
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_db:97  popd
~/devstack/ha/service-mgmt/sm-db ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:366  sudo install -m 0644 -p src/sm_db_configuration.h src/sm_db_foreach.h src/sm_db.h src/sm_db_iterator.h src/sm_db_node_history.h src/sm_db_nodes.h src/sm_db_service_action_results.h src/sm_db_service_actions.h src/sm_db_service_dependency.h src/sm_db_service_domain_assignments.h src/sm_db_service_domain_interfaces.h src/sm_db_service_domain_members.h src/sm_db_service_domain_neighbors.h src/sm_db_service_domains.h src/sm_db_service_group_members.h src/sm_db_service_groups.h src/sm_db_service_heartbeat.h src/sm_db_service_instances.h src/sm_db_services.h /usr/local/include
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:367  sudo install -m 0755 -p src/libsm_db.so.1.0.0 /usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:368  sudo cp -P src/libsm_db.so src/libsm_db.so.1 /usr/local/lib64
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:372  sqlite3 database/sm.db
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:373  sqlite3 database/sm.hb.db
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:376  local dest_dir=
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:377  [[ sudo != \s\u\d\o ]]
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:381  cd database
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:382  sudo -E make DEST_DIR= install
install -d /var/lib/sm
install -d /var/lib/sm/patches
install sm.hb.db /var/lib/sm
install sm.db /var/lib/sm
install -m 644 sm-patch.sql /var/lib/sm/patches
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_db:387  popd
~/devstack/ha/devstack
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sm-dump 
/var/run/sm/sm.db not available.
```

- https://opendev.org/starlingx/ha/src/branch/master/service-mgmt/sm-db/opensuse/libsm_db1.spec

```sh
%dir %{_sharedstatedir}/sm/patches
%{_libdir}/libsm_db.so.1
%{_libdir}/libsm_db.so.1.0.0
%config(noreplace) %{_sharedstatedir}/sm/sm.hb.db
%config(noreplace) %{_sharedstatedir}/sm/sm.db
%{_sharedstatedir}/sm/patches/sm-patch.sql
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo cp -r /var/lib/sm/ /var/run
```

ToDo Do we need a change in openSUSE specfile?

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sm-dump 
-Service_Groups------------------------------------------------------------------------
-Services------------------------------------------------------------------------------
```

### install_sm

```sh
+build.sh:main:44                          install_sm
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:259  pushd /opt/stack/devstack/ha/service-mgmt/sm
~/devstack/ha/service-mgmt/sm ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:261  build_sm
+opt/stack/devstack/ha/devstack/lib/ha:build_sm:49  pushd /opt/stack/devstack/ha/service-mgmt/sm
~/devstack/ha/service-mgmt/sm ~/devstack/ha/service-mgmt/sm ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:build_sm:54  make 'CCFLAGS=-g -O2 -Wall -Wformat -Wunused-result -std=c++11' 'INCLUDES=-I/opt/stack/devstack/ha/service-mgmt/sm-common/src -I/opt/stack/devstack/ha/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include -I/opt/stack/devstack/ha/devstack/metal/mtce/src/heartbeat/ -I/opt/stack/devstack/ha/devstack/fault/fm-common/sources/' 'LDLIBS=-L /opt/stack/devstack/ha/service-mgmt/sm-common/src -L /opt/stack/devstack/ha/service-mgmt/sm-db/src  -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl' build
make[1]: Entering directory '/opt/stack/devstack/ha/service-mgmt/sm/src'
g++ -g -O2 -Wall -Wformat -Wunused-result -std=c++11 -D__STDC_FORMAT_MACROS -Wformat -Wformat-security main.o sm_process.o sm_process_death.o sm_heartbeat.o sm_heartbeat_msg.o sm_heartbeat_thread.o sm_log.o sm_log_thread.o sm_alarm.o sm_alarm_thread.o sm_troubleshoot.o sm_api.o sm_notify_api.o sm_msg.o sm_node_api.cpp sm_node_fsm.o sm_node_unknown_state.o sm_node_enabled_state.o sm_node_disabled_state.o sm_service_domain_table.o sm_service_domain_member_table.o sm_service_domain_interface_table.o sm_service_domain_neighbor_table.o sm_service_domain_assignment_table.o sm_service_domain_api.o sm_service_domain_utils.o sm_service_domain_fsm.o sm_service_domain_initial_state.o sm_service_domain_waiting_state.o sm_service_domain_other_state.o sm_service_domain_backup_state.o sm_service_domain_leader_state.o sm_service_domain_interface_api.o sm_service_domain_interface_fsm.o sm_service_domain_interface_unknown_state.o sm_service_domain_interface_enabled_state.o sm_service_domain_interface_disabled_state.o sm_service_domain_neighbor_fsm.o sm_service_domain_neighbor_down_state.o sm_service_domain_neighbor_exchange_start_state.o sm_service_domain_neighbor_exchange_state.o sm_service_domain_neighbor_full_state.o sm_service_domain_scheduler.o sm_service_domain_filter.o sm_service_domain_weight.o sm_service_group_table.o sm_service_group_member_table.o sm_service_group_api.o sm_service_group_health.o sm_service_group_engine.o sm_service_group_fsm.o sm_service_group_initial_state.o sm_service_group_active_state.o sm_service_group_go_active_state.o sm_service_group_go_standby_state.o sm_service_group_standby_state.o sm_service_group_disabling_state.o sm_service_group_disabled_state.o sm_service_group_shutdown_state.o sm_service_group_enable.o sm_service_group_go_active.o sm_service_group_go_standby.o sm_service_group_disable.o sm_service_group_audit.o sm_service_group_notification.o sm_service_table.o sm_service_dependency_table.o sm_service_action_table.o sm_service_action_result_table.o sm_service_api.o sm_service_dependency.o sm_service_engine.o sm_service_fsm.o sm_service_initial_state.o sm_service_unknown_state.o sm_service_enabled_active_state.o sm_service_enabled_go_active_state.o sm_service_enabled_go_standby_state.o sm_service_enabled_standby_state.o sm_service_enabling_state.o sm_service_enabling_throttle_state.o sm_service_disabling_state.o sm_service_disabled_state.o sm_service_shutdown_state.o sm_service_enable.o sm_service_go_active.o sm_service_go_standby.o sm_service_disable.o sm_service_audit.o sm_service_action.o sm_service_heartbeat.o sm_service_heartbeat_api.o sm_service_heartbeat_thread.o sm_main_event_handler.o fm_api_wrapper.o sm_failover.o sm_swact_state.o sm_worker_thread.cpp sm_task_affining_thread.o sm_node_swact_monitor.cpp sm_failover_fsm.cpp sm_failover_initial_state.cpp sm_failover_normal_state.cpp sm_failover_fail_pending_state.cpp sm_failover_failed_state.cpp sm_failover_survived_state.cpp sm_failover_ss.o sm_service_domain_interface_not_in_use_state.o sm_configuration_table.o sm_failover_utils.o sm_cluster_hbs_info_msg.cpp sm_configure.cpp -rdynamic -I/opt/stack/devstack/ha/service-mgmt/sm-db/src/ -I/opt/stack/devstack/ha/devstack/metal/mtce/src/heartbeat/ -L /opt/stack/devstack/ha/service-mgmt/sm-common/src -L /opt/stack/devstack/ha/service-mgmt/sm-db/src  -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl -o sm
make[1]: Leaving directory '/opt/stack/devstack/ha/service-mgmt/sm/src'
+opt/stack/devstack/ha/devstack/lib/ha:build_sm:59  popd
~/devstack/ha/service-mgmt/sm ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:264  sudo install -m 755 src/sm /usr/local/bin/sm
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:265  sudo install -m 755 scripts/sm /etc/init.d/sm
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:266  sudo install -m 755 scripts/sm.shutdown /etc/init.d/sm-shutdown
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:267  sudo install -d 755 /usr/local/sbin
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:268  sudo install -m 755 scripts/sm.notify /usr/local/sbin/sm-notify
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:269  sudo install -m 755 scripts/sm.troubleshoot /usr/local/sbin/sm-troubleshoot
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:270  sudo install -m 755 scripts/sm.notification /usr/local/sbin/sm-notification
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:271  sudo install -d /etc/pmon.d
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:272  sudo install -m 644 scripts/sm.conf /etc/pmon.d/sm.conf
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:273  sudo install -d /etc/logrotate.d
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:274  sudo install -m 644 scripts/sm.logrotate /etc/logrotate.d/sm.logrotate
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:275  sudo install -m 644 -D scripts/sm.service /etc/systemd/system
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:276  sudo install -m 644 -D scripts/sm-shutdown.service /etc/systemd/system
+opt/stack/devstack/ha/devstack/lib/ha:install_sm:278  popd
~/devstack/ha/devstack
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm
stack@linux-0ibz:~/devstack/ha/devstack> echo $?
5
```

### install_sm_client

```sh
+build.sh:main:45                          install_sm_client
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_client:304  setup_install /opt/stack/devstack/ha/service-mgmt-client/sm-client
/opt/stack/devstack/ha/devstack/lib/ha: line 304: setup_install: command not found
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> vi lib/ha 
```

```sh
 function install_sm_client {
    pushd $STX_HA_DIR/service-mgmt-client/sm-client

    sudo python setup.py install \
        --root=/ \
        --install-lib=$PYTHON_SITE_DIR \
        --prefix=/usr \
        --install-data=/usr/share

    popd

     $STX_SUDO install -m 755 ${GITDIR[$STX_HA_NAME]}/service-mgmt-client/sm-client/usr/bin/smc $STX_BIN_DIR
 }
```

```sh
+build.sh:main:45                          install_sm_client
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_client:304  pushd /opt/stack/devstack/ha/service-mgmt-client/sm-client
~/devstack/ha/service-mgmt-client/sm-client ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_client:306  sudo python setup.py install --root=/ --install-lib=/usr/lib/python2.7/site-packages --prefix=/usr --install-data=/usr/share
running install
running build
running build_py
package init file 'sm_client/openstack/common/config/__init__.py' not found (or not a regular file)
running install_lib
running install_egg_info
running egg_info
writing sm_client.egg-info/PKG-INFO
writing top-level names to sm_client.egg-info/top_level.txt
writing dependency_links to sm_client.egg-info/dependency_links.txt
reading manifest file 'sm_client.egg-info/SOURCES.txt'
writing manifest file 'sm_client.egg-info/SOURCES.txt'
removing '/usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info' (and everything under it)
Copying sm_client.egg-info to /usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info
running install_scripts
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_client:312  popd
~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_client:314  sudo install -m 755 /opt/stack/devstack/ha/service-mgmt-client/sm-client/usr/bin/smc /usr/local/bin
```

```sh
stack@linux-0ibz:~/devstack> smc --smc-url=127.0.0.1:7777 servicenode-list
Unsupported scheme: 
```

### install_sm_tools

```sh
+build.sh:main:46                          install_sm_tools
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_tools:391  pushd /opt/stack/devstack/ha/service-mgmt-tools/sm-tools
~/devstack/ha/service-mgmt-tools/sm-tools ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_tools:393  sudo python setup.py install --root=/ --install-lib=/usr/lib/python2.7/site-packages --prefix=/usr --install-data=/usr/share
running install
running build
running build_py
creating build
creating build/lib
creating build/lib/sm_tools
copying sm_tools/__init__.py -> build/lib/sm_tools
copying sm_tools/sm_action.py -> build/lib/sm_tools
copying sm_tools/sm_api_msg_utils.py -> build/lib/sm_tools
copying sm_tools/sm_configure.py -> build/lib/sm_tools
copying sm_tools/sm_dump.py -> build/lib/sm_tools
copying sm_tools/sm_provision.py -> build/lib/sm_tools
copying sm_tools/sm_query.py -> build/lib/sm_tools
running install_lib
creating /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/__init__.py -> /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/sm_action.py -> /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/sm_api_msg_utils.py -> /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/sm_configure.py -> /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/sm_dump.py -> /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/sm_provision.py -> /usr/lib/python2.7/site-packages/sm_tools
copying build/lib/sm_tools/sm_query.py -> /usr/lib/python2.7/site-packages/sm_tools
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/sm_action.py to sm_action.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/sm_api_msg_utils.py to sm_api_msg_utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/sm_configure.py to sm_configure.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/sm_dump.py to sm_dump.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/sm_provision.py to sm_provision.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_tools/sm_query.py to sm_query.pyc
running install_egg_info
running egg_info
creating sm_tools.egg-info
writing sm_tools.egg-info/PKG-INFO
writing top-level names to sm_tools.egg-info/top_level.txt
writing dependency_links to sm_tools.egg-info/dependency_links.txt
writing entry points to sm_tools.egg-info/entry_points.txt
writing manifest file 'sm_tools.egg-info/SOURCES.txt'
reading manifest file 'sm_tools.egg-info/SOURCES.txt'
writing manifest file 'sm_tools.egg-info/SOURCES.txt'
Copying sm_tools.egg-info to /usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info
running install_scripts
Installing sm-dump script to /usr/bin
Installing sm-query script to /usr/bin
Installing sm-restart script to /usr/bin
Installing sm-unmanage script to /usr/bin
Installing sm-iface-state script to /usr/bin
Installing sm-provision script to /usr/bin
Installing sm-configure script to /usr/bin
Installing sm-manage script to /usr/bin
Installing sm-restart-safe script to /usr/bin
Installing sm-deprovision script to /usr/bin
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_tools:399  popd
~/devstack/ha/devstack
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sm-dump 
/var/run/sm/sm.db not available.
```

### install_sm_api

```sh
+build.sh:main:47                          install_sm_api
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:282  pushd /opt/stack/devstack/ha/service-mgmt-api/sm-api
~/devstack/ha/service-mgmt-api/sm-api ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:284  build_sm_api
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:63  pushd /opt/stack/devstack/ha/service-mgmt-api/sm-api
~/devstack/ha/service-mgmt-api/sm-api ~/devstack/ha/service-mgmt-api/sm-api ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:65  python setup.py build
running build
running build_py
package init file 'sm_api/openstack/common/config/__init__.py' not found (or not a regular file)
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:67  popd
~/devstack/ha/service-mgmt-api/sm-api ~/devstack/ha/devstack
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:287  sudo python setup.py install --root=/ --install-lib=/usr/lib/python2.7/site-packages --prefix=/usr --install-data=/usr/share
running install
running build
running build_py
package init file 'sm_api/openstack/common/config/__init__.py' not found (or not a regular file)
running install_lib
creating /usr/lib/python2.7/site-packages/sm_api
copying build/lib/sm_api/__init__.py -> /usr/lib/python2.7/site-packages/sm_api
creating /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/config.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/context.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/exception.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/log.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/policy.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/safe_utils.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/service.py -> /usr/lib/python2.7/site-packages/sm_api/common
copying build/lib/sm_api/common/utils.py -> /usr/lib/python2.7/site-packages/sm_api/common
creating /usr/lib/python2.7/site-packages/sm_api/db
copying build/lib/sm_api/db/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/db
copying build/lib/sm_api/db/api.py -> /usr/lib/python2.7/site-packages/sm_api/db
copying build/lib/sm_api/db/migration.py -> /usr/lib/python2.7/site-packages/sm_api/db
creating /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy
copying build/lib/sm_api/db/sqlalchemy/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy
copying build/lib/sm_api/db/sqlalchemy/api.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy
copying build/lib/sm_api/db/sqlalchemy/migration.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy
copying build/lib/sm_api/db/sqlalchemy/models.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy
creating /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo
copying build/lib/sm_api/db/sqlalchemy/migrate_repo/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo
copying build/lib/sm_api/db/sqlalchemy/migrate_repo/manage.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo
creating /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions
copying build/lib/sm_api/db/sqlalchemy/migrate_repo/versions/001_init.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions
copying build/lib/sm_api/db/sqlalchemy/migrate_repo/versions/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions
creating /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/base.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/smo_node.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/smo_sda.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/smo_sdm.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/smo_service.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/smo_servicegroup.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/smo_sgm.py -> /usr/lib/python2.7/site-packages/sm_api/objects
copying build/lib/sm_api/objects/utils.py -> /usr/lib/python2.7/site-packages/sm_api/objects
creating /usr/lib/python2.7/site-packages/sm_api/api
copying build/lib/sm_api/api/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/api
copying build/lib/sm_api/api/acl.py -> /usr/lib/python2.7/site-packages/sm_api/api
copying build/lib/sm_api/api/api.py -> /usr/lib/python2.7/site-packages/sm_api/api
copying build/lib/sm_api/api/app.py -> /usr/lib/python2.7/site-packages/sm_api/api
copying build/lib/sm_api/api/config.py -> /usr/lib/python2.7/site-packages/sm_api/api
copying build/lib/sm_api/api/hooks.py -> /usr/lib/python2.7/site-packages/sm_api/api
creating /usr/lib/python2.7/site-packages/sm_api/api/controllers
copying build/lib/sm_api/api/controllers/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers
copying build/lib/sm_api/api/controllers/root.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers
creating /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/base.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/collection.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/link.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/nodes.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/service_groups.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/servicenode.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/services.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/sm_sda.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/smc_api.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
copying build/lib/sm_api/api/controllers/v1/utils.py -> /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
creating /usr/lib/python2.7/site-packages/sm_api/api/middleware
copying build/lib/sm_api/api/middleware/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/api/middleware
copying build/lib/sm_api/api/middleware/auth_token.py -> /usr/lib/python2.7/site-packages/sm_api/api/middleware
copying build/lib/sm_api/api/middleware/parsable_error.py -> /usr/lib/python2.7/site-packages/sm_api/api/middleware
creating /usr/lib/python2.7/site-packages/sm_api/cmd
copying build/lib/sm_api/cmd/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/cmd
copying build/lib/sm_api/cmd/api.py -> /usr/lib/python2.7/site-packages/sm_api/cmd
creating /usr/lib/python2.7/site-packages/sm_api/openstack
copying build/lib/sm_api/openstack/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/cliutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/context.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/eventlet_backdoor.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/excutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/fileutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/gettextutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/importutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/jsonutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/local.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/lockutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/log.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/log_handler.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/loopingcall.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/network_utils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/periodic_task.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/policy.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/processutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/service.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/setup.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/strutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/threadgroup.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/timeutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/uuidutils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
copying build/lib/sm_api/openstack/common/version.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/db
copying build/lib/sm_api/openstack/common/db/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db
copying build/lib/sm_api/openstack/common/db/api.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db
copying build/lib/sm_api/openstack/common/db/exception.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy
copying build/lib/sm_api/openstack/common/db/sqlalchemy/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy
copying build/lib/sm_api/openstack/common/db/sqlalchemy/models.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy
copying build/lib/sm_api/openstack/common/db/sqlalchemy/session.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy
copying build/lib/sm_api/openstack/common/db/sqlalchemy/utils.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap
copying build/lib/sm_api/openstack/common/rootwrap/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap
copying build/lib/sm_api/openstack/common/rootwrap/cmd.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap
copying build/lib/sm_api/openstack/common/rootwrap/filters.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap
copying build/lib/sm_api/openstack/common/rootwrap/wrapper.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/amqp.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/common.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/dispatcher.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/impl_fake.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/impl_kombu.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/impl_qpid.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/impl_zmq.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/matchmaker.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/matchmaker_redis.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/matchmaker_ring.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/proxy.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/serializer.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/service.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
copying build/lib/sm_api/openstack/common/rpc/zmq_receiver.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/api.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/log_notifier.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/no_op_notifier.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/rpc_notifier.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/rpc_notifier2.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
copying build/lib/sm_api/openstack/common/notifier/test_notifier.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/config
copying build/lib/sm_api/openstack/common/config/generator.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/config
creating /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture
copying build/lib/sm_api/openstack/common/fixture/__init__.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture
copying build/lib/sm_api/openstack/common/fixture/mockpatch.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture
copying build/lib/sm_api/openstack/common/fixture/moxstubout.py -> /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture
byte-compiling /usr/lib/python2.7/site-packages/sm_api/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/config.py to config.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/context.py to context.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/exception.py to exception.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/log.py to log.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/policy.py to policy.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/safe_utils.py to safe_utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/service.py to service.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/common/utils.py to utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/api.py to api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/migration.py to migration.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/api.py to api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migration.py to migration.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/models.py to models.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/manage.py to manage.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/001_init.py to 001_init.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/base.py to base.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/smo_node.py to smo_node.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/smo_sda.py to smo_sda.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/smo_sdm.py to smo_sdm.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/smo_service.py to smo_service.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/smo_servicegroup.py to smo_servicegroup.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/smo_sgm.py to smo_sgm.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/objects/utils.py to utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/acl.py to acl.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/api.py to api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/app.py to app.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/config.py to config.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/hooks.py to hooks.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/root.py to root.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/base.py to base.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/collection.py to collection.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/link.py to link.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/nodes.py to nodes.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/service_groups.py to service_groups.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/servicenode.py to servicenode.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/services.py to services.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/sm_sda.py to sm_sda.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/smc_api.py to smc_api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/utils.py to utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/middleware/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/middleware/auth_token.py to auth_token.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/api/middleware/parsable_error.py to parsable_error.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/cmd/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/cmd/api.py to api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/cliutils.py to cliutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/context.py to context.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/eventlet_backdoor.py to eventlet_backdoor.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/excutils.py to excutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/fileutils.py to fileutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/gettextutils.py to gettextutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/importutils.py to importutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/jsonutils.py to jsonutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/local.py to local.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/lockutils.py to lockutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/log.py to log.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/log_handler.py to log_handler.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/loopingcall.py to loopingcall.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/network_utils.py to network_utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/periodic_task.py to periodic_task.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/policy.py to policy.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/processutils.py to processutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/service.py to service.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/setup.py to setup.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/strutils.py to strutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/threadgroup.py to threadgroup.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/timeutils.py to timeutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/uuidutils.py to uuidutils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/version.py to version.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/api.py to api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/exception.py to exception.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/models.py to models.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/session.py to session.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/utils.py to utils.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/cmd.py to cmd.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/filters.py to filters.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/wrapper.py to wrapper.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/amqp.py to amqp.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/common.py to common.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/dispatcher.py to dispatcher.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_fake.py to impl_fake.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_kombu.py to impl_kombu.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_qpid.py to impl_qpid.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_zmq.py to impl_zmq.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker.py to matchmaker.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_redis.py to matchmaker_redis.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_ring.py to matchmaker_ring.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/proxy.py to proxy.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/serializer.py to serializer.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/service.py to service.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/zmq_receiver.py to zmq_receiver.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/api.py to api.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/log_notifier.py to log_notifier.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/no_op_notifier.py to no_op_notifier.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier.py to rpc_notifier.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier2.py to rpc_notifier2.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/test_notifier.py to test_notifier.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/config/generator.py to generator.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/__init__.py to __init__.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/mockpatch.py to mockpatch.pyc
byte-compiling /usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/moxstubout.py to moxstubout.pyc
running install_egg_info
running egg_info
creating sm_api.egg-info
writing sm_api.egg-info/PKG-INFO
writing top-level names to sm_api.egg-info/top_level.txt
writing dependency_links to sm_api.egg-info/dependency_links.txt
writing entry points to sm_api.egg-info/entry_points.txt
writing manifest file 'sm_api.egg-info/SOURCES.txt'
reading manifest file 'sm_api.egg-info/SOURCES.txt'
writing manifest file 'sm_api.egg-info/SOURCES.txt'
Copying sm_api.egg-info to /usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info
running install_scripts
Installing sm-api script to /usr/bin
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:293  sudo install -m 755 scripts/sm-api /etc/init.d
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:295  sudo install -m 644 -D scripts/sm-api.service /etc/systemd/system
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:296  sudo install -m 644 -D scripts/sm_api.ini /etc/sm
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:297  sudo install -m 644 scripts/sm-api.conf /etc/pmon.d
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:298  sudo install -m 644 -D etc/sm-api/policy.json /etc/sm-api
+opt/stack/devstack/ha/devstack/lib/ha:install_sm_api:300  popd
~/devstack/ha/devstack
```

## Init init_ha

> Tbd

## Configure configure_ha

> Tbd

## Run

### Common

```sh
controller-0:~$ ls -al /var/run/sm*
-rw-r--r-- 1 root root   7 nov  7 03:06 /var/run/sm-api.pid
-r--r--r-- 1 root root   0 nov  7 03:06 /var/run/sm_boot_complete
-rw-r--r-- 1 root root   7 nov  7 03:06 /var/run/sm-eru.pid
-rw------- 1 root root   6 nov  7 02:57 /var/run/sm-notify.pid
-rw-r--r-- 1 root root   7 nov  7 03:06 /var/run/sm.pid
-rw-r--r-- 1 root root   7 nov  7 03:06 /var/run/sm-trap.pid
-rw-r--r-- 1 root root   7 nov  7 03:06 /var/run/sm-watchdog.pid

/var/run/sm:
total 1220
drwxr-xr-x  2 root root     180 nov  7 03:07 .
drwxr-xr-x 63 root root    3480 nov  7 13:45 ..
-rw-r--r--  1 root root      10 nov  7 03:06 .platform_cores
-rw-r--r--  1 root root  120832 nov  7 13:45 sm.db
-rw-r--r--  1 root root   32768 nov  7 03:06 sm.db-shm
-rw-r--r--  1 root root 1048032 nov  7 13:45 sm.db-wal
-rw-r--r--  1 root root    5120 nov  7 03:06 sm.hb.db
-rw-r--r--  1 root root   32768 nov  7 03:07 sm.hb.db-shm
-rw-r--r--  1 root root       0 nov  7 03:07 sm.hb.db-wal
```

### sm-common

General

- ToDo: https://en.opensuse.org/openSUSE:Systemd_packaging_guidelines#Creating_files_and_subdirectories_in_.2Fvar.2Frun_and_.2Frun

#### sm-common :: sm-eru

From StarlingX CentOS

```sh
[sysadmin@controller-0 ~(keystone_admin)]$ sm-eru
Failed to open scheduler log file (/var/log/sm-scheduler.log).
Failed to start debug thread, error=FAILED.
Debug initialization failed, error=FAILED.
[sysadmin@controller-0 ~(keystone_admin)]$ ps -aux | grep sm-eru
root      106661  0.1  0.0 112624  2084 ?        Sl   03:06   0:53 /usr/bin/sm-eru
sysadmin 1395040  0.0  0.0 112712   984 pts/0    S+   11:25   0:00 grep --color=auto sm-eru
[sysadmin@controller-0 ~(keystone_admin)]$ sudo /etc/init.d/sm-eru status
sm-eru is running
```

From StarlingX openSUSE [Error]

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo sm-eru
stack@linux-0ibz:~/devstack/ha/devstack> ps -aux | grep sm-eru
root     13631  0.0  0.0 129784  8892 pts/0    T    Nov01   0:00 sudo sm-eru
root     13632  0.0  0.0 113960  5088 pts/0    Tl   Nov01   0:00 sm-eru
stack    25348  0.0  0.0   8688   948 pts/0    S+   12:35   0:00 grep --color=auto sm-eru
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-eru status
```
 
```sh
stack@linux-0ibz:~/devstack/ha/devstack> cat /var/run/sm-eru.pid
13632
stack@linux-0ibz:~/devstack/ha/devstack> ps -aux | grep 13632
root     13632  0.0  0.0 113960  5088 pts/0    Tl   Nov01   0:00 sm-eru
stack    27598  0.0  0.0   8688   828 pts/0    S+   13:04   0:00 grep --color=auto 13632
```

ToInform: where does /etc/init.d/functions goes?

```sh
#. /etc/init.d/functions
```

ToInform: sm-watchdog path is different, change in spec

> - To search about ways to hanlde old sysinv into systemd

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-eru status
+ logger '/usr/bin/sm-eru is missing'
stack@linux-0ibz:~/devstack/ha/devstack> ls /usr/local/bin/sm-eru
/usr/local/bin/sm-eru
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-eru start
/etc/init.d/sm-eru: line 49: start-stop-daemon: command not found
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> zypper lr
Repository priorities are without effect. All enabled repositories share the same priority.

#  | Alias                     | Name                               | Enabled | GPG Check | Refresh
---+---------------------------+------------------------------------+---------+-----------+--------
 1 | openSUSE-Leap-15.1-1      | openSUSE-Leap-15.1-1               | No      | ----      | ----   
 2 | repo-debug                | Debug Repository                   | No      | ----      | ----   
 3 | repo-debug-non-oss        | Debug Repository (Non-OSS)         | No      | ----      | ----   
 4 | repo-debug-update         | Update Repository (Debug)          | No      | ----      | ----   
 5 | repo-debug-update-non-oss | Update Repository (Debug, Non-OSS) | No      | ----      | ----   
 6 | repo-non-oss              | Non-OSS Repository                 | Yes     | (r ) Yes  | Yes    
 7 | repo-oss                  | Main Repository                    | Yes     | (r ) Yes  | Yes    
 8 | repo-source               | Source Repository                  | No      | ----      | ----   
 9 | repo-source-non-oss       | Source Repository (Non-OSS)        | No      | ----      | ----   
10 | repo-update               | Main Update Repository             | Yes     | (r ) Yes  | Yes    
11 | repo-update-non-oss       | Update Repository (Non-Oss)        | Yes     | (r ) Yes  | Yes    
```

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo zypper install sysvinit
```

#### sm-common :: sm-watchdog

From StarlingX CentOS

```sh
[sysadmin@controller-0 ~(keystone_admin)]$ sm-watchdog 
Failed to open scheduler log file (/var/log/sm-scheduler.log).
Failed to start debug thread, error=FAILED.
Debug initialization failed, error=FAILED.
[sysadmin@controller-0 ~(keystone_admin)]$ ps -aux | grep sm-watchdog
root      106283  0.9  0.0 114692  2204 ?        Sl   03:06   4:49 /usr/bin/sm-watchdog
sysadmin 1395762  0.0  0.0 112712   988 pts/0    S+   11:25   0:00 grep --color=auto sm-watchdog
[sysadmin@controller-0 ~(keystone_admin)]$ sudo /etc/init.d/sm-watchdog status
sm-watchdog is running
```

From StarlingX openSUSE [Error]

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo sm-watchdog
^C
stack@linux-0ibz:~/devstack/ha/devstack> ps -aux | grep sm-watchdog
stack    25731  0.0  0.0   8688   828 pts/0    S+   12:40   0:00 grep --color=auto sm-watchdog
```

ToInform: where does /etc/init.d/functions goes?

```sh
#. /etc/init.d/functions
```

ToInform: sm-watchdog path is different, change in spec

```shstack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-watchdog status
+ logger '/usr/bin/sm-watchdog is missing'
stack@linux-0ibz:~/devstack/ha/devstack> ls /usr/local/bin/sm-watchdog
```

ToInform: start-stop-daemon

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo /etc/init.d/sm-watchdog start
/etc/init.d/sm-watchdog: line 49: start-stop-daemon: command not found
```

#### sm-common :: sm-eru-dump

From StarlingX CentOS

```sh
[sysadmin@controller-0 ~(keystone_admin)]$ sm-eru-dump 
    ...
    106223. 2019-11-07T11:27:49.614 tc-stats: eno2 sfq 40: bytes: 121647734646 packets: 90652767 qlen: 0 backlog: 0 drops: 0 requeues: 0 overlimits: 0
total-records: 106224 (entry-size=288 bytes)
```

From StarlingX openSUSE

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sudo sm-eru-dump
total-records: 0 (entry-size=288 bytes)
total-records: 0 (entry-size=288 bytes)
```

## sm-db

From StarlingX CentOS

```sh
[sysadmin@controller-0 ~(keystone_admin)]$ sm-dump

-Service_Groups------------------------------------------------------------------------
unable to open database file
```

```sh
[sysadmin@controller-0 ~(keystone_admin)]$ sudo sm-dump
-Service_Groups------------------------------------------------------------------------
oam-services                     active               active                         
...
-Services------------------------------------------------------------------------------
oam-ip                           enabled-active       enabled-active                  
management-ip                    enabled-active       enabled-active                  
...
```

From StarlingX openSUSE

```sh
stack@linux-0ibz:~/devstack/ha/devstack> sm-dump

-Service_Groups------------------------------------------------------------------------
oam-services                     initial              initial                        

-Services------------------------------------------------------------------------------
oam-ip                           initial              initial              none       
management-ip                    initial              initial              none       
```

ToInform: /var/run will dissapear when rebooted, see [here](https://en.opensuse.org/openSUSE:Systemd_packaging_guidelines#Creating_files_and_subdirectories_in_.2Fvar.2Frun_and_.2Frun), changes to be place [here](https://review.opendev.org/#/c/692625)

From [here](https://opendev.org/starlingx/ha/src/branch/master/service-mgmt-tools/sm-tools/sm_tools/sm_provision.py)

```
database_name = "/var/lib/sm/sm.db"
runtime_db_name = "/var/run/sm/sm.db"
```

## sm

From StarlingX CentOS

```sh
controller-0:~$ /etc/init.d/sm status
sm is running
```

```sh
controller-0:~$ ps -aux | grep sm
root      106283  0.9  0.0 114692  2204 ?        Sl   03:06   6:08 /usr/bin/sm-watchdog
root      106578  1.1  0.0 563472 13208 ?        S<l  03:06   7:28 /usr/bin/sm
root      106599  0.0  0.0 184004  1788 ?        S<l  03:06   0:01 /usr/bin/sm
root      106644  0.1  0.0 380824 66988 ?        S    03:06   0:54 /usr/bin/python2 /usr/bin/sm-api --config-file=/etc/sm-api/sm-api.conf --verbose --use-syslog --syslog-log-facility local1
root      106661  0.1  0.0 112624  2084 ?        Sl   03:06   1:09 /usr/bin/sm-eru
```
