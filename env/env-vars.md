# env vars

```
IFS=$'\n'

for j in $(cat .env)
do
    echo "$j"
done
```

```
IFS=$'\n' for j in $(cat .env); do eval "export $j"; done
```

```
(export $(cat .env | grep -v ^# | xargs -d '\n') && per-env)
cross-env $(cat .env | grep -v ^# | xargs | echo) per-env
```
