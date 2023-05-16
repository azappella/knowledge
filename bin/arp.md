# arp

address resolution display and control

```
arp -na
```

## Find IP from MAC address

```
arp -na | grep -w -i '80:1f' | awk '{print $2}'
```

## Find MAC address from IP address

```
arp -na | grep -w -i '192.168.1.9' | awk '{print $4}'
```