# Network shares

Install `nfs-client`

- Create directory `chmod -R 755 /share`
- `chown nfsnobody:nfsnobody /share`


- Systemctl check `rpcbind, nfs-server and nfs-idmap` are up.


- Add resources

```
/etc/exports

/share 172.31.96.178(rw,sync_no_root_squash,no_all_squash)

# verify

exportfs -a
```

- Mount the remout resource

```
mount -t nfs 172.31.124.130:/share /mnt/remote
```
