# docker context 

## create context

Example 1
```
docker context create docker-alpine \
  --default-stack-orchestrator=swarm \
  --docker host="ssh://root@localhost:2224"
```

Example 2
```
docker context create docker-alpine \
  --default-stack-orchestrator=swarm \ 
  --docker host="ssh://root@localhost:2224"
```

## use context

Use example 2
```
docker context use docker-alpine
```