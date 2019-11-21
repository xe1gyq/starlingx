# StarlingX In A Box

## Hardware and VM requirements

All steps succeded.

## Install VM using qcow2 images

### Install virt-manager on host

All steps succeded.

### Create VM from qcow2 images

> See proposed changes for some steps

10. Start the VM.
    ```
    virsh create simplex-controller-1.xml
    ```
    It will take 5 – 10 min to start the VM, depending on your processor.

    > Tbd

11. Once the VM is up, log in to the VM using SSH using the following credentials and IP address.

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
