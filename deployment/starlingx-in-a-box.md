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

