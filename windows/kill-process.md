# kill-process

Kill Process by Name

List all Windows processes and find the full name of a process to kill (case insensitive):
```
C:\> tasklist | findstr /I process_name
```

Kill the process by name:
```
C:\> taskkill /IM process_name.exe
```

Kill Process by PID

List all Windows processes and find the PID of a process to kill (case insensitive):
```
C:\> tasklist | findstr /I process_name
```

Kill the process by PID:
```
C:\> taskkill /PID process_id
```

Kill Process by Port

List all Windows processes listening on TCP and UDP ports and find the PID of a process running on a specific port:
```
C:\> netstat -ano | findstr :port
```

Find the name of a process by its PID:
```
C:\> tasklist /FI "pid eq process_id"
```

Kill the process by name or by PID:
```
C:\> taskkill /IM process_name.exe
- or -
C:\> taskkill /PID process_id
```