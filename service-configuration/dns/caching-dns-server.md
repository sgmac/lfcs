# DSN cache

`apt install bind`

Edit `/etc/named.conf`


```
options {
  //...
  allow-query { localhostl;any;  };
  allow-query-cache {localhost; any;};
}
```

- Permissions `root:group`
- `ls -lZ /etc/named.conf | grep conf_t`
- In case the SELinux permissions are required `semanage fcontext -a -t named conf_t /etc/named.conf`
- Same the above for `/etc/named.rfc912.zones`
- Verify the configuration `name-checkconf /etc/named.conf`
- Restart service `systemctl start bind.service`, verify is enabled.
- Check firewall on port 53

## Unbound


```
/etc/unbound.conf
interface: 0.0.0.0

# users from the local network can access the server
access-control: 192.168.122.0/24
forward-zone:
  name: "."
  forward-addr: 8.8.8.8
domain-insecure: mydomain.local
```
