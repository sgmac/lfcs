# AppArmor

- Configurations in `/etc/apparmor.d/`
- Install `apparmor` and `aaparmor-utitls`.
- Application profiles in /etc/apparmor.d
- You can create `aa-genprof /your/application`. Then you run your application from other terminal. Press `s` in the previous one to scan events.
- You can use `aa-status`. You need to be sure armour tools are installed.

## Troublehsooting AppArmor

- Use `aa-complain`, `aa-logprof` and `aa-notify`.
