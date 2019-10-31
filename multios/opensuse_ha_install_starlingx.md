# openSUSE High Availability Install StarlingX

## Step One

This is the state of Service Manager after StarlingX ISO has been installed:

```sh
localhost:~$ rpm -qa | grep "sm-"
sm-eru-1.0.0-20.tis.x86_64
sm-client-1.0-2.tis.x86_64
sm-api-1.0-4.tis.x86_64
sm-tools-1.0-2.tis.x86_64
sm-1.0.0-33.tis.x86_64
sm-db-1.0.0-31.tis.x86_64
sm-common-libs-1.0.0-20.tis.x86_64
sm-common-1.0.0-20.tis.x86_64
```

### sm-eru

```sh
localhost:~$ rpm -ql sm-eru-1.0.0-20.tis.x86_64
/etc/init.d/sm-eru
/etc/pmon.d/sm-eru.conf
/usr/bin/sm-eru
/usr/bin/sm-eru-dump
/usr/lib/systemd/system/sm-eru.service
```

```sh
localhost:~$ sm-eru
Failed to open scheduler log file (/var/log/sm-scheduler.log).
Failed to start debug thread, error=FAILED.
Debug initialization failed, error=FAILED.
localhost:~$ sm-eru-dump 
total-records: 2003 (entry-size=288 bytes)
         0. 2019-10-31T17:02:59.108 mem-stats: total: 24522892 free: 22510180 buffers: 30864 cached: 1389992 swap-cached: 0 swap-total: 0 swap-free: 0 active: 390364 inactive: 1334888 dirty: 1180748 hugepages-total: 0 hugepages-free: 0 hugepage-size: 2048 nfs-uncommited: 0 commited: 577092
         1. 2019-10-31T17:02:59.108 disk-stats: sdb major: 8 minor: 16 reads-completed: 96 reads-merged: 0 sectors-read: 4224 ms-spent-reading: 1 writes-completed: 0 writes-merged: 0 sectors-written: 0 ms-spent-writing: 0 io-inprogress: 0 ms-spent-io: 1 weighted-ms-spent-io: 1
         2. 2019-10-31T17:02:59.109 disk-stats: sda major: 8 minor: 0 reads-completed: 14140 reads-merged: 20 sectors-read: 460500 ms-spent-reading: 2477 writes-completed: 1125 writes-merged: 2077 sectors-written: 64472 ms-spent-writing: 1145 io-inprogress: 0 ms-spent-io: 2290 weighted-ms-spent-io: 3605
         2002. 2019-10-31T18:26:02.626 net-stats: none rx-bytes: 0 rx-packets: 0 rx-errors: 0 rx-dropped: 0 rx-fifo-errors: 0 rx-frame-errors: 0 rx-compressed-packets: 0 rx-multicast-frames: 0 tx-bytes: 0 tx-packets: 0 tx-errors: 0 tx-dropped: 0 tx-fifo-errors: 0 tx-collisions: 0 tx-carrier-loss: 0 tx-compressed-packets: 0
total-records: 2003 (entry-size=288 bytes)
```

### sm-client

