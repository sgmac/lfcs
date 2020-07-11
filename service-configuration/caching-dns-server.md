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
