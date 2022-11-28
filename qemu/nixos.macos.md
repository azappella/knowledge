# nixos on mac os

## Download NixOS

```
wget https://channels.nixos.org/nixos-22.05/latest-nixos-minimal-x86_64-linux.iso
```

## Create an image placeholder
```
qemu-img create -f qcow2 nixos-test.img 20G
```

## Launch with qemu 

qemu-system-x86_64 -accel hvf -boot d \    
-cdrom latest-nixos-minimal-x86_64-linux.iso \
-m 2G -cpu host -smp 2 -hda nixos-test.img