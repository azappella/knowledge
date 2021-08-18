# useful commands

## logs

### UniFi Security Gateway: contains USG’s general logging.
```
tail -f /var/log/messages
```

### General logging
```
show log
```

### IPsec VPN Logging

```
show vpn log
```

### FreeRADIUS Logging
```
sudo cat /var/log/freeradius/radius.log
```

### DNSmasq Logging
```
sudo cat /var/log/dnsmasq.log
```

## DHCP

### Show leases

```
show dhcp leases
```

### Clear all leases

```
clear dhcp leases
```

### Clear one lease

```
clear dhcp lease ip 192.168.1.18
```

## DNS

### Show DNS statistics

```
show dns forwarding statistics
```

### Clear DNS cache

```
clear dns forwarding cache
```

## vpn

### show status

```
show vpn ipsec status
```

### restart vpn

```
ipsec restart && sleep 1
```