```sh
localhost:~$ rpm -ql sm-client-1.0-2.tis.x86_64
/usr/bin/smc
/usr/lib/python2.7/site-packages/sm_client
/usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info
/usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info/PKG-INFO
/usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info/SOURCES.txt
/usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info/dependency_links.txt
/usr/lib/python2.7/site-packages/sm_client-1.0.0-py2.7.egg-info/top_level.txt
/usr/lib/python2.7/site-packages/sm_client/__init__.py
/usr/lib/python2.7/site-packages/sm_client/__init__.pyc
/usr/lib/python2.7/site-packages/sm_client/__init__.pyo
/usr/lib/python2.7/site-packages/sm_client/client.py
/usr/lib/python2.7/site-packages/sm_client/client.pyc
/usr/lib/python2.7/site-packages/sm_client/client.pyo
/usr/lib/python2.7/site-packages/sm_client/common
/usr/lib/python2.7/site-packages/sm_client/common/__init__.py
/usr/lib/python2.7/site-packages/sm_client/common/__init__.pyc
/usr/lib/python2.7/site-packages/sm_client/common/__init__.pyo
/usr/lib/python2.7/site-packages/sm_client/common/base.py
/usr/lib/python2.7/site-packages/sm_client/common/base.pyc
/usr/lib/python2.7/site-packages/sm_client/common/base.pyo
/usr/lib/python2.7/site-packages/sm_client/common/http.py
/usr/lib/python2.7/site-packages/sm_client/common/http.pyc
/usr/lib/python2.7/site-packages/sm_client/common/http.pyo
/usr/lib/python2.7/site-packages/sm_client/common/utils.py
/usr/lib/python2.7/site-packages/sm_client/common/utils.pyc
/usr/lib/python2.7/site-packages/sm_client/common/utils.pyo
/usr/lib/python2.7/site-packages/sm_client/exc.py
/usr/lib/python2.7/site-packages/sm_client/exc.pyc
/usr/lib/python2.7/site-packages/sm_client/exc.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack
/usr/lib/python2.7/site-packages/sm_client/openstack/__init__.py
/usr/lib/python2.7/site-packages/sm_client/openstack/__init__.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/__init__.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common
/usr/lib/python2.7/site-packages/sm_client/openstack/common/__init__.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/__init__.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/__init__.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/config
/usr/lib/python2.7/site-packages/sm_client/openstack/common/config/generator.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/config/generator.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/config/generator.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/gettextutils.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/gettextutils.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/gettextutils.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/importutils.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/importutils.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/importutils.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/__init__.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/__init__.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/__init__.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/cmd.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/cmd.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/cmd.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/filters.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/filters.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/filters.pyo
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/wrapper.py
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/wrapper.pyc
/usr/lib/python2.7/site-packages/sm_client/openstack/common/rootwrap/wrapper.pyo
/usr/lib/python2.7/site-packages/sm_client/shell.py
/usr/lib/python2.7/site-packages/sm_client/shell.pyc
/usr/lib/python2.7/site-packages/sm_client/shell.pyo
/usr/lib/python2.7/site-packages/sm_client/v1
/usr/lib/python2.7/site-packages/sm_client/v1/__init__.py
/usr/lib/python2.7/site-packages/sm_client/v1/__init__.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/__init__.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/client.py
/usr/lib/python2.7/site-packages/sm_client/v1/client.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/client.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/shell.py
/usr/lib/python2.7/site-packages/sm_client/v1/shell.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/shell.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/sm_nodes.py
/usr/lib/python2.7/site-packages/sm_client/v1/sm_nodes.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/sm_nodes.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/sm_sda.py
/usr/lib/python2.7/site-packages/sm_client/v1/sm_sda.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/sm_sda.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service.py
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_node.py
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_node.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_node.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_node_shell.py
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_node_shell.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_node_shell.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_shell.py
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_shell.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/smc_service_shell.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/smc_servicegroup.py
/usr/lib/python2.7/site-packages/sm_client/v1/smc_servicegroup.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/smc_servicegroup.pyo
/usr/lib/python2.7/site-packages/sm_client/v1/smc_servicegroup_shell.py
/usr/lib/python2.7/site-packages/sm_client/v1/smc_servicegroup_shell.pyc
/usr/lib/python2.7/site-packages/sm_client/v1/smc_servicegroup_shell.pyo
```

