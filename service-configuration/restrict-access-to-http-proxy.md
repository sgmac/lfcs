# HTTP Proxy access restrict

`apt install squid`

- Limit access to users or IP addresses we want.
- `ls -l /etc/squid`
- Edit `/etc/squid/squid.conf`

```
acl localnet src 192.168.1.0/24
acl nochad src 10.9.185.155
http_access allow localnet !nochad
```


