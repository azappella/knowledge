# ssh 

## Local port forwarding

Local port forwarding allows you to forward traffic on the SSH client to some destination through an SSH server.
This lets you access remote services over an encrypted connection as if they were local services.
Example use cases:

- Accessing a remote service (database, redis, memcached, etc.) listening on internal IPs
- Locally accessing resources available on a private network
- Transparently proxying a request to a remote service

## Remote port forwarding

Edit /etc/ssh/sshd_config and add the following

```
GatewayPorts yes
```

And then restart the ssh daemon

```
sudo systemctl restart sshd

```

By default, OpenSSH only allows connecting to remote forwarded ports from the server host. However, the GatewayPorts option in the server configuration file sshd_config can be used to control this. The following alternatives are possible:

    GatewayPorts no

This prevents connecting to forwarded ports from outside the server computer.

    GatewayPorts yes

This allows anyone to connect to the forwarded ports. If the server is on the public Internet, anyone on the Internet can connect to the port.

    GatewayPorts clientspecified

This means that the client can specify an IP address from which connections to the port are allowed. The syntax for this is:

    ssh -R 52.194.1.73:8080:localhost:80 host147.aws.example.com

In this example, only connections from the IP address 52.194.1.73 to port 8080 are allowed.


## Dynamic port forwarding

```
ssh -N -D 9000 user@server_b
```

Then, configure your web browser to use SOCKS proxy host 127.0.0.1 and port 9000

## Useful flags

```
-i # specify an ssh identity file / key
-f # forks the ssh process into the background
-n # prevents reading from STDIN
-N # do not run remote commands. Used only when forwarding ports
-T # disables TTY allocation
```

## Example

```
ssh -nNT -L 9000:imgur.com:80 user@example.com
```

## references

- https://www.ssh.com/academy/ssh/tunneling/example