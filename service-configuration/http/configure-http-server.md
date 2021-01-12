# Configure HTTP server

- Install `apache2` `apache2-utils` and `apache2-docs`
- Symlink based enable, `conf-availables` and `conf-enabled` same for `mods` and `sites`.
- `/etc/apache2/apache2.conf`

- Use tools `a2dismod,a2dissite,a2enmod,a2ensite`.
- Example `a2ensite vhost.conf`


## Configuring vhosts

- Use `_default_` to catchall requests that don't match a specific virtual host.

- Permit apache2  to serve content from `/web`. Add this to `apache2.conf`
```
<Directory /web>
        AllowOverride None
        Require all granted
</Directory>
```
- Add `/etc/apache2/conf-available/accounts.conf`
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /web/accounts
        ServerName accounts.example.com
        ErrorLog ${APACHE_LOG_DIR}/accounts.log
	CustomLog  ${APACHE_LOG_DIR}/accounts_access.log combined
</VirtualHost>
```
- Enable the site `a2ensite accounts.conf`
- Systemd `systemctl reload apache2`

## Basic auth

```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /web/accounts
        ServerName accounts.example.com
        ErrorLog ${APACHE_LOG_DIR}/accounts_error.log
        CustomLog  ${APACHE_LOG_DIR}/accounts_access.log combined

# This needs to point to the root of the document where
# the content is served.

<Directory /web/accounts>
  AuthType Basic
  AuthName "whatsoever"
  AuthUserFile /etc/apache2/htpasswd
  Require valid-user
</Directory>
</VirtualHost>
```
- Reload the configuration `systemctl reload apache2`
