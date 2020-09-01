# Create RAID devices

- RAID0: striping
- RAID1: mirror
- RAID5: striping with distribute parity.
- RAID6: striping with dual  distribute parity.
- RAID10: mirror + stripe (stripe of mirrors) You need at least 4 devices.

- Install `mdadm`


#### Create a stripe
```
$ sudo mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=2 /dev/xvdf1 /dev/xvdf2 
$ mdadm --detail /dev/md0
$ cat /proc/mdstat
```

#### Create a mirror

```
sudo mdadm  --create /dev/md0 --level=1 --raid-disks=2 /dev/vdd1 /dev/vde1
```

### Create a raid5

- The spare it will rebuilds
```
sudo mdadm --create /dev/md1 --level=5 --raid-disks=3 --spare-devices=1  /dev/sda /dev/sdb /dev/sdc /dev/sdc
sudo mdadm --fail /dev/md1  /dev/sdc
sudo mdadm --remove /dev/md1  /dev/sdc
```

## Add it to fstab

- Then `mkfs -t ext4 /dev/md0`
- Now it can be mounted
- Add the device to `/etc/mdadm/mdadm.conf`
- `mdadm --detail --scan >> /etc/mdadm/mdadm.conf`
- Check `/etc/default/mdadm`

```
AUTOSTART=true
```

- Add a spare device `mdadm /dev/md0 --add /dev/sdc1`

