# systemd

- `systemctl  status | enable |stop| start | cat`
- Services `/etc/systemd/system` user defined.
- System wide `/lib/systemd/system`
- It's event driven, when it's needed, starts.


## dns

You can manage `● systemd-resolved.service`

- `/etc/systemd/resolv.conf`

It's possible to query `systemd-resolved`
`$ systemd-resolve  --statistics | --status | --flush-caches`


```
systemctl default
systemctl reboot
# to view system logs
journalctl -xb
```

- Help `systemctl -t help`
- List all installed units  `systemctl list-unit-files`
- List active units `systemctl list-units`

## modifiying

- always you modify a service `systemctl cat vsftpd.service`
- Show properties of one or more units, jobs, or the manager itself. If no argument is specified, properties of the manager will be shown.  `systemctl show vsftpd.service`

```
systemctl cat  vsftpd.service
# /lib/systemd/system/vsftpd.service
[Unit]
Description=vsftpd FTP server
After=network.target

[Service]
Type=simple
ExecStart=/usr/sbin/vsftpd /etc/vsftpd.conf
ExecReload=/bin/kill -HUP $MAINPID
ExecStartPre=-/bin/mkdir -p /var/run/vsftpd/empty

[Install]
WantedBy=multi-user.target
```


- always you modify a service `systemctl daemon-reload`

## Targets

- A `target` is a group of services, some tables are isolatable, means you can use them as a state your system should be in:

* emercency.target
* rescue.target
* multi-user.target
* graphical.target

- Use `systemctl list-dependencies name.target` to see the contents and dependencies of a systemd target. 

```
sudo systemctl list-dependencies local-fs.target
local-fs.target
● ├─-.mount
● ├─boot-efi.mount
● ├─home-sgm-.steam.mount
● ├─mnt-training.mount
● ├─systemd-fsck-root.service
● ├─systemd-remount-fs.service
● ├─var-lib-docker.mount
● └─var-lib-libvirt-images.mount
```

- Most common operations

- `systemctl start name.target`
- `systemctl isolate name.target`
- `systemctl get-default`
- `systemctl set-default name.target`
-  `systemctl list-units --type target`
-  `systemctl isolate multi-user.target`
-  `systemctl start graphical.target`
-  `systemctl isolate emergency.target`

