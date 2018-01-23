<div dir=rtl>بنام خدا</div>

###### top
- [Mount Vdi](#mount-vdi)


### Mount Vdi
you could mount vdi as Partion:
```vala
  modprobe nbd
  qemu-nbd -c /dev/nbd0 <vdi-file>
  ll /dev/nbd0*
  mount /dev/nbd0p2 /mnt  <-- this may C drive
  ... do your jobs
  umount /mnt
  qemu-nbd -d /dev/nbd0
```

