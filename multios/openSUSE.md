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

## ha

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
