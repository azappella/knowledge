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
-vga virtio -display cocoa,show-cursor=on,left-command-key=on -usb \
-m 2G -cpu host -smp 2 -hda nixos-test.img 

## useful shortcuts on macos

    /Ctrl-alt-g/ -> free the mouse from inside the image.
    /Ctrl-alt-f/ -> toggle switch fullscreen.


## References

- https://dev.to/64j0/starting-with-nixos-using-qemu-2ngh
- https://nixos.org/manual/nixos/stable/index.html#sec-installation