# Iptables

- List `iptables -L -n -v`
- Insert `iptables -I 1 -p tcp --dport 22 -j ACCEPT | DROP | REJECT` (drop does not response)
- Append `iptables -I 1 -p tcp --dport 22 -j `
- Delete `iptables --flush`
- Forward `iptables -P FORWARD ACCEPT`
- ICMP reject `iptables -A INPUT -p icmp -j REJECT`
- Delete `iptables -D INPUT 1`
