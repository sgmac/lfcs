# Networking routing

- `ip route list`
- `traceroute | mtr`
- `sysctl -p net.ipv4.ip_forward=1`
- Only works if you have an interface in 192.168.1.69 `ip route add 10.40.37.0/24 via 192.168.1.69` 


- Multiple routes to the same place with different weight
```
ip route del 10.40.37.0/24 proto static metric 10 via inet 192.168.1.69 dev eth0
```

- Verify with traceroute


