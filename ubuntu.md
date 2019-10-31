# Ubuntu

```sh
stack@workstation:~/devstack$ git clone https://opendev.org/starlingx/ha.git
stack@workstation:~/devstack$ cd ha/devstack/
```

```sh
stack@workstation:~/devstack/ha/devstack$ git clone https://opendev.org/starlingx/metal.git
stack@workstation:~/devstack/ha/devstack$ git clone https://opendev.org/starlingx/fault.git
```

```sh
make[1]: Entering directory '/opt/stack/devstack/ha/service-mgmt/sm/src'
```

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
 
 .c.o:
        $(CXX) $(INCLUDES) $(CCFLAGS) $(EXTRACCFLAGS) -c $< -o $@
```

```sh
stack@workstation:~/devstack/ha/devstack$ cd fault/fm-common/sources/
stack@workstation:~/devstack/ha/devstack/fault/fm-common/sources$ make
stack@workstation:~/devstack/ha/devstack/fault/fm-common/sources$ sudo cp libfmcommon.so /usr/lib/
stack@workstation:~/devstack/ha/devstack/fault/fm-common/sources$ cd -
```
