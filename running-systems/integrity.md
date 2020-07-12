# Integrity

- Identify `lsblk`
- Umount `sudo unmount /mnt`
- Fsck `sudo fcsk -y /dev/xvdf1`
- Next boot runs the fsck `touch /forcefsck`
- Process running group `ps -fG sgm`
