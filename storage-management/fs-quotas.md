# Quotas FS

- Add option `usrquota` to the fstab
- Re-mount filesystem `mount -o remount`
- Be sure not users are uploading files to the mountpath `quotacheck -cugm /path/to/mount`
- User `edquota  USER`
- Get a report of the user `quota user`
- Another command `repquota -a`
- For period time `edquota -t` 

