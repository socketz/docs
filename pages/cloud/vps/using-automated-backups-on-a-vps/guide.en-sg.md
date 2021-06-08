---
title: 'Using automatic backups on a VPS'
slug: using-automated-backups-on-a-vps
excerpt: 'Find out how to enable and use the Automated Backup option in the OVHcloud Control Panel'
section: 'Backup options'
order: 2
---

**Last updated 2020/09/23**


## Objective

This option offers a convenient way to frequently have complete VPS backups available from your OVHcloud Control Panel without having to connect to the server to create and restore them manually. Another advantage is that you can also choose to mount a backup and then access it via SSH.

**This guide explains the usage of auto-backups for your OVHcloud VPS.**

> [!primary]
>
Before applying backup options, we recommend to consult the [product pages and FAQ](https://www.ovhcloud.com/en-sg/vps/options/) for pricing comparisons and further details.
>

## Requirements

- access to the [OVHcloud Control Panel](https://ca.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/sg/&ovhSubsidiary=sg)
- an OVHcloud [VPS service](https://www.ovhcloud.com/en-sg/vps/) already set up
- administrative access (root) via SSH to your VPS (optional)

## Instructions

Log in to your [OVHcloud Control Panel](https://ca.ovh.com/auth/?action=gotomanager&from=https://www.ovh.com/sg/&ovhSubsidiary=sg), navigate to the "Bare Metal Cloud" section, and select your server from the left-hand sidebar under `Virtual Private Servers`{.action}.

### Step 1: Subscribing to the Automated backups option

After selecting your VPS, click on the `Automated backup`{.action} tab in the horizontal menu.

In the next step, please take note of the pricing information, then click on `Order`{.action}. You will be guided through the order process and receive a confirmation email. Backups will now be created daily until the option is cancelled again.


### Step 2: Restoring a backup from the OVHcloud Control Panel

After selecting your VPS, click on the `Automated backup`{.action} tab in the horizontal menu. There will be a maximum of 15 daily backups available. Click on `...`{.action} next to the backup you would like to restore and select `Restoration`{.action}.

![autobackupvps](images/backup_vps_step1.png){.thumbnail}

If you recently changed your root password, make sure to tick the option "Modify the root password on restoration" in the popup window to preserve your current root password and click on `Confirm`{.action}. You will receive an email as soon as the task is complete. The restoration might take a while, depending on the disk space used.

> [!alert]
>
Please note that the automated backups will not include your additional disks.
>

### How to mount and access a backup

It is not necessary to completely overwrite your existing service with a restoration. The "Mounting" option allows you to access the backup data to retrieve your files. 

> [!warning]
>OVHcloud is providing you with services for which you are responsible, with regard to their configuration and management. You are therefore responsible for ensuring they function correctly.
>
>This guide is designed to assist you in common tasks as much as possible. Nevertheless, we recommend contacting a specialised provider and/or the software publisher for the service if you encounter any difficulties. We will not be able to assist you ourselves. You can find more information in the “Go further” section of this guide.
>

#### Step 1: Control Panel 

Click on `...`{.action} next to the backup you need to access and select `Mounting`{.action}.

![autobackupvps](images/backup_vps_step2.png){.thumbnail}

After the process is completed, you will receive an email. You can now connect to your VPS and add the partition where your backup is located.

#### Step 2: Secure Shell

First, connect to your VPS via SSH.

You can use the following command to verify the name of the newly attached device:

```
$ lsblk
```

Here is a sample output of this command:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda       8:0    0   25G  0 disk 
├─sda1    8:1    0 24.9G  0 part /
├─sda14   8:14   0    4M  0 part 
└─sda15   8:15   0  106M  0 part 
sdb       8:16   0   25G  0 disk 
├─sdb1    8:17   0 24.9G  0 part 
├─sdb14   8:30   0    4M  0 part 
└─sdb15   8:31   0  106M  0 part /boot/efi
```
In this example, the partition containing your backup filesystem is named "sdb1".
Next, create a directory for this partition and define it as the mountpoint:

```
$ mkdir -p /mnt/restore
$ mount /dev/sdb1 /mnt/restore
```

You can now switch to this folder and access your backup data.

### Best practice for using auto-backups

The Automated Backup functionality is based on VPS snapshots. We recommend to follow the steps below to prevent any issues before using this option.

#### Configuring the QEMU agent on a VPS

Snapshots are instantaneous images of your running system ("live snapshot"). To ensure the availability of your system when the snapshot is created, the QEMU agent is used to prepare the filesystem for the process.

The required *qemu-guest-agent* is not installed by default on most distributions. Moreover, licensing restrictions may prevent OVHcloud from including it in the available OS images. Therefore, it is best practice to verify and install the agent in case it is not activated on your VPS. Connect to your VPS via SSH and follow the instructions below, according to your operating system.

##### **Debian-based distributions (Debian, Ubuntu)**

Use the following command to check whether the system is properly set up for snapshots:

```
$ file /dev/virtio-ports/org.qemu.guest_agent.0
/dev/virtio-ports/org.qemu.guest_agent.0: symbolic link to ../vport2p1
```
If the output is different ("No such file or directory"), install the latest package:

```
$ sudo apt-get update
$ sudo apt-get install qemu-guest-agent
```

Reboot the VPS:

```
$ sudo restart
```

Check the service to ensure it is running:

```
$ sudo service qemu-guest-agent status
```

##### **Redhat-based distributions (Centos, Fedora)**

Use the following command to check whether the system is properly set up for snapshots:

```
$ file /dev/virtio-ports/org.qemu.guest_agent.0
/dev/virtio-ports/org.qemu.guest_agent.0: symbolic link to ../vport2p1
```

If the output is different ("No such file or directory"), install and enable the agent:

```
$ sudo yum install qemu-guest-agent
$ sudo chkconfig qemu-guest-agent on
```

Reboot the VPS:

```
$ sudo reboot
```

Check the agent and verify that it is running:

```
$ sudo service qemu-guest-agent status
```

##### **Windows**

You can install the agent via MSI file, available from the Fedora project website: <https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-qemu-ga/>

Verify that the service is running by using this powershell command:

```
PS C:\Users\Administrator> Get-Service QEMU-GA
Status   Name               DisplayName
------   ----               -----------
Running  QEMU-GA            QEMU Guest Agent
```

## Go further

[Using snapshots on a VPS](../using-snapshots-on-a-vps)


Join our community of users on <https://community.ovh.com/en/>.
