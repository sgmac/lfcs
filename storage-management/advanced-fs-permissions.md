# Advanced file system permisions

- `chown` 
- `chmod` Sticky bit 1000, GID 2000, SID 4000 + your permissions (i.e `chmod 1770 /path/somedir`)
-  `Group 2000` To create all the directories, files of the same group
- `find / -type d -perm -2000`

