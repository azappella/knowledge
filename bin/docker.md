# docker

## pull image
```
docker pull image:tag
```
## list containers that have been run
```
docker ps -a
```
## docker run

    docker run -it

    the -i switch keeps STDIN open even if not attached
    the -t switch allocates a pseudo-TTY
    the -p switch publishes a container's ports to the host (this can be a list of ports)
    the -v switch bind mount a volume
        -v /path/on/host:/path/on/guest
    the -e switch passes env variables to the container, e.g. -e "[variable-name]=[new-value]"

    the --rm switch automatically removes the container when it exits

    the --net switch specifies which isolated network you want to run the network from

## docker port

will list all ports from a container

    docker port [container-name]

