# Filesystems

- Create FS `mkfs -t ext4 /dev/sda1`
- BTRFS `mkfs -t btrfs /dev/sda2`

## SystemD mounts

```
/run/systemd/generator$ ls -l
total 12
-rw-r--r-- 1 root root 231 Sep  1 08:13 -.mount
-rw-r--r-- 1 root root 228 Sep  1 08:13 boot-efi.mount
drwxr-xr-x 2 root root  60 Sep  1 08:13 getty.target.wants
drwxr-xr-x 2 root root  80 Sep  1 08:13 local-fs.target.requires
-rw-r--r-- 1 root root 275 Sep  1 08:13 mnt-samba.mount
drwxr-xr-x 2 root root  60 Sep  1 08:13 multi-user.target.wants
-rw-r--r-- 1 root root   0 Sep  1 08:13 netplan.stamp
drwxr-xr-x 2 root root  60 Sep  1 08:13 network-online.target.wants
drwxr-xr-x 2 root root  60 Sep  1 08:13 remote-fs.target.requires
```

## SystemD Automounts

- You need to use the same name as with the mount: i.e: `mnt-books.mount` and `mnt-books.automount`.

```
sudo systemctl  list-unit-files | grep automount
proc-sys-fs-binfmt_misc.automount      static

sudo systemctl cat proc-sys-fs-binfmt_misc.automount 
# /lib/systemd/system/proc-sys-fs-binfmt_misc.automount
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Arbitrary Executable File Formats File System Automount Point
Documentation=https://www.kernel.org/doc/html/latest/admin-guide/binfmt-misc.html
Documentation=https://www.freedesktop.org/wiki/Software/systemd/APIFileSystems
DefaultDependencies=no
Before=sysinit.target
ConditionPathExists=/proc/sys/fs/binfmt_misc/
ConditionPathIsReadWrite=/proc/sys/

[Automount]
Where=/proc/sys/fs/binfmt_misc
```

- Now to add `mnt-books.automount`

```
cat<<EOF>>/etc/systemd/system/mnt-books.automount
[Automount]
Where=/mnt/automount
EOF
sudo systemctl disable --now mnt-books.mount
sudo systemctl enable --now mnt-books.automount
mount | grep books
systemd-1 on /mnt/books type autofs (rw,relatime,fd=41,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=77182)
cd /mnt/books
mount | grep books
systemd-1 on /mnt/books type autofs (rw,relatime,fd=41,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=77182)
/dev/mapper/mirror-books on /mnt/books type ext4 (rw,relatime,data=ordered)
```


# EXT4

- Create a filesystem with larger number of inodes, in case you have smaller files:

```
mkfs.ext4 -N 262144 /dev/sdb1
 sudo tune2fs -l /dev/mapper/mirror-books 
tune2fs 1.44.1 (24-Mar-2018)
Filesystem volume name:   <none>
Last mounted on:          <not available>
Filesystem UUID:          3c900961-acad-4225-b5cf-7ad497956590
Filesystem magic number:  0xEF53
Filesystem revision #:    1 (dynamic)
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent 64bit flex_bg sparse_super large_file huge_file dir_nlink extra_isize metadata_csum
Filesystem flags:         signed_directory_hash 
Default mount options:    user_xattr acl
Filesystem state:         clean
Errors behavior:          Continue
Filesystem OS type:       Linux
Inode count:              285600
Block count:              1141760
Reserved block count:     57088
Free blocks:              1102978
Free inodes:              285589
First block:              0
Block size:               4096
Fragment size:            4096
Group descriptor size:    64
Reserved GDT blocks:      557
Blocks per group:         32768
Fragments per group:      32768
Inodes per group:         8160
Inode blocks per group:   510
Flex block group size:    16
Filesystem created:       Mon Aug 31 11:32:58 2020
Last mount time:          Mon Aug 31 11:34:41 2020
Last write time:          Mon Aug 31 11:34:41 2020
Mount count:              1
Maximum mount count:      -1
Last checked:             Mon Aug 31 11:32:58 2020
Check interval:           0 (<none>)
Lifetime writes:          66 MB
Reserved blocks uid:      0 (user root)
Reserved blocks gid:      0 (group root)
First inode:              11
Inode size:	          256
Required extra isize:     32
Desired extra isize:      32
Journal inode:            8
Default directory hash:   half_md4
Directory Hash Seed:      01792d49-cd24-4461-9b1f-91450123b287
Journal backup:           inode blocks
Checksum type:            crc32c
Checksum:                 0x36733c7f

```

# XFS


- Apt install xfsdump
- To add a label `xfs_admin -L videos /dev/sda5`


```
xfs_info  /mnt/mountpoint
xfs_repair -n /dev/nvme1p1 (unmounted)
xfs_dump 
xfs_restore
LABEL=videos /videos xfs defaults 0 0
```

- Level0 (Full backup)
- Level2 (incremental


Xfsdump -l 0 -f  /mnt/data.xfsdump /dev/nvme1n1p5
Xfsrestore -f /mnt/data.xfsdump /mnt/<MOUNTPOINT>

xfrestore -I (selective restore)


# BTRFS

- Subvolumes can't have a different unlike LVM.
```
mkfs -t btrfs /dev/nvme1n1p6
mount -t btrfs /dev/nvme1n1p6 /mnt/btrfs
btrfs device add /dev/nvme1n1p7 /mnt/btrfs
btrfs filesystem show /mnt/btrfs

# This needs to be done when you add new drives.
btrfs balance start --full-balance  /mnt/btrfs

# Convert ext2,3,4 to btrfs, unmount the filesystem
#  block size needs 4096

btrfs-convert /dev/nvme1n1p2
mkfs.ext -b 4096 /dev/nvme1n1p2

```

## Subvolumes

- can be mounted as a different filesystem

```
btrfs subvolume create /mnt/btrfs/sovl1
btrfs subvolume list /mnt/btrfs
mkdir /mnt/sovl1 
mount -t btrfs -o subvol=svol1 /dev/nvme1n1p6 /mnt/svol1
```

## Snapshots
- picture of a subvolume in a point in time.
- copy-on-write, only takes  space when they are modified.

```
btrfs subvolume snapshot /mnt/btrfs/svol1   /mnt/btrfs/snap
btrfs subvolume snapshot -r /mnt/btrfs/svol1 /mnt/btrfs/snap_ro
```

## ZFS

- Pooled storage
- Copy-on-write, the filesystem's metada is update only on write.
- Snapshots
- Data integrity
- We create zfs pools and attach vdev.

## smartd/smartcl

- apt install smartmontools

```
# info
smartctl -i /dev/nvme1n1
smartctl -s on /dev/nvmen1n1
smartctl -a /dev/nvmen1n1
smartctl -c /dev/nvmen1n1
smartctl -t /dev/sda1

# you can configure checks
systemctl start smartd
```

## Autofs
- Configure `/etc/auto.master` (requires autofs package)

```
#mountput  /path/to/auto.file
/automount /etc/auto.nfs

# /etc/auto.ext4
test -fstype=ext4 :dev/nvme1n1p1
dev -fstype=xfs,ro :/dev/nvme1n1p2
nfs -fstype=xfs,ro hostname:/paht/volume/nfs

systemctl restart autofs
ls /autmount/test
ls /automount/dev
df -h
```


# Mount isos

```
mount -o loop /path/to/iso /mnt/iso
mkisofs -o /root/data.iso /root/data
```
