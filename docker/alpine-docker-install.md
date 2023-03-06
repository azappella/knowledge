# install docker on alpine

## prerequisites 

- A 64-bit installation
- Version 3.10 or higher of the Linux kernel. The latest version of the kernel available for your platform is recommended.
- iptables version 1.4 or higher
- git version 1.7 or higher
- A ps executable, usually provided by procps or a similar package.
- XZ Utils 4.9 or higher
- A properly mounted cgroupfs hierarchy; a single, all-encompassing cgroup mount point is not sufficient. See Github issues #2683, #3485, #4568).


## installation steps

```sh
setup-alpine
apk update
apk upgrade
vi /etc/apk/repositories
apk update
apk add docker docker-cli-compose

vi cgroupfs-mount
chmod u+x cgroupfs-mount 
./cgroupfs-mount 

apk add iptables
rc-update add iptables 
/etc/init.d/iptables save
/etc/init.d/networking restart

apk add git
apk add xz
reboot

./cgroupfs-mount 
dockerd
rc-update add docker
service docker start

docker version
docker info
```

## cgroupfs-mount script

```
#!/bin/sh
# Copyright 2011 Canonical, Inc
#           2014 Tianon Gravi
# Author: Serge Hallyn <serge.hallyn@canonical.com>
#         Tianon Gravi <tianon@debian.org>
set -e

# for simplicity this script provides no flexibility

# if cgroup is mounted by fstab, don't run
# don't get too smart - bail on any uncommented entry with 'cgroup' in it
if grep -v '^#' /etc/fstab | grep -q cgroup; then
    echo 'cgroups mounted from fstab, not mounting /sys/fs/cgroup'
    exit 0
fi

# kernel provides cgroups?
if [ ! -e /proc/cgroups ]; then
    exit 0
fi

# if we don't even have the directory we need, something else must be wrong
if [ ! -d /sys/fs/cgroup ]; then
    exit 0
fi

# mount /sys/fs/cgroup if not already done
if ! mountpoint -q /sys/fs/cgroup; then
    mount -t tmpfs -o uid=0,gid=0,mode=0755 cgroup /sys/fs/cgroup
fi

cd /sys/fs/cgroup

# get/mount list of enabled cgroup controllers
for sys in $(awk '!/^#/ { if ($4 == 1) print $1 }' /proc/cgroups); do
    mkdir -p $sys
    if ! mountpoint -q $sys; then
        if ! mount -n -t cgroup -o $sys cgroup $sys; then
            rmdir $sys || true
        fi
    fi
done

# example /proc/cgroups:
#  #subsys_name hierarchy   num_cgroups enabled
#  cpuset   2   3   1
#  cpu  3   3   1
#  cpuacct  4   3   1
#  memory   5   3   0
#  devices  6   3   1
#  freezer  7   3   1
#  blkio    8   3   1

# enable cgroups memory hierarchy, like systemd does (and lxc/docker desires)
# https://github.com/systemd/systemd/blob/v245/src/core/cgroup.c#L2983
# https://bugs.debian.org/940713
if [ -e /sys/fs/cgroup/memory/memory.use_hierarchy ]; then
    echo 1 > /sys/fs/cgroup/memory/memory.use_hierarchy
fi

exit 0

```

## References

- https://docs.docker.com/engine/install/binaries/
- https://wiki.alpinelinux.org/wiki/Configure_Networking#Firewalling_with_iptables_and_ip6tables
