# nginx

check if nginx configuration is ok

```
nginx -t
```

graceful reload nginx processes

```
nginx -s reload
```

dump full nginx configuration

```
nginx -T
```

after config change, test and reload

```
nginx -t &&  nginx -s reload
```

