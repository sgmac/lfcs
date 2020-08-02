# Filesystems

- Create FS `mkfs -t ext4 /dev/sda1`
- BTRFS `mkfs -t btrfs /dev/sda2`

# XFS

xfs_info  /mnt/mountpoint
xfs_repair -n /dev/nvme1p1 (unmounted)


- Apt install xfsdump
xfs_dump 
xfs_restore

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
