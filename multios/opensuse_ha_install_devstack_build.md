# openSUSE HA Install DevStack Build

Build command

```
stack@linux-qwyc:~/devstack/ha/devstack> bash build.sh 
```

## Runtime dependencies

- https://review.opendev.org/#/c/691905

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

/usr/lib64/gcc/x86_64-suse-linux/7/../../../../x86_64-suse-linux/bin/ld: cannot find -ljson-c
collect2: error: ld returned 1 exit status
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> sudo zypper install glib2-devel libuuid-devel sqlite3-devel libjson-c-devel
```

## Includes libraries

> Errors with build_sm anb build_sm_api

```sh
# This works for Zuul jobs using OpenStack's DevStack roles
plugin_requires ha metal
plugin_requires ha config
```

Error mtceHbsCluster.h

```sh
sm_cluster_hbs_info_msg.h:11:10: fatal error: mtceHbsCluster.h: No such file or directory
 #include "mtceHbsCluster.h"
          ^~~~~~~~~~~~~~~~~~
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> git clone https://opendev.org/starlingx/metal.git
stack@linux-qwyc:~/devstack/ha/devstack> find . -name mtceHbsCluster.h
./metal/mtce/src/heartbeat/mtceHbsCluster.h
```

```patch
diff --git a/devstack/lib/ha b/devstack/lib/ha
index 12ee724..c6adcfc 100644
--- a/devstack/lib/ha
+++ b/devstack/lib/ha
@@ -52,7 +52,7 @@ function build_sm {
     # CCFLAGS= -g -O2 -Wall -Werror -Wformat  -std=c++11
     make \
         CCFLAGS="-g -O2 -Wall -Wformat -Wunused-result -std=c++11" \
-        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include" \
+        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include -I$STX_HA_DIR/devstack/metal/mtce/src/heartbeat/" \
         LDLIBS="-L $STX_HA_DIR/service-mgmt/sm-common/src -L $STX_HA_DIR/service-mgmt/sm-db/src  -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl" \
         build
```

Error fmAPI.h

```sh
In file included from sm_log_thread.c:23:0:
fm_api_wrapper.h:10:10: fatal error: fmAPI.h: No such file or directory
 #include <fmAPI.h>
          ^~~~~~~~~
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> git clone https://opendev.org/starlingx/fault
stack@linux-qwyc:~/devstack/ha/devstack> find . -name fmAPI.h
./fault/fm-common/sources/fmAPI.h
```

Final Patch

```patch
diff --git a/devstack/lib/ha b/devstack/lib/ha
index 12ee724..f9d9786 100644
--- a/devstack/lib/ha
+++ b/devstack/lib/ha
@@ -52,7 +52,7 @@ function build_sm {
     # CCFLAGS= -g -O2 -Wall -Werror -Wformat  -std=c++11
     make \
         CCFLAGS="-g -O2 -Wall -Wformat -Wunused-result -std=c++11" \
-        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include" \
+        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I/usr/local/include -I$STX_HA_DIR/devstack/metal/mtce/src/heartbeat/ -I$STX_HA_DIR/devstack/fault/fm-common/sources/" \
         LDLIBS="-L $STX_HA_DIR/service-mgmt/sm-common/src -L $STX_HA_DIR/service-mgmt/sm-db/src  -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl" \
         build
 
diff --git a/service-mgmt/sm/src/Makefile b/service-mgmt/sm/src/Makefile
index 171c292..132ffa6 100644
--- a/service-mgmt/sm/src/Makefile
+++ b/service-mgmt/sm/src/Makefile
@@ -123,7 +123,7 @@ OBJS= $(SRCS:.c=.o)
 CCFLAGS= -g -O2 -Wall -Werror -Wformat  -std=c++11
 EXTRACCFLAGS= -D__STDC_FORMAT_MACROS -Wformat -Wformat-security
 LDLIBS= -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl
-LDFLAGS = -rdynamic
+LDFLAGS = -rdynamic -I/opt/stack/devstack/ha/devstack/metal/mtce/src/heartbeat/ -I/opt/stack/devstack/ha/devstack/fault/fm-common/sources/ -I/opt/stack/devstack/ha/service-mgmt/sm-db/src/
 
 .c.o:
        $(CXX) $(INCLUDES) $(CCFLAGS) $(EXTRACCFLAGS) -c $< -o $@
```

Error fmcommon

```sh
g++ -o fmManager fm_main.o  -lfmcommon -lrt -lpthread -luuid
/usr/lib64/gcc/x86_64-suse-linux/7/../../../../x86_64-suse-linux/bin/ld: cannot find -lfmcommon
collect2: error: ld returned 1 exit status
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack/fault/devstack> sudo cp /opt/stack/devstack/ha/devstack/fault/fm-common/sources/libfmcommon.so /usr/lib64/
stack@linux-qwyc:~/devstack/ha/devstack/fault/devstack> sudo cp /opt/stack/devstack/ha/devstack/fault/fm-common/sources/libfmcommon.so /usr/lib64/fmcommon.so
```

Build Successful

```
stack@linux-qwyc:~/devstack/ha/devstack> bash build.sh 
```

## Sandbox

```sh
zypper se -s libjson
sudo zypper install libjson-c-devel
sudo zypper remove jsoncpp-devel
sudo zypper install libjson-c-devel
```

```sh
sudo zypper in --download-only sm-common
find /var/cache/zypp -iname "sm-common*rpm"
rpm -qlp /var/cache/zypp/packages/home_xe1gyq/x86_64/sm-common-1.0.0-lp151.2.2.x86_64.rpm
find /var/cache/zypp -iname "libamon1*rpm"
rpm -qlp /var/cache/zypp/packages/Cloud_StarlingX_2.0/x86_64/libamon1-1.0-lp150.10.1.x86_64.rpm 
sudo zypper install sm-common
```

```sh
diff --git a/devstack/lib/ha b/devstack/lib/ha
index 12ee724..f2a2bf4 100644
--- a/devstack/lib/ha
+++ b/devstack/lib/ha
@@ -52,7 +52,7 @@ function build_sm {
     # CCFLAGS= -g -O2 -Wall -Werror -Wformat  -std=c++11
     make \
         CCFLAGS="-g -O2 -Wall -Wformat -Wunused-result -std=c++11" \
-        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include" \
+        INCLUDES="-I$STX_HA_DIR/service-mgmt/sm-common/src -I$STX_HA_DIR/service-mgmt/sm-db/src -I/usr/lib64/glib-2.0/include -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -I$/usr/local/include -I$STX_HA_DIR/devstack/metal/mtce/src/heartbeat/ -I$STX_HA_DIR/devstack/fault/fm-common/sources/" \
         LDLIBS="-L $STX_HA_DIR/service-mgmt/sm-common/src -L $STX_HA_DIR/service-mgmt/sm-db/src  -lsqlite3 -lglib-2.0 -luuid -lpthread -lrt -lsm_common -lsm_db -lfmcommon -ljson-c -lcrypto -lssl" \
         build
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> sudo ln -s /usr/include/json /usr/include/json-c
stack@linux-qwyc:~/devstack/ha/devstack> pkg-config --cflags jsoncpp
stack@linux-qwyc:~/devstack/ha/devstack> cat /usr/lib64/pkgconfig/jsoncpp.pc 
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> sudo zypper addrepo https://download.opensuse.org/repositories/devel:libraries:c_c++/openSUSE_Leap_15.1/devel:libraries:c_c++.repo
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> git clone https://github.com/json-c/json-c.git
stack@linux-qwyc:~/devstack/ha/devstack/json-c> git checkout -b json-c-0.11 origin/json-c-0.11
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> cd jsoncpp/
stack@linux-qwyc:~/devstack/ha/devstack/jsoncpp> git checkout -b 0.10.0 0.10.0
```

```sh
stack@linux-qwyc:~/devstack/ha/devstack> zypper se --provides libjsoncpp
```

```sh
sudo zypper -n addrepo -G http://download.opensuse.org/repositories/Cloud:/StarlingX:/2.0/openSUSE_Leap_15.0/Cloud:StarlingX:2.0.repo &> /dev/null
sudo zypper -n addrepo -G http://download.opensuse.org/repositories/Cloud:/OpenStack:/Stein/openSUSE_Leap_15.0/Cloud:OpenStack:Stein.repo &> /dev/null
```

```sh
+ make MAJOR=1 MINOR=0 -I/opt/stack/devstack/ha/devstack/fault/fm-common/sources/
g++ -o fmManager fm_main.o  -lfmcommon -lrt -lpthread -luuid
/usr/lib64/gcc/x86_64-suse-linux/7/../../../../x86_64-suse-linux/bin/ld: cannot find -lfmcommon
collect2: error: ld returned 1 exit status
make: *** [Makefile:24: fmManager] Error 1
+ popd
~/devstack/ha/devstack/fault/devstack
```

Error with build_sm_api

```sh
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:63  pushd /opt/stack/devstack/ha/service-mgmt-api/sm-api
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:65  python setup.py build
package init file 'sm_api/openstack/common/config/__init__.py' not found (or not a regular file)
+opt/stack/devstack/ha/devstack/lib/ha:build_sm_api:67  popd
```

## Differences

```sh
diff --git a/devstack/lib/fault b/devstack/lib/fault
index 614fe31..95a03a0 100644
--- a/devstack/lib/fault
+++ b/devstack/lib/fault
@@ -106,7 +106,7 @@ function build_fm_mgr {
     # build
     CPATH=$STX_INST_DIR/include LIBRARY_PATH=$STX_INST_DIR/lib make \
         MAJOR=$major \
-        MINOR=$minor
+        MINOR=$minor -I/opt/stack/devstack/ha/devstack/fault/fm-common/sources/
 
     popd
 }
diff --git a/fm-mgr/sources/Makefile b/fm-mgr/sources/Makefile
index 0d683c3..d04d010 100755
--- a/fm-mgr/sources/Makefile
+++ b/fm-mgr/sources/Makefile
@@ -2,7 +2,7 @@ SRCS = fm_main.cpp
 OBJS = fm_main.o
 
 OBJS = $(SRCS:.cpp=.o)
-INCLUDES = -I.
+INCLUDES = -I. -I/opt/stack/devstack/ha/devstack/fault/fm-common/sources
 CCFLAGS = -g -O2 -Wall -Werror
 EXTRACCFLAGS = -Wformat -Wformat-security
```

```sh

```
