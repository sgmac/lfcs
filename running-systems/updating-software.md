# Updating software

- Install `sudo dpkg -i package.deb`
- List packages `dpkg -l `
- List files in a pacakge `dpkg -L coreutils`
- Info about a package `dpkg -s coreutilsb`
- Search `dpkg -S /bin/ls`
          coreutils: /bin/ls
- Update/upgrade `apt update && apt upgrade -y`
- Cache `apt-cache update && apt-cache search kvm`
- Changelog `apt-get changelog nmap`
- Buildep ` apt-get build-dep nmap`
- Clean `apt auto-clean`