```sh
localhost:~$ rpm -ql sm-api-1.0-4.tis.x86_64
/etc/init.d/sm-api
/etc/pmon.d/sm-api.conf
/etc/sm
/etc/sm-api/policy.json
/etc/sm/sm_api.ini
/usr/bin/sm-api
/usr/lib/python2.7/site-packages/sm_api
/usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info
/usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info/PKG-INFO
/usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info/SOURCES.txt
/usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info/dependency_links.txt
/usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info/entry_points.txt
/usr/lib/python2.7/site-packages/sm_api-1.0.0-py2.7.egg-info/top_level.txt
/usr/lib/python2.7/site-packages/sm_api/__init__.py
/usr/lib/python2.7/site-packages/sm_api/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/api
/usr/lib/python2.7/site-packages/sm_api/api/__init__.py
/usr/lib/python2.7/site-packages/sm_api/api/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/api/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/api/acl.py
/usr/lib/python2.7/site-packages/sm_api/api/acl.pyc
/usr/lib/python2.7/site-packages/sm_api/api/acl.pyo
/usr/lib/python2.7/site-packages/sm_api/api/api.py
/usr/lib/python2.7/site-packages/sm_api/api/api.pyc
/usr/lib/python2.7/site-packages/sm_api/api/api.pyo
/usr/lib/python2.7/site-packages/sm_api/api/app.py
/usr/lib/python2.7/site-packages/sm_api/api/app.pyc
/usr/lib/python2.7/site-packages/sm_api/api/app.pyo
/usr/lib/python2.7/site-packages/sm_api/api/config.py
/usr/lib/python2.7/site-packages/sm_api/api/config.pyc
/usr/lib/python2.7/site-packages/sm_api/api/config.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers
/usr/lib/python2.7/site-packages/sm_api/api/controllers/__init__.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/root.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/root.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/root.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/__init__.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/base.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/base.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/base.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/collection.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/collection.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/collection.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/link.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/link.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/link.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/nodes.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/nodes.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/nodes.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/service_groups.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/service_groups.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/service_groups.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/servicenode.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/servicenode.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/servicenode.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/services.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/services.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/services.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/sm_sda.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/sm_sda.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/sm_sda.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/smc_api.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/smc_api.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/smc_api.pyo
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/utils.py
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/utils.pyc
/usr/lib/python2.7/site-packages/sm_api/api/controllers/v1/utils.pyo
/usr/lib/python2.7/site-packages/sm_api/api/hooks.py
/usr/lib/python2.7/site-packages/sm_api/api/hooks.pyc
/usr/lib/python2.7/site-packages/sm_api/api/hooks.pyo
/usr/lib/python2.7/site-packages/sm_api/api/middleware
/usr/lib/python2.7/site-packages/sm_api/api/middleware/__init__.py
/usr/lib/python2.7/site-packages/sm_api/api/middleware/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/api/middleware/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/api/middleware/auth_token.py
/usr/lib/python2.7/site-packages/sm_api/api/middleware/auth_token.pyc
/usr/lib/python2.7/site-packages/sm_api/api/middleware/auth_token.pyo
/usr/lib/python2.7/site-packages/sm_api/api/middleware/parsable_error.py
/usr/lib/python2.7/site-packages/sm_api/api/middleware/parsable_error.pyc
/usr/lib/python2.7/site-packages/sm_api/api/middleware/parsable_error.pyo
/usr/lib/python2.7/site-packages/sm_api/cmd
/usr/lib/python2.7/site-packages/sm_api/cmd/__init__.py
/usr/lib/python2.7/site-packages/sm_api/cmd/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/cmd/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/cmd/api.py
/usr/lib/python2.7/site-packages/sm_api/cmd/api.pyc
/usr/lib/python2.7/site-packages/sm_api/cmd/api.pyo
/usr/lib/python2.7/site-packages/sm_api/common
/usr/lib/python2.7/site-packages/sm_api/common/__init__.py
/usr/lib/python2.7/site-packages/sm_api/common/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/common/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/common/config.py
/usr/lib/python2.7/site-packages/sm_api/common/config.pyc
/usr/lib/python2.7/site-packages/sm_api/common/config.pyo
/usr/lib/python2.7/site-packages/sm_api/common/context.py
/usr/lib/python2.7/site-packages/sm_api/common/context.pyc
/usr/lib/python2.7/site-packages/sm_api/common/context.pyo
/usr/lib/python2.7/site-packages/sm_api/common/exception.py
/usr/lib/python2.7/site-packages/sm_api/common/exception.pyc
/usr/lib/python2.7/site-packages/sm_api/common/exception.pyo
/usr/lib/python2.7/site-packages/sm_api/common/log.py
/usr/lib/python2.7/site-packages/sm_api/common/log.pyc
/usr/lib/python2.7/site-packages/sm_api/common/log.pyo
/usr/lib/python2.7/site-packages/sm_api/common/policy.py
/usr/lib/python2.7/site-packages/sm_api/common/policy.pyc
/usr/lib/python2.7/site-packages/sm_api/common/policy.pyo
/usr/lib/python2.7/site-packages/sm_api/common/safe_utils.py
/usr/lib/python2.7/site-packages/sm_api/common/safe_utils.pyc
/usr/lib/python2.7/site-packages/sm_api/common/safe_utils.pyo
/usr/lib/python2.7/site-packages/sm_api/common/service.py
/usr/lib/python2.7/site-packages/sm_api/common/service.pyc
/usr/lib/python2.7/site-packages/sm_api/common/service.pyo
/usr/lib/python2.7/site-packages/sm_api/common/utils.py
/usr/lib/python2.7/site-packages/sm_api/common/utils.pyc
/usr/lib/python2.7/site-packages/sm_api/common/utils.pyo
/usr/lib/python2.7/site-packages/sm_api/db
/usr/lib/python2.7/site-packages/sm_api/db/__init__.py
/usr/lib/python2.7/site-packages/sm_api/db/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/db/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/db/api.py
/usr/lib/python2.7/site-packages/sm_api/db/api.pyc
/usr/lib/python2.7/site-packages/sm_api/db/api.pyo
/usr/lib/python2.7/site-packages/sm_api/db/migration.py
/usr/lib/python2.7/site-packages/sm_api/db/migration.pyc
/usr/lib/python2.7/site-packages/sm_api/db/migration.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/__init__.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/api.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/api.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/api.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/__init__.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/manage.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/manage.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/manage.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/001_init.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/001_init.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/001_init.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/__init__.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migrate_repo/versions/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migration.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migration.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/migration.pyo
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/models.py
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/models.pyc
/usr/lib/python2.7/site-packages/sm_api/db/sqlalchemy/models.pyo
/usr/lib/python2.7/site-packages/sm_api/objects
/usr/lib/python2.7/site-packages/sm_api/objects/__init__.py
/usr/lib/python2.7/site-packages/sm_api/objects/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/base.py
/usr/lib/python2.7/site-packages/sm_api/objects/base.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/base.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/smo_node.py
/usr/lib/python2.7/site-packages/sm_api/objects/smo_node.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/smo_node.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sda.py
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sda.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sda.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sdm.py
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sdm.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sdm.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/smo_service.py
/usr/lib/python2.7/site-packages/sm_api/objects/smo_service.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/smo_service.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/smo_servicegroup.py
/usr/lib/python2.7/site-packages/sm_api/objects/smo_servicegroup.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/smo_servicegroup.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sgm.py
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sgm.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/smo_sgm.pyo
/usr/lib/python2.7/site-packages/sm_api/objects/utils.py
/usr/lib/python2.7/site-packages/sm_api/objects/utils.pyc
/usr/lib/python2.7/site-packages/sm_api/objects/utils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack
/usr/lib/python2.7/site-packages/sm_api/openstack/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common
/usr/lib/python2.7/site-packages/sm_api/openstack/common/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/cliutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/cliutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/cliutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/config
/usr/lib/python2.7/site-packages/sm_api/openstack/common/config/generator.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/config/generator.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/config/generator.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/context.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/context.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/context.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/api.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/api.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/api.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/exception.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/exception.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/exception.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/models.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/models.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/models.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/session.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/session.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/session.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/utils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/utils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/db/sqlalchemy/utils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/eventlet_backdoor.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/eventlet_backdoor.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/eventlet_backdoor.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/excutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/excutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/excutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fileutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fileutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fileutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/mockpatch.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/mockpatch.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/mockpatch.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/moxstubout.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/moxstubout.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/fixture/moxstubout.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/gettextutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/gettextutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/gettextutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/importutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/importutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/importutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/jsonutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/jsonutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/jsonutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/local.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/local.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/local.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/lockutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/lockutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/lockutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/log.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/log.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/log.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/log_handler.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/log_handler.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/log_handler.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/loopingcall.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/loopingcall.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/loopingcall.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/network_utils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/network_utils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/network_utils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/api.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/api.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/api.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/log_notifier.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/log_notifier.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/log_notifier.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/no_op_notifier.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/no_op_notifier.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/no_op_notifier.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier2.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier2.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/rpc_notifier2.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/test_notifier.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/test_notifier.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/notifier/test_notifier.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/periodic_task.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/periodic_task.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/periodic_task.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/policy.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/policy.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/policy.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/processutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/processutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/processutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/cmd.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/cmd.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/cmd.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/filters.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/filters.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/filters.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/wrapper.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/wrapper.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rootwrap/wrapper.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/__init__.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/__init__.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/__init__.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/amqp.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/amqp.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/amqp.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/common.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/common.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/common.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/dispatcher.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/dispatcher.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/dispatcher.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_fake.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_fake.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_fake.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_kombu.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_kombu.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_kombu.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_qpid.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_qpid.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_qpid.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_zmq.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_zmq.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/impl_zmq.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_redis.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_redis.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_redis.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_ring.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_ring.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/matchmaker_ring.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/proxy.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/proxy.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/proxy.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/serializer.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/serializer.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/serializer.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/service.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/service.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/service.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/zmq_receiver.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/zmq_receiver.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/rpc/zmq_receiver.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/service.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/service.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/service.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/setup.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/setup.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/setup.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/strutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/strutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/strutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/threadgroup.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/threadgroup.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/threadgroup.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/timeutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/timeutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/timeutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/uuidutils.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/uuidutils.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/uuidutils.pyo
/usr/lib/python2.7/site-packages/sm_api/openstack/common/version.py
/usr/lib/python2.7/site-packages/sm_api/openstack/common/version.pyc
/usr/lib/python2.7/site-packages/sm_api/openstack/common/version.pyo
/usr/lib/systemd/system/sm-api.service
```

