# Manage and Configure containers


`docker run -v $PWD/html/:/var/ww/html --name http -p 80:80 -dit nginx:latest`

- Inside LXD you may need to increase

`/proc/sys/kernel/keys/maxkeys`

- `docker start|stop|rm`
- `docker image ls|rmi`
