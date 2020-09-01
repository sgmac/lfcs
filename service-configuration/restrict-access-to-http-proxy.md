# HTTP Proxy access restrict

`apt install squid`

- Limit access to users or IP addresses we want.
- `ls -l /etc/squid`
- Edit `/etc/squid/squid.conf`

```
http_access allow localnet !nochad
http_port 3128
acl localnet src 192.168.1.0/24
acl nochad src 10.9.185.155
```

## Configuring ACLS

```
acl_workinghours time MTWHF 07:00-19:00
acl blockedsite url_regex ^http://.*.oracle.com/.*$
acl valid_users proxy_auth REQUIRED

http_access allow workinghours
`

