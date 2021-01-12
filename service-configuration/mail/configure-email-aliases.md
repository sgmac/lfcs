# Email aliases

- Install `postfix`
- Allow user to send an email to an alias and a group gets the same email.

```
/etc/postfix/aliases

webmaster: user
chad: chad, boss
```

Every email to `chad`, gets to `chad` and to `boss`

``` 
$ sudo postalias /etc/postfix/aliases
```
