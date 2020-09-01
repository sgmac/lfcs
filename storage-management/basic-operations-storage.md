# Basic operations

- `lsbk`
- `fdisk /dev/sdb1` (a: bootable partition,  g: GPT, w: write)
- `parted`

## MBR vs GPT

`Master Boot Record` store parition information in a 512byte  as well as the bootstrap loader

* 4 primary partitions
* Max of 2TB 



`Guid Partition Table`

* up to 128 partitions


- Kernel can not update the table partiions `cat /proc/partitions`. You can run `partprobe`.

## GPT partitions


- Using gpart
```
parted /dev/sdc
print 
mklable 
New disk label type?
gpt
print
mkpart
Start? 0
End? 500M
print
quit
```
- Using gdisk

```
gdisk /dev/sdb
?
```

## SSD

- `noatime` to avoid updating access time


```

# /lib/systemd/system/fstrim.service       
[Unit]
Description=Discard unused blocks on filesystems from /etc/fstab
Documentation=man:fstrim(8)
ConditionVirtualization=!container

[Service]
Type=oneshot
ExecStart=/sbin/fstrim --fstab --verbose --quiet
ProtectSystem=strict
ProtectHome=yes
PrivateDevices=no
PrivateNetwork=yes
PrivateUsers=no
ProtectKernelTunables=yes
ProtectKernelModules=yes
ProtectControlGroups=yes
MemoryDenyWriteExecute=yes
SystemCallFilter=@default @file-system @basic-io @system-service

: sudo systemctl status  fstrim.service                                                                            [~]
● fstrim.service - Discard unused blocks on filesystems from /etc/fstab
     Loaded: loaded (/lib/systemd/system/fstrim.service; static; vendor preset: enabled)
     Active: inactive (dead) since Mon 2020-08-31 07:35:21 CEST; 10h ago
TriggeredBy: ● fstrim.timer
       Docs: man:fstrim(8)
    Process: 1620 ExecStart=/sbin/fstrim --fstab --verbose --quiet (code=exited, status=0/SUCCESS)
   Main PID: 1620 (code=exited, status=0/SUCCESS)

Aug 31 07:34:46 void systemd[1]: Starting Discard unused blocks on filesystems from /etc/fstab...
Aug 31 07:35:21 void fstrim[1620]: /mnt/training: 224,2 GiB (240747241472 bytes) trimmed on /dev/mapper/crucial_p1-training
Aug 31 07:35:21 void fstrim[1620]: /var/lib/docker: 73,7 GiB (79069937664 bytes) trimmed on /dev/mapper/crucial_p1-docker
Aug 31 07:35:21 void fstrim[1620]: /boot/efi: 478,2 MiB (501374976 bytes) trimmed on /dev/nvme0n1p1
Aug 31 07:35:21 void fstrim[1620]: /: 239,7 GiB (257401503744 bytes) trimmed on /dev/nvme0n1p2
Aug 31 07:35:21 void systemd[1]: fstrim.service: Succeeded.
Aug 31 07:35:21 void systemd[1]: Finished Discard unused blocks on filesystems from /etc/fstab.


sudo systemctl cat fstrim.timer                                                                                  [~]
# /lib/systemd/system/fstrim.timer
[Unit]
Description=Discard unused blocks once a week
Documentation=man:fstrim
ConditionVirtualization=!container

[Timer]
OnCalendar=weekly
AccuracySec=1h
Persistent=true

[Install]
WantedBy=timers.target

```
