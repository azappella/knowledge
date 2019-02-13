# ssh

SSH keys serve as a means of identifying yourself to an SSH server using public-key cryptography and challenge-response authentication.

SSH keys are always generated in pairs with one known as the private key and the other as the public key. The private key is known only to you and it should be safely guarded. By contrast, the public key can be shared freely with any SSH server to which you wish to connect.

[Source](https://wiki.archlinux.org/index.php/SSH_keys)

## RSA

ssh-keygen defaults to RSA therefore there is no need to specify it with the -t option.

    ssh-keygen -b 4096

## ed25519

    ssh-keygen -t ed25519


You can also add an optional comment field to the public key with the -C switch, to more easily identify it in places such as ~/.ssh/known_hosts, ~/.ssh/authorized_keys and ssh-add -L output.

    ssh-keygen -b 4096 -C "youremail@example.com"

## Managing multiple keys

```
~/.ssh/config

Host SERVER1
   IdentitiesOnly yes
   IdentityFile ~/.ssh/id_rsa_SERVER1

Host SERVER2
   IdentitiesOnly yes
   IdentityFile ~/.ssh/id_ed25519_SERVER2
```

## Changing a key's passphrase
```
ssh-keygen -f ~/.ssh/id_rsa -p
```

## check key fingerprint
```
ssh-keygen -E md5 -lf <key>
```
If you prefer the standard sha256 output
```
ssh-keygen -lf <key>
```

## generate a public key from a private key
```
ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
```