```sh
localhost:~$ sudo /etc/init.d/sm-api stop
Stopping sm-api (via systemctl):                           [  OK  ]
localhost:~$ sudo /etc/init.d/sm-api status
sm-api is running; no pidfile
localhost:~$ sudo /etc/init.d/sm-api start
Starting sm-api (via systemctl):                           [  OK  ]
localhost:~$ sudo /etc/init.d/sm-api status
sm-api is running
```

### sm-tools

```sh
localhost:~$ rpm -ql sm-tools-1.0-2.tis.x86_64
/usr/bin/sm-configure
/usr/bin/sm-deprovision
/usr/bin/sm-dump
/usr/bin/sm-iface-state
/usr/bin/sm-manage
/usr/bin/sm-provision
/usr/bin/sm-query
/usr/bin/sm-restart
/usr/bin/sm-restart-safe
/usr/bin/sm-unmanage
/usr/lib/python2.7/site-packages/sm_tools
/usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info
/usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info/PKG-INFO
/usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info/SOURCES.txt
/usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info/dependency_links.txt
/usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info/entry_points.txt
/usr/lib/python2.7/site-packages/sm_tools-1.0.0-py2.7.egg-info/top_level.txt
/usr/lib/python2.7/site-packages/sm_tools/__init__.py
/usr/lib/python2.7/site-packages/sm_tools/__init__.pyc
/usr/lib/python2.7/site-packages/sm_tools/__init__.pyo
/usr/lib/python2.7/site-packages/sm_tools/sm_action.py
/usr/lib/python2.7/site-packages/sm_tools/sm_action.pyc
/usr/lib/python2.7/site-packages/sm_tools/sm_action.pyo
/usr/lib/python2.7/site-packages/sm_tools/sm_api_msg_utils.py
/usr/lib/python2.7/site-packages/sm_tools/sm_api_msg_utils.pyc
/usr/lib/python2.7/site-packages/sm_tools/sm_api_msg_utils.pyo
/usr/lib/python2.7/site-packages/sm_tools/sm_configure.py
/usr/lib/python2.7/site-packages/sm_tools/sm_configure.pyc
/usr/lib/python2.7/site-packages/sm_tools/sm_configure.pyo
/usr/lib/python2.7/site-packages/sm_tools/sm_dump.py
/usr/lib/python2.7/site-packages/sm_tools/sm_dump.pyc
/usr/lib/python2.7/site-packages/sm_tools/sm_dump.pyo
/usr/lib/python2.7/site-packages/sm_tools/sm_provision.py
/usr/lib/python2.7/site-packages/sm_tools/sm_provision.pyc
/usr/lib/python2.7/site-packages/sm_tools/sm_provision.pyo
/usr/lib/python2.7/site-packages/sm_tools/sm_query.py
/usr/lib/python2.7/site-packages/sm_tools/sm_query.pyc
/usr/lib/python2.7/site-packages/sm_tools/sm_query.pyo
```

