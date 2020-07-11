# HTTP Restrict access

- `/var/www/html` or the page we want to protect

- Important to notice that the `Allow,Deny` requires the command and right after the Deny. A whitespace would make the restart of the service fail.
```
<Directory /var/www/html/>
   Order Allow,Deny
   Allow from 10.94.39.16
   Allow from 127
   Allow from ::1
</Directory>
```
