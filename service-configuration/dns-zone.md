# Maintain DNS zone

1. Define a new zone in `/etc/bind/named.conf`

```
zone "la.local" {
   type master;
   file "/etc/bind/la.local";
};
```

2. Create the DB

Serial number needs to be increased 
```
cat<<EOF>/etc/bind/la.local
$ORIGIN la.local
$TTL 600

@ IN SOA dns1.la.local.  mail.la.local. (
  1;
  21600;
  3600;
  604800;
  86400;
);

dns1    IN A 10.9.8.10 
dns2    IN A 10.9.8.11

webserver IN A 10.9.8.80
www IN CNAME  webserver

mail IN A      10.9.8.50
    IN MX 10  mail.la.local
    # backup 
    IN MX 20  labuckup.ca.local
    IN NS  dns1.la.local
    IN NS  dns2.la.local
```
