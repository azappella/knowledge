# dd

## create a bootable usb flashdrive

identify the target drive with `sudo lsblk -f`

```
sudo umount /dev/sdb
sudo dd if=/path/to/ubuntu.iso of=/dev/sdb bs=1M
```