```sh
localhost:~$ sm-dump 
/var/run/sm/sm.db not available.
```

### sm

```sh
localhost:~$ rpm -ql sm-1.0.0-33.tis.x86_64
/etc/init.d/sm
/etc/init.d/sm-shutdown
/etc/logrotate.d/sm.logrotate
/etc/pmon.d/sm.conf
/usr/bin/sm
/usr/lib/systemd/system/sm-shutdown.service
/usr/lib/systemd/system/sm.service
/usr/local/sbin/sm-notification
/usr/local/sbin/sm-notify
/usr/local/sbin/sm-troubleshoot
/usr/share/licenses/sm-1.0.0
/usr/share/licenses/sm-1.0.0/LICENSE
```

```sh
localhost:~$ sudo /etc/init.d/sm status
sm is running
localhost:~$ sudo /etc/init.d/sm stop
Stopping sm (via systemctl):                               [  OK  ]
localhost:~$ sudo /etc/init.d/sm status
sm is not running
localhost:~$ sudo /etc/init.d/sm start
Starting sm (via systemctl):                               [  OK  ]
```

### sm-db

```sh
localhost:~$ rpm -ql sm-db-1.0.0-31.tis.x86_64
/usr/lib64/libsm_db.so.1
/usr/lib64/libsm_db.so.1.0.0
/usr/share/licenses/sm-db-1.0.0
/usr/share/licenses/sm-db-1.0.0/LICENSE
/var/lib/sm/patches/sm-patch.sql
/var/lib/sm/sm.db
/var/lib/sm/sm.hb.db
```

