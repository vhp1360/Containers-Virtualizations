<div dir="rtl">بنام خدا</div>

<div dir="rtl">چگونه از یک لینوکس ردهت درحال اجرا یک داکر بسازیم نسخه ۱.۱۰.۳</div>

- how to make docker image of running linux redhat OS:
```go
   tar --numeric-owner --exclude=/proc --exclude=/sys --exclude=/mnt --exclude=/var/cache \
       --exclude=/usr/share/doc --exclude=/tmp --exclude=/var/log -zcvf /mnt/RHEL7.2-base.tar.gz /
   cat /mnt/RHEL7.2-base.tar.gz | docker import - RHEL7.2/7.2
   docker run -i -t RHEL7.2/7.2 /bin/bash
```
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
- Link a container and some issues:
   - Howto Named a container : `docker run -d --name NewName ContainerName`.
   - Howto Linked : `docker run -d --link "AboveName" ...`.
- Install some packages into container:
  - pip3:
    ```go
      wget https://raw.githubusercontent.com/pypa/pip/701a80f451a62aadf4eeb21f371b45424821582b/contrib/get-pip.py -O /root/get-pip.py
      python3.4 /root/get-pip.py
    ```
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
<div dir="rtl"></div>
<div dir="rtl"></div>



