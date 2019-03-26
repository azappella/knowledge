# gpg

## create entropy for key generation

dd if=/dev/sda of=/dev/zero

## decrypt

```
gpg -o somefile.zip -d somefile.zip.gpg
```

## encrypt

```
gpg --recipient recipient@email --output your-file.zip.gpg -encrypt your-file.zip
```

## useful links

- https://loganmarchione.com/2015/12/brief-introduction-gpg/
- https://hashrocket.com/blog/posts/encryption-with-gpg-a-story-really-a-tutorial