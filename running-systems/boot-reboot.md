# Boot,reboot

- Correct way `shutdown -r now || shutdown -r +15`

## Operating modes

System V, you can find out `runlevel`

* 0: halt
* 1: single-user
* 2: multi-user without network
* 3: multi-user with network
* 5: multi-user with network/graphic model
* 6: reboot

- Enter grub, go do the end fo the `linux /boot/vmlinuz...` and add the runlevel

i``
 linux   /boot/vmlinuz-5.4.0-59-generic root=UUID=d1ef36ef-2258-4435-810a-aaa72da61785 ro  acpi_enforce_resou    rces=lax amd_iommu=on 3
```




