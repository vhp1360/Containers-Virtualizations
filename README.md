<div dir="rtl">بنام خدا</div>

<div dir="rtl">چگونه از یک لینوکس ردهت درحال اجرا یک داکر بسازیم نسخه ۱.۱۰.۳</div>

how to make docker image of running linux redhat OS:

    tar --numeric-owner --exclude=/proc --exclude=/sys --exclude=/mnt --exclude=/var/cache --exclude=/usr/share/doc --exclude=/tmp --exclude=/var/log -zcvf /mnt/RHEL7.2-base.tar.gz /
    cat /mnt/RHEL7.2-base.tar.gz | docker import - RHEL7.2/7.2
    docker run -i -t RHEL7.2/7.2 /bin//bash

run ssh

    yum install openssh-server -y --nogpgcheck
    /usr/sbin/sshd-keygen
    /usr/sbin/sshd
    

<div dir="rtl"></div>
<div dir="rtl"></div>



