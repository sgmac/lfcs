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
  1; 			serial
  21600;                refresh
  3600;                 retry
  604800;               expire 
  86400;                negative cache ttl
  )
;
@  IN NS ns.mydomain.local.

ns      IN A 192.168.122.215
webserver IN A 192.168.122.215
www       IN CNAME  webserver
EOF
```

The `mail.<local_zone>` needs to be provided

```
/etc/bind/named.conf
/
zone "lab.internal" {
	type master;
	file "/etc/bind/lab.internal";
};


 /etc/bind/lab.internal
$TTL	604800
@	IN	SOA	ns.lab.internal. mx.lab.internal (
			   1232		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 	; Negative Cache TTL
			)
;
@	IN	NS	ns.lab.internal.

ns	IN	A	192.168.122.228
lfclient IN	A 	192.168.122.103
lfcs	IN	CNAME	ns
www	IN	CNAME   lfclient
```

It's posible to verify if there is some error:

```
named-checkzone lab.internal  /etc/bind/lab.internal
zone lab.internal/IN: loaded serial 1232
OK
```
