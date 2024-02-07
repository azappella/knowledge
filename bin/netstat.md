# netstat

In cmd:
```
netstat -na | find "8080"
```

In bash:
```
netstat -na | grep "8080"
```

In PowerShell:
```
netstat -na | Select-String "8080"
```