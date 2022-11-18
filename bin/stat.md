# stat

## determine file's owner & permissions

In this example, the sf_Shared directory is owned by the root user and the vboxsf group. It is also possible to determine a file's owners and permissions using the stat command:

Owning user:
```
$ stat -c %U /media/sf_Shared/

root
```

Owning group:
```
$ stat -c %G /media/sf_Shared/

vboxsf
```

Access rights:
```
$ stat -c %A /media/sf_Shared/

drwxrwx---
```