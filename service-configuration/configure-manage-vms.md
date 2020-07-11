# Configure manage VMs

- Install `apt install qemu-kvm libvirtd`
- `virt-install --name=tinyalpine --vpus=1 --memory=1024 --cdrom=image.sio --disk  size=5`
- `virsh list --all`
- `virsh edit tinyalpine`
- `virsh autostart tinyalpine`
- `virsh autostart --disable otherVM`
-  Source needs to be stopped `virt-clone --original=tinyalpine --name=tiny2 --file=/var/lib/libvirt/tinyalpine2.qcow2 `