# Maintain DNS zone

- Install `bind9`

1. Define a new zone in `/etc/bind/named.conf`

```
zone "la.local" {
   type master;
   file "/etc/bind/domain.local";
};
```

2. Create the DB

Serial number needs to be increased 

```
cat<<EOF>/etc/bind/domain.local
$TTL 600
@ IN SOA ns.mydomain.local. mail.mydomain.local. (
  1;
  21600;
  3600;
  604800;
  86400;
  )
;
@  IN NS ns.mydomain.local.

ns      IN A 192.168.122.215
webserver IN A 192.168.122.215
www       IN CNAME  webserver
EOF
```
