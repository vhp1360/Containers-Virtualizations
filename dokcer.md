<div dir="rtl">بنام خدا</div>

###### top

- [Make Container from LiveOS](#make-container-from-liveos)
- [Send Command to Container](#send-command-to-container)
- [Some Regulare Code](#some-regulare-code)
- [Linking](#linking)
- [Rmove,Delete](#rmovedelete)
- [Run,Attach](#runattach)
- [Ps](#ps)
- [Commit,Export,Import,Save](#commitexportimportsave)
- [Resolving Some Errors](#resolving-some-errors)



[top](#top)
### Make Container from LiveOS

<div dir="rtl">چگونه از یک لینوکس ردهت درحال اجرا یک داکر بسازیم نسخه ۱.۱۰.۳</div>
- how to make docker image of running linux redhat OS:
```go
   tar --numeric-owner --exclude=/proc --exclude=/sys --exclude=/mnt --exclude=/var/cache \
       --exclude=/usr/share/doc --exclude=/tmp --exclude=/var/log -zcvf /mnt/RHEL7.2-base.tar.gz /
   cat /mnt/RHEL7.2-base.tar.gz | docker import - RHEL7.2/7.2
   docker run -i -t RHEL7.2/7.2 /bin/bash
```

[top](#top)       
### Send Command to Container
- run ssh
```go
   yum install openssh-server openssh openssh-client -y --nogpgcheck
   /usr/sbin/sshd-keygen
   /usr/sbin/sshd
```
- send command to running container
```go
   docker exec `docker ps ID` Command
   docker exec -it `docker ps ID` /bin/bash <- connet to running container
```
- connect to running container with the same Output Terminal
```go
  docker run --name ContainerName
  docker attach `docker ps ID Or ContainerName`
```
- how to copy a file to running container
```go
   docker cp Files `docker images name`:Path/FileNam
``` 
[top](#top)       
### Some Regulare Code
- some commands
```go
   service docker start
   docker images
   docker ps
   docker ps -a --> it gaves some report of containers may you run and exit them but they still busy.
   docker stop ContainerID
   docker kill ContainerID
   docker rm ContainerID
   docker cp /Files to Copy 149805c9921d:/root/
   docker commit 149805c9921d docker-couchbase/01
   docker images
   docker run -h HostName -i -t docker-Image /bin/bash
   docker commit 4619e3893ed5 DockerName/No
   docker run -p HostPort:ContainerPort -it ...
   docker export ContainerID -o ExportContainerName
   docker tag ContainerID ContainerName <- after Import container, New Container has None Name
   docker run -v /path/to/host:/path/to/container ...
   
   umount /var/lib/docker/devicemapper/mnt/656cfd09aee399c8ae8c8d3e7...
```

[top](#top)       
### Linking
- Link a container and some issues:
   - Howto Named a container : `docker run -d --name NewName ContainerName`.
   - Howto Linked : `docker run -d --link "AboveName" ...`.
- Install some packages into container:
  - pip3:
    ```go
      wget https://raw.githubusercontent.com/pypa/pip/701a80f451a62aadf4eeb21f371b45424821582b/contrib/get-pip.py -O /root/get-pip.py
      python3.4 /root/get-pip.py
    ```
[top](#top)       
### Rmove,Delete
- remove images:
```go
  docker rmi -f ContainerIDs
```
   - may you faced issue like _docker: Error response from daemon: devmapper: Thin Pool has 142989 free data blocks \
      which is less than minimum required 163840 free data blocks. Create more free space in thin pool or use \
      dm.min_free_space option to change behavior._ then you should try:
   ```go
     docker rm $(docker ps -q -f status=exited)
     docker volume rm $(docker volume ls -qf dangling=true)
     docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
   ```
- Purne:
```go
  docker system prune
```
- Autoremove container when you exit it:
```go
  docker run -it --rm 
```


[top](#top)       
### Run,Attach
- keep alive container every time
```go
  docker run --restart=always ...
  docker run --restart=on-failure:10 ... <- restart maximum 10 times on failure state
```
- Use Special Network:
```go
  docker run --net=host ...  <- Host
  docker run --net=container:<name|id> ... <- Use Container Network
```

[top](#top)
### Resolving Some Errors:

- [Docker “ERROR: could not find an available, non-overlapping IPv4 address pool among the defaults \
   to assign to the network”](https://stackoverflow.com/questions/43720339/docker-error-could-not-find-an-available-non-overlapping-ipv4-address-pool-am)
```vim
  docker network ls
  docker network rm $(docker network ls | grep -v "bridge" | awk '/ / { print $1 }')
```
- [Unable to create docker0 if VPN is active · Issue #779 · docker/libnetwork](https://github.com/docker/libnetwork/issues/779#issuecomment-231727303)
```vim
   openvpn --config vpn_config_file --route-up fix-routes.sh
 ```
   - or script like this:
   ```vim
      #!/bin/sh
      echo "Adding default route to $route_vpn_gateway with /0 mask..."
      ip route add default via $route_vpn_gateway
      echo "Removing /1 routes..."
      ip route del 0.0.0.0/1 via $route_vpn_gateway
      ip route del 128.0.0.0/1 via $route_vpn_gateway
   ```







