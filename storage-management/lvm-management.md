# LVM opertions

- `apt install lvm2`
- Create a partition and mark it as LVM Linux (`8e`)
- `pvcreate /dev/sdb1 /dev/sdb2`
- `vgcreate /dev/sdb1 /dev/sdb2 my_group`
- `lvcreate --name my_volume --size my_group`
- `lvdisplay`
- `mkfs -t ext4 /dev/my_group/my_volume`
- You can list `vgs,lvs,pvs`
- `vgextend my_group /dev/sdc3`
- Do a `pvscan` to verify is have been added.
- `lvextend -L +10G /dev/my_group/my_volume `
- `e2fsck /dev/my_group/my_volume`
- `resize2fs /dev/my_group/my_volume`
