# openSUSE HA Install DevStack Sandbox

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
