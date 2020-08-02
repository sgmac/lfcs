# Mount FS: SAMBA/CIFS

*****IMPORTANT**: You need to add user to samba
**PENDING** samba shared is mount but need to find out how to write. 

```
$ sudo smbpasswd -a sgm 

#remoing a user
$ smbpasswd -x sgm
```

In the client the package `cifs-utils` was missing and this provides `mount.cifs`

Use `testparm` to show the configuration.


## Client
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

## Server

- Install `sudo apt install samba`

```
# /etc/samba/smb.conf
[shared]
  comment = Shared lfcs2.lab
  path = /mnt/shared
  browseable = yes
  read only = no
  guest ok = no
  valid users = sgm root
```

- Then `sudo systemctl restart smbd.service` and go to the client and test

```
smbclient -W LFCS  -I lfcs2 -U sgm -L share
WARNING: The "syslog" option is deprecated
Enter LFCS\sgm's password: 

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	shared          Disk      Shared lfcs2.lab
	IPC$            IPC       IPC Service (lfcs2 server (Samba, Proxmox vLab))
Reconnecting with SMB1 for workgroup listing.

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
	LFCS                 LFCS2
```

If mount does not work check errors with `dmesg`

```
sgm@lfcs1:/mnt$ sudo mount -a
mount: /mnt/samba_lfcs2: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.

$ sudo dmesg
1377.434107] FS-Cache: Netfs 'cifs' registered for caching
[11377.434233] Key type cifs.spnego registered
[11377.434235] Key type cifs.idmap registered
[11377.438483] No dialect specified on mount. Default has changed to a more secure dialect, SMB2.1 or later (e.g. SMB3), from CIFS (SMB1). To use the less secure SMB1 dialect to access old servers which do not support SMB3 (or SMB2.1) specify vers=1.0 on mount.
```



