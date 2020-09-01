# LVM opertions


- You can resize up/down Ext4 but you can only increase XFS.
- `apt install lvm2`

- Create a partition and mark it as LVM Linux (`8e`)
- `pvcreate /dev/sdb1 /dev/sdb2`
- `vgcreate /dev/sdb1 /dev/sdb2 my_group`
- `lvcreate --name my_volume --size my_group`
-  `lvcreate -n vol2 -L 10G vg_testing`
- `lvdisplay`
- `mkfs -t ext4 /dev/my_group/my_volume`

- Ocupates all the free space in the vgbooks VG. `lvcreate -l 100%FREE -n lvbooks vgbooks`

## Mounting with SystemD

- The name of the unit needs to match the  mountpoint `/mnt/books`. Then unit is named `mnt-books.mount`.

```
mnt-books.mount 
[Unit]
Description=Mounts books LV
Conflicts=umount.target
Before=local.fs.target umount.target

[Mount]
What=/dev/mirror/books
Where=/mnt/books
Type=ext4
Options=defaults


[Install]
WantedBy=local-fs.target
```

## Mirroring

- You can `man lvcreate` for more information.

```
sudo  lvcreate --type raid1 -m1 -L 500m -n mylv  mirror
  Logical volume "mylv" created.
sgm@lfcs:~$ sudo lvdisplay  /dev/mirror/mylv
  --- Logical volume ---
  LV Path                /dev/mirror/mylv
  LV Name                mylv
  VG Name                mirror
  LV UUID                JyTbp9-Gluf-F2o3-txzy-V0bc-h3CL-NEacvA
  LV Write Access        read/write
  LV Creation host, time lfcs, 2020-08-31 08:37:14 +0000
  LV Status              available
  # open                 0
  LV Size                500.00 MiB
  Current LE             125
  Mirrored volumes       2
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:4

sgm@lfcs:~$ sudo lvs -a
  LV              VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mylv            mirror rwi-a-r--- 500.00m                                    100.00          
  [mylv_rimage_0] mirror iwi-aor--- 500.00m                                                    
  [mylv_rimage_1] mirror iwi-aor--- 500.00m                                                    
  [mylv_rmeta_0]  mirror ewi-aor---   4.00m                                                    
  [mylv_rmeta_1]  mirror ewi-aor---   4.00m              
```

## Resizing

- Always use `-r` to resize the filesystem. Reducing `lvreduce -L -100M -r /dev/vgdata/lvext4`

- You can list `vgs,lvs,pvs`
- `vgextend my_group /dev/sdc3`
- Do a `pvscan` to verify is have been added.
- `lvextend -L +10G /dev/my_group/my_volume `
- `e2fsck /dev/my_group/my_volume`
- `resize2fs /dev/my_group/my_volume`


## Snapshots

- A snapshot is a copy of the metadata.
- Are meant to be temporary.

```
$ sudo lvcreate --snapshot --size 300m --name snap_backup  mirror/mylv
  Using default stripesize 64.00 KiB.
  Logical volume "snap_backup" created.
```

