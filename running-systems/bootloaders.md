# Bootloaders

Main configuration `/boot/grub/grub.cfg` is based in the configuration files found in `/etc/grub.d`. You can configure also `/etc/default/grub`, then you must run `update-grub2`

```sh
#!/bin/sh
# /etc/grub.d/50_other_linux
echo "Displayed when update-grub2 is run"
cat << EOF
menuentry "Other linux partition" {
set root=(hd0, 3)
linux /boo/vmlinuz
initrd /boot/initrd.img
}
EOF
```

- Do not  forget to `chmod +x /etc/grub.d/50_other_linux`
- `grub-install  --root-directory=/mnt /dev/sda`
