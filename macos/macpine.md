# macpine 

Install docker within an Alpine VM

See https://github.com/beringresearch/macpine

## Copy ssh keys

```
ssh-keygen -t ed25519 -C "alpine-local"
ssh-copy-id -p 2224 root@localhost 
```

## Update DOCKER_HOST & add it to your bashrc or zshrc

```
export DOCKER_HOST="ssh://root@localhost:2224"
```