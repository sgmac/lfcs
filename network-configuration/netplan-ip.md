# Network configuration


- Netplan

```yaml
enp0s8:				
      dhcp4: no
      dhcp6: no
      addresses: [192.168.56.110/24, ]
      gateway4:  192.168.56.1
      nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
```

- ifupdown
```
auto eth0
iface eth0 inet static
  adress 192.168.1.20
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 1.1.1.1
  dns-search mydomain.com
```

# Boot services

- systemcl enable sshd.service
- `apt install  xinetd  k
