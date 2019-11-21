# StarlingX In A Box

From [Add StarlingX in a Box guide](https://review.opendev.org/#/c/692202/)

> My comments under block quotes.

## Hardware and VM requirements

> No changes.

## Install VM using qcow2 images

### Install virt-manager on host

> No changes.

### Create VM from qcow2 images

> See proposed changes for some steps

11. Once the VM is up, log in to the VM using SSH using the following credentials and IP address.

    > You will get a message that password has expired, and you need to change it.

    ```
    User Name: sysadmin
    Password: St8rlingX*
    IP Address: 10.10.10.2
    ```

    ```sh
    user@workstation:~/starlingx/opendev/docs$ ssh sysadmin@10.10.10.2
    Release 19.09
    ------------------------------------------------------------------------
    W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
    ------------------------------------------------------------------------
    THIS IS A PRIVATE COMPUTER SYSTEM.
    This computer system including all related equipment, network devices
    (specifically including Internet access), are provided only for authorized use.
    All computer systems may be monitored for all lawful purposes, including to
    ensure that their use is authorized, for management of the system, to
    facilitate protection against unauthorized access, and to verify security
    procedures, survivability and operational security. Monitoring includes active
    attacks by authorized personnel and their entities to test or verify the
    security of the system. During monitoring, information may be examined,
    recorded, copied and used for authorized purposes. All information including
    personal information, placed on or sent over this system may be monitored. Uses
    of this system, authorized or unauthorized, constitutes consent to monitoring
    of this system. Unauthorized use may subject you to criminal prosecution.
    Evidence of any such unauthorized use collected during monitoring may be used
    for administrative, criminal or other adverse action. Use of this system
    constitutes consent to monitoring for these purposes.

    sysadmin@10.10.10.2's password: St8rlingX*
    ```

    Once we hit enter, we are required to change password:

    ```
    WARNING: Unauthorized access to this system is forbidden and will be
    prosecuted by law. By accessing this system, you agree that your
    actions may be monitored if unauthorized usage is suspected.

    WARNING: Your password has expired.
    You must change your password now and login again!
    Changing password for user sysadmin.
    Changing password for sysadmin.
    (current) UNIX password: St8rlingX*
    New password: Some4Password*
    Retype new password: Some4Password*
    passwd: all authentication tokens updated successfully.
    Connection to 10.10.10.2 closed.
    ```

## Verify the installation

```sh
controller-0:~$ source /etc/platform/openrc
[sysadmin@controller-0 ~(keystone_admin)]$ 
```

```
[sysadmin@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[sysadmin@controller-0 ~(keystone_admin)]$ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
controller-0   Ready    master   58d   v1.15.3
```

### Access the StarlingX Horizon GUI

No changes.

### Access the StarlingX Kubernetes GUI

1. Access the Kubernetes Dashboard GUI in your browser at the following address.

> For __Note__ _You may encounter security warnings. Click on Advanced and then Continue._, click on [this](https://user-images.githubusercontent.com/159464/63768974-48201a80-c8da-11e9-84ba-2c8c24daea06.png) link to see an image example of the message presented, you click on _Advanced_ first and then on _Continue_ to access the StarlingX Kubernetes GUI.

3. Use an existing token or generate a new token to log in.

> Tested both methods

```sh
controller-0:~$ kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
Name:         admin-user-token-2q7k8
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: admin-user
              kubernetes.io/service-account.uid: 353b957b-afa8-4e01-80ce-f0f76d95f601

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLTJxN2s4Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIzNTNiOTU3Yi1hZmE4LTRlMDEtODBjZS1mMGY3NmQ5NWY2MDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06YWRtaW4tdXNlciJ9.5oxtIQ8OSkrSLv3gnTJBOD4GvPNA1gNoDXhwcZKBx7NUefx1xvhth9XhU1PFdDT4diTcBQGwCKkcoHhYe1DRvmR_T-w79UPM6PBusA_8uTNw0iH0GyXGKWZUqFfSReFhKbfx7meuSkm3EttNLZtPLK6Kkbw_UpBRMOD40uaJ4g8mIgD5tcqkbZQPgUDUSq8ESXj7s21Z7iy5zrCLkSo7ewRMzrVfLtR7Y0JFyq6PgnqorMazbuXdHdxf9-lO9OKoMtaTHYe0jqE4lBY1b10Vhp7sfwXv5uDirVWOiWhz6NXpmZnV1cocPmJ4VN8maISiRu4hBBF0ERgeXrk4mZggNA
ca.crt:     1025 bytes
namespace:  11 bytes
```

> After succesfully logged in into Kubernetes Dashboard, we will see several error messages for EdgeX pods "One or more pods have errors", should we notify?

```
Failed create pod sandbox: 
MountVolume.SetUp failed for volume "default-token-tjzd5"
Etc...
```

### EdgeX Deployment

1. Uninstall EdgeX.

> Below is the text output to replace _stx-in-box-edgex-uninstall.jpg_

```sh
controller-0:~$ ./edgex-on-kubernetes-master/hack/edgex-down.sh 
Deleting EdgeX deployments now!
deployment.extensions "edgex-core-consul" deleted
deployment.extensions "rulesengine" deleted
deployment.extensions "edgex-export-distro" deleted
deployment.extensions "edgex-export-client" deleted
deployment.extensions "edgex-support-scheduler" deleted
deployment.extensions "edgex-core-command" deleted
deployment.extensions "edgex-core-data" deleted
deployment.extensions "edgex-core-metadata" deleted
deployment.extensions "edgex-support-notifications" deleted
deployment.extensions "edgex-support-logging" deleted
deployment.extensions "edgex-mongo" deleted
EdgeX deployments created successfully!
Deleting EdgeX services now!
service "edgex-core-consul" deleted
service "edgex-support-rulesengine" deleted
service "edgex-export-distro" deleted
service "edgex-export-client" deleted
service "edgex-support-scheduler" deleted
service "edgex-core-command" deleted
service "edgex-core-data" deleted
service "edgex-core-metadata" deleted
service "edgex-support-notifications" deleted
service "edgex-support-logging" deleted
service "edgex-mongo" deleted
EdgeX services deleted successfully !
```

2. Install EdgeX.

> Below is the output to replace _stx-in-box-edgex-install.jpg_

```sh
controller-0:~$ ./edgex-on-kubernetes-master/hack/edgex-up.sh
Creating EdgeX services now!
service/edgex-core-consul created
service/edgex-mongo created
service/edgex-support-logging created
service/edgex-support-notifications created
service/edgex-core-metadata created
service/edgex-core-data created
service/edgex-core-command created
service/edgex-support-scheduler created
service/edgex-export-client created
service/edgex-export-distro created
service/edgex-support-rulesengine created
EdgeX services created successfully !
Creating EdgeX deployments now!
deployment.extensions/edgex-core-consul created
deployment.extensions/edgex-mongo created
deployment.extensions/edgex-support-logging created
deployment.extensions/edgex-support-notifications created
deployment.extensions/edgex-core-metadata created
deployment.extensions/edgex-core-data created
deployment.extensions/edgex-core-command created
deployment.extensions/edgex-support-scheduler created
deployment.extensions/edgex-export-client created
deployment.extensions/edgex-export-distro created
deployment.extensions/rulesengine created
EdgeX deployments created successfully!
```

3. Check the EdgeX deployment.

> Below is the output to replace _stx-in-box-edgex-check.jpg_

```sh
edgex-core-command-5f5c5fdf76-nrf2r               1/1     Running            2          2m49s
edgex-core-consul-5878c69b76-cvvql                0/1     ErrImagePull       0          3m55s
edgex-core-data-6cf777656-fstfg                   1/1     Running            2          3m1s
edgex-core-metadata-cfc8b5cd7-2mvx7               1/1     Running            2          3m12s
edgex-export-client-7dc8cd7d69-qt66q              1/1     Running            1          2m27s
edgex-export-distro-6f59d74596-p2p7r              1/1     Running            0          2m16s
edgex-mongo-7d66895d45-sfvds                      1/1     Running            0          3m45s
edgex-support-logging-567998f4fc-5xwcw            0/1     CrashLoopBackOff   2          3m34s
edgex-support-notifications-987746788-dwpp7       1/1     Running            2          3m23s
edgex-support-scheduler-5588cb5899-2bprf          0/1     Error              1          2m38s
```
