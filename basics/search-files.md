# Searching for files

- find options `$ find / -not -name "test.txt"`
- find types `$ find / -type c|d|f|l`
- find logs `$ find / -type f -name "*.logs"`
- find size `$ find / -size +100M|k|c`
- find files created +1d ago `$ find / -type f -mtime 1`
- find files created less than 1d ago `$ find / -type f -mtime -1`
- find user `$ find / -type f -user user`
- find permissions `$ find / -perm 755 -user sgm `
- find exec `$ find / -perm 755 -exec chmod 744 {}\;`
- which
- locate `updatedb`
- man -k find
