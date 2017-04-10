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
```ruby
   yum install openssh-server openssh openssh-client -y --nogpgcheck
   /usr/sbin/sshd-keygen
   /usr/sbin/sshd
```
- send command to running container
```go
   docker exec `docker ps ID` Command
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
   
   
   umount /var/lib/docker/devicemapper/mnt/656cfd09aee399c8ae8c8d3e7...
```
- Link a container and some issues:
   - Howto Named a container : `docker run -d --name NewName ContainerName`.
   - Howto Linked : `docker run -d --link "AboveName" ...`.

<div dir="rtl"></div>
<div dir="rtl"></div>



