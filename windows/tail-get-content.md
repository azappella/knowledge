# tail

Since Powershell does not have a tail command, you need to use the `Get-Content` command

```
Get-Content [path-to-file] -Tail [number-of-lines]
```

To monitor a file, use the `-Wait` switch

```
Get-Content D:\log.txt -Tail 100 -Wait
```