### sm-common-libs

```sh
localhost:~$ rpm -ql sm-common-libs-1.0.0-20.tis.x86_64
/usr/lib64/libsm_common.so.1
/usr/lib64/libsm_common.so.1.0.0
/var/lib/sm
/var/lib/sm/watchdog
/var/lib/sm/watchdog/modules
/var/lib/sm/watchdog/modules/libsm_watchdog_nfs.so.1
/var/lib/sm/watchdog/modules/libsm_watchdog_nfs.so.1.0.0
```

## sm-common

```sh
localhost:~$ rpm -ql sm-common-1.0.0-20.tis.x86_64
/etc/init.d/sm-watchdog
/etc/pmon.d/sm-watchdog.conf
/usr/bin/sm-watchdog
/usr/lib/systemd/system/sm-watchdog.service
/usr/share/licenses/sm-common-1.0.0
/usr/share/licenses/sm-common-1.0.0/LICENSE
```

```sh
localhost:~$ sudo /etc/init.d/sm-watchdog stop
Stopping sm-watchdog (via systemctl):                      [  OK  ]
localhost:~$ /etc/init.d/sm-watchdog status
sm-watchdog is not running
localhost:~$ sudo /etc/init.d/sm-watchdog start
Starting sm-watchdog (via systemctl):                      [  OK  ]
localhost:~$ /etc/init.d/sm-watchdog status
sm-watchdog is running
```

## Step Two

Source Code

```sh
[user@22b9459ea3f6 starlingx]$ repo grep SmPuppet
cgcs-root/stx/config/sysinv/sysinv/sysinv/setup.cfg:    031_smapi = sysinv.puppet.smapi:SmPuppet
cgcs-root/stx/config/sysinv/sysinv/sysinv/sysinv/puppet/smapi.py:class SmPuppet(openstack.OpenstackBasePuppet):
```

```sh
systemconfig.puppet_plugins = 
    031_smapi = sysinv.puppet.smapi:SmPuppet
```

```sh
A single instance of :py:class:`sysinv.conductor.manager.ConductorManager` is
created within the *sysinv-conductor* process, and is responsible for
performing all actions for hosts managed by system inventory.
Commands are received via RPC calls. The conductor service also performs
collection of inventory data for each host.

cgcs-root/stx/config/sysinv/sysinv/sysinv/sysinv/
cgcs-root/stx/metal/inventory/inventory/inventory/
```

## Step Three

```sh

```
