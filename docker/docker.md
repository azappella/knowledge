# docker

list all containers

```
docker ps -a
```

docker run

```
docker run -it --name CONTAINER -d IMAGE
```

docker exec

```
docker exec -i CONTAINER COMMAND
```

## docker exec

Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Run a command in a running container

Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container

## docker run

the -i switch keeps STDIN open even if not attached
the -t switch allocates a pseudo-TTY
the -p switch publishes a container's ports to the host (this can be a list of ports)
the -v switch bind mount a volume
    -v /path/on/host:/path/on/guest
the -e switch passes env variables to the container

the --rm switch automatically removes the container when it exits

the --net switch specifies which isolated network you want to run the network from

## docker port

docker port [container-name] will list all ports from a container