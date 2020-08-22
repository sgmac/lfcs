# Configure IMAP and IMAPS

- Install `dovecot-core dovecot-pop3d dovecot-imapd`
- Find out the name of group `/var/mail/user`
- Edit `/etc/dovecot/conf.d/10-mail.conf`

```
mail_priviliged_group = <SAME_GROUP_POSTFIX>
```

- `/etc/dovecot/conf.d/20-imapd.conf`
- `/etc/dovecot/conf.d/20-pop3d.conf`

- Create certificates (how to generate those?)

- Enable ssl and add keys  `/etc/dovecot/conf.d/10-ssl.conf`
- Restart service and check they are running `110,143,993,995`

## Troubleshhoting

- Check `/var/log/mail.log` for errors or `journalctl -xu dovecot`
- In the `/etc/dovecot/dovecot.conf` check `protocols=imap` 
- Check port listening `ss -putan`
- Verify conncetion `mutt -f imap://user@localhost`
