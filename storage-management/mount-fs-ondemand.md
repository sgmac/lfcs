# Mount FS: SAMBA/CIFS

- Install `apt install cifs samba-client`
- `smbclient -I <SERVER_IP> -U user -L share`
- Credentials

- `touch /mnt/.smbcredentials`

```
user=user
password=password
```

- Change permissions `chmod 600`
- Add `/etc/fstab`
- `//172.16.20.20/my_share /mnt/samba cifs   credentials=/mnt/.smbcredentials,defaults 0 0`
- `sudo mount -a`

- It's updated whenever a filesystem is mounted or unmounted `/etc/mtab`
