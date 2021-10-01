# Initial configuration of a server

### Update hostname

```sh
echo "server-1" > /etc/hostname
hostname -F /etc/hostname
```

### Update system's host file

```sh
vim /etc/hosts
```

The `hosts` file creates static  associations between IP addresses and hostnames or domains which the  system prioritizes before DNS for name resolution. Enter your **Fully Qualified Domain Name** (FQDN) if you have one, and with the local hostname you set in the steps above. In the example below, `203.0.113.10` is the public IP address, `hostname` is the local hostname, and `hostname.example.com` is the FQDN.

```conf
127.0.0.1 localhost
203.0.113.10 server-1.example.com server-1
```

### Configure Timezone

```sh
sudo dpkg-reconfigure tzdata
```

### Create a user

```sh
adduser <username>
```

### Add a user to sudo group

```sh
sudo usermod -aG sudo <username>
```

### Create ssh dir

```sh
mkdir -p ~/.ssh && sudo chmod -R 700 ~/.ssh/
```

### Create authorized_keys file & add public key

```sh
vim ~/.ssh/authorized_keys
```

### update ssh folder permissions

```sh
chmod 644 -f ~/.ssh/*.pub ~/.ssh/authorized_keys ~/.ssh/known_hosts
```


