# alpine-node

## best practices for non-root users on alpine linux

```
RUN addgroup -g 1000 -S username && \
    adduser -u 1000 -S username -G username
```
[Source](https://github.com/mhart/alpine-node/issues/48)

## alpine linux has no usermod, but you can install it

```
RUN echo http://dl-2.alpinelinux.org/alpine/edge/community/ >> /etc/apk/repositories
RUN apk --no-cache add shadow && usermod -u 1000 www-data
```

[Source](https://github.com/chrootLogin/docker-nextcloud/issues/3)

