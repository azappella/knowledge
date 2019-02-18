# netcat

## quickly copy file from one machine to another

on target machine (listen)
```
nc -l PORT > file
```

on source machine
```
cat file | nc HOST PORT
```

## copy entire folder

on target machine
```
nc -l PORT | tar xf -
```

on source machine
```
tar cf - FOLDER | nc HOST PORT
```