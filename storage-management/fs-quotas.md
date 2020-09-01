# Quotas FS


*****IMPORTANT** Check always the  man (i.e: man edquota)
- Add option `usrquota` to the fstab
- Re-mount filesystem `mount -o remount`
- Be sure not users are uploading files to the mountpath `quotacheck -cugm /path/to/mount`
- User `edquota  USER`
- Get a report of the user `quota user`
- Another command `repquota -a`
- For period time `edquota -t` 

## XFS
- Add `uquota` to `/etc/fstab`
- Umount and mount
- Enable quota

```
xfs_quota -x -c 'limit bsoft=10m bhard=20m linda' /path/mountpoint
# check report
xfs_quota -x  -c report /path/mountpoint
```

- This is bad approach, but you ensure that settings these permissions `chmod 777 /path/mountpoint` works



## EXT4 quotas

- Install the `quota` package if missing.
- It's required to created an index (aquota.user) before using it. 

```


## This creates the  aquota.user file.
$ sudo  quoatacheck -cum /path/to/filesystem

## Enable quotas
$ sudo quotaon -v /mnt/ext4/
quotaon: cannot find /mnt/ext4/aquota.group on /dev/sdb1 [/mnt/ext4]

$ quotacheck -cum /mnt/ext4/
/dev/sdb1 [/mnt/ext4]: user quotas turned on

## Disable quotas
$ quotaoff -v /mnt/ext4/
/dev/sdb1 [/mnt/ext4]: group quotas turned off
/dev/sdb1 [/mnt/ext4]: user quotas turned off
```

### Editing quotas

- You specify the size and the number of inodes, this would mean 2 different files, or N hardlinks.

```sh
$ sudo edquota -u sgm
Disk quotas for user sgm (uid 1000):
  Filesystem                   blocks       soft       hard     inodes     soft     hard
  /dev/sdb1                      4800       2048       4800          1     1        2


sudo repquota -a
*** Report for user quotas on device /dev/sdb1
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0       
sgm       +-    4800    2048    4800  7days       1     1     2  7days


$ touch /mnt/ext4/file2
$  sudo repquota -a
*** Report for user quotas on device /dev/sdb1
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0       
sgm       ++    4800    2048    4800  6days       2     1     2  6days

$ touch file3
touch: cannot touch 'file3': Disk quota exceeded

## Determine blocksize
$ tune2fs -l /dev/sdb1  | grep -i "block size"
Block size:               4096

### IMPORTANT 

- Edquota block size is multiple of 1024 bytes (kibibyte)
-  Symbols  K,  M,  G,  and T can be appended to numeric value to express kibibytes, mebibytes, gibibytes, and tebibytes.


sudo repquota -a
*** Report for user quotas on device /dev/sdb1
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0       
sgm       +- 2097156       1 10485760  7days       1     1     2  7days
```

- In order a quota report, we have :

```
 sudo quota sgm
Disk quotas for user sgm (uid 1000): 
     Filesystem  blocks   quota   limit   grace   files   quota   limit   grace
      /dev/sdb1 2097156*      1 15728640   6days       1       1       2
```
