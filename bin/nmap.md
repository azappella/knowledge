# nmap

## quickly scan for open ports in local network

```
sudo nmap -sP -PS -n 192.168.0.1/24
```

## Get a list of valid IPs in a subnet

```
nmap -sP 192.168.1.*
```

or 

```
nmap -sn 192.168.1.0/24
```

Per the man page "In newer releases of nmap, -sP is known as -sn".

## more information

```
sudo nmap -sS -sV -O -n 192.168.0.1/24

-sS Send a TCP SYN and waits for a syn/ack to determine if port is open
-sV Determine service version of the service running on a port
-O Determine the operating system of the host
-n Never do dns resolution(unless you have a good reason)
--reason Helps determine why Nmap says a port is closed or filtered
```