# SystemD mounts

- It seems to be automated through `/etc/fstab` but it is the **systemd-fstab-generator** that processes the fstab and converts mounts found there to systemd managed mounts. 
- You can manually create systemd mounts through mount units. Examples: `ls /usr/lib/systemd/system/*.mount `
- `systemctl list-units --type mount`
