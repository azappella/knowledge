# bind remote docker socket

Bind remote docker.sock with SSH tunnel

```
ssh -nNT -L /tmp/docker.sock:/var/run/docker.sock  <USER>@<IP> &
```

Set environment variable for local Docker client:

```
export DOCKER_HOST=unix:///tmp/docker.sock
```

## References

- https://linuxhandbook.com/docker-remote-access/