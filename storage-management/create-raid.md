# Create RAID devices

- Install `mdadm`

```
$ sudo mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=2 /dev/xvdf1 /dev/xvdf2 
$ mdadm --detail /dev/md0
$ cat /proc/mdstat
```

- Then `mkfs -t ext4 /dev/md0`
- Now it can be mounted
- Add the device to `/etc/mdadm/mdadm.conf`
- `mdadm --detail --scan >> /etc/mdadm/mdadm.conf`
- Check `/etc/default/mdadm`

```
AUTOSTART=true
```

- Add a spare device `mdadm /dev/md0 --add /dev/sdc1`

