# Network shares


## NFS

Install `nfs-client`, tools?

- Create directory `chmod -R 755 /share`
- `chown nfsnobody:nfsnobody /share`


- Systemctl check `rpcbind, nfs-server and nfs-idmap` are up.


- Add resources `no_root_squash` user root in src computer will be root in the NFS share server.

```
/etc/exports

/share 172.31.96.178(rw,sync_no_root_squash,no_all_squash)

# verify

exportfs -rav
```

- You can verify is working doing `showmount -e localhost`

### Troubleshooting

```
sudo rpcinfo -p 192.168.122.215
sudo showmount -e 192.168.122.215
cat  /var/lib/nfs/etab 
/mnt/nfs_shared	192.168.1.0/24(rw,sync,wdelay,hide,nocrossmnt,secure,root_squash,no_all_squash,no_subtree_check,secure_locks,acl,no_pnfs,anonuid=65534,anongid=65534,sec=sys,rw,secure,root_squash,no_all_squash)

sudo journalctl | grep rpc.mountd
Aug 26 08:14:39 lfcs rpc.mountd[4693]: Version 1.3.3 starting
Aug 26 08:14:39 lfcs rpc.mountd[4693]: Caught signal 15, un-registering and exiting.
Aug 26 08:14:39 lfcs rpc.mountd[4959]: Version 1.3.3 starting
Aug 26 08:19:14 lfcs rpc.mountd[4959]: Caught signal 15, un-registering and exiting.
Aug 26 08:19:14 lfcs rpc.mountd[5647]: Version 1.3.3 starting
Aug 26 08:19:30 lfcs rpc.mountd[5647]: refused mount request from 192.168.122.215 for /mnt/nfs_shared (/mnt/nfs_shared): unmatched host
Aug 26 08:21:09 lfcs rpc.mountd[5647]: refused mount request from 192.168.122.215 for /mnt/nfs_shared (/mnt/nfs_shared): unmatched host
Aug 26 08:30:31 lfcs rpc.mountd[5647]: Caught signal 15, un-registering and exiting.
Aug 26 08:30:31 lfcs rpc.mountd[6427]: Version 1.3.3 starting
Aug 26 08:31:16 lfcs rpc.mountd[6427]: Caught signal 15, un-registering and exiting.
Aug 26 08:31:16 lfcs rpc.mountd[6510]: Version 1.3.3 starting
Aug 26 08:35:48 lfcs sudo[6659]:      sgm : TTY=pts/0 ; PWD=/mnt/nfs_shared ; USER=root ; COMMAND=/bin/journalctl -xu rpc.mountd
Aug 26 08:35:52 lfcs sudo[6662]:      sgm : TTY=pts/0 ; PWD=/mnt/nfs_shared ; USER=root ; COMMAND=/bin/journalctl -xe rpc.mountd
Aug 26 08:35:55 lfcs sudo[6664]:      sgm : TTY=pts/0 ; PWD=/mnt/nfs_shared ; USER=root ; COMMAND=/bin/journalctl -xu rpc.mountd

```


### NFS client

- Mount the remout resource

```
mount -t nfs 172.31.124.130:/share /mnt/remote
```

## CIFS
- Install `samba` package. Client `cifs-utils smbclient`
- Configure new share
-  Add the user to samba `smbpasswd -a sgm`
- The user you had must be an existing user in the system.
```
[data]
 comment = /data/share
 path = /data
 browseable = yes
 writeable = yes
 valid users = sgm
```

- Test with `sudo smbclient -L 192.168.122.215`
- Finally you can mount with this command

`sudo mount -o username=sgm,password=sgm //192.168.122.215/data /mnt/samba/`

- Add to fstab

```
//192.168.122.215/data /mnt/samba cifs  credentials=/mnt/.credentials,_netdev 	0 0
```


