# HTTP server logs

```
LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
```

- h : hostname or IP address
- l: remote login name
- u: remote user
- t: date and time the requst
- r: first line of the request to the server
- `%>s:` the final status of that request

```
 ErrorLog ${APACHE_LOG_DIR}/error.log
 CustomLog ${APACHE_LOG_DIR}/access.log sgmcustom
```

