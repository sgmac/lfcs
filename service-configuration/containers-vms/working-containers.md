# Containers
- Share OS's kernel

## Docker
- `docker inspect container_name`
- `nmap -sn 172.17.0.0/24`
- `curl http://172.17.0.2`
- Info about containers running `docker ps`
- `docker stop <CONTAINER_ID>`
- `docker run -it --rm -v /data:/storage/data  container` 

## LXC


- Install `apt install lxc lxc-templates`
- This commands create a container but download and provides options `lxc-create -t download -n lfcs-container`.
- Info about package `dpkg -L lxc-templates`
- Create `lxc-create -t TEMPLATE_NAME -n CONTAINER_NAME`
- List containers `lxc-ls -f `
- Start `lxc-start -n ubuntu` (-F foreground, -d background)
- Stop `lxc-stop -n ubuntu`
- Info `lxc-info -n ubuntu`
- Attach `lxc-attach -n ubuntu`
