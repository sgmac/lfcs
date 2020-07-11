# Hard/Symlinks 

- not hard links across filesystems.
```
ln /mnt/training/Linux_Installation_Instructions.pdf .
ln: failed to create hard link './Linux_Installation_Instructions.pdf' => '/mnt/training/Linux_Installation_Instructions.pdf': Invalid cross-device link
```

- not hard links for directory
```
 ln EXAMPLE  dir1
ln: EXAMPLE: hard link not allowed for directory
```
