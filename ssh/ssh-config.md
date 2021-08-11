# ssh config

## Simple example

```
Host home
Hostname home.example.com
IdentityFile ~/.ssh/id_rsa.home
User <your home acct>

Host work
Hostname work.example.com
IdentityFile ~/.ssh/id_rsa.work
User <your work acct>
```

## Another config file example

```
Host targaryen
    HostName 192.168.1.10
    User daenerys
    Port 7654
    IdentityFile ~/.ssh/targaryen.key

Host tyrell
    HostName 192.168.10.20

Host martell
    HostName 192.168.10.50

Host *ell
    user oberyn

Host * !martell
    LogLevel INFO

Host *
    User root
    Compression yes

```

## Multiple github keys 

```
#user1 account
Host github.com-user1
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_user1

#user2 account
Host github.com-user2
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_user2
```

```
git clone git@github-org:orgname/some_repository.git

```

## Multiple github users

```
git clone git@github.com:user1/example.git example1
cd example1
git config user.name "user1"
git config user.email "user1@example.com" 
```

```
git clone git@github.com:user2/example.git example2
cd example2
git config user.name "user2"
git config user.email "user2@example.com" 
```