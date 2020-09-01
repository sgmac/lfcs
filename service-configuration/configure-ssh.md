# Configure SSH servers/clients

1. Configuration for the server
```
/etc/ssh/sshd_config

AllowUsers sgm
DenyUsers root
PermitRootLogin no
PasswordAuthentication yes
# watch the DNS name of the client
UseDNS yes

```

2. SSH with public key `ssh-copy-id`

3. Check `~/.ssh/authorized_keys`


- Optional use `ssh-agent` and `ssh-add` to add the key.
- Use scp to copy files `scp -P 2222 somefile remoteserver:` (alternative rsync)


## Port forwarding

`ssh -L8080:localhost:8080  user@system`
