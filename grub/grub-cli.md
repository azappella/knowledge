# grub command line

for paging long command outputs
``` 
grub> set pager=1
```

list partitions
``` 
ls 
(hd0) (hd0,gpt5) (hd0,gpt4) ...
```

explore filesystem
```
ls (hd0,gpt5)
```

read files on the filesystem
```
cat (hd0,gpt5)/etc/issue
```

booting from grub
```
grub> set root=(hd0,1)
grub> linux /boot/vmlinuz-3.13.0-29-generic root=/dev/sda1
grub> initrd /boot/initrd.img-3.13.0-29-generic
grub> boot
```