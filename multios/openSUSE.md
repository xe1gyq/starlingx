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
