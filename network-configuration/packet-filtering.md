# Iptables

- List `iptables -L -n -v`
- Insert `iptables -I 1 -p tcp --dport 22 -j ACCEPT | DROP | REJECT` (drop does not response)
- Append `iptables -I 1 -p tcp --dport 22 -j `
- Delete `iptables --flush`
- Delete a specific chain `iptables -F INPUT`
- Forward `iptables -P FORWARD ACCEPT`
- ICMP reject `iptables -A INPUT -p icmp -j REJECT`
- Delete `iptables -D INPUT 1`
- Logs `sudo iptables -A INPUT -m state --state NEW -p tcp --dport 80 -j LOG --log-prefix "NEW_HTTP_CONN: "`

## Masquerade

```
net.ipv4.ip_forward=1
net.ipv6.conf.default.forwarding=1
sysctl -p

/etc/rc.local
sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o <INTERFACE> -j MASQUERADE

```

# UFW

It's a wrapper for iptables

```
$ sudo ufw enable 
$ sudo ufw allow ssh/tcp
$ sudo ufw logging on
$ sudo ufw insert 1 allow 80
$ sudo ufw delete deny 22
$ sudo ufw allow proto tcp from 192.168.0.2 to any port 22
$ sudo ufw delete allow proto tcp from 192.168.122.103  to any port 22
$ sudo ufw status numbered
$ sudo ufw app list
$ sudo ufw allow Samba
$ sudo ufw allow from 192.168.0.0/24 to any app Samba
$ sudo ufw app info Samba

```
