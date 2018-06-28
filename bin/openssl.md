# openssl


### Generate private key

```
openssl genrsa -out ./server.key 4096
```

### Create a CSR (certificate signing request)

```
openssl req -new -sha256 -key ./server.key -out ./server.csr
```

### Check your CSR

```
openssl req -noout -text -in ./server.csr
```

### CSR Fields

```
DN field                Explanation                                                 Example
----------------------------------------------------------------------------------------------------
Common Name             The fully qualified domain name for your web server.        example.com
                        This must be an exact match                                 *.example.com
ds

Organization Name       The exact legal name of your organization. Do not
                        abbreviate your organization name.                          example.com

Organizational Unit     Section of the organization.                                IT

City or Locality        The city where your organization is legally located.        Wellesley Hills

State or Province       The state or province where your organization is legally    Massachusetts
                        located. Do not use an abbreviation.

Country                 The two-letter ISO abbreviation for your country.           US

```

## Create a CA (certificate authority)

```
openssl genrsa -des3 -out ./ca.key 4096
openssl req -new -x509 -days 365 -key ./ca.key -out ./ca.pem
```

## Sign a CSR with CA

```
openssl x509 -req \
        -days 365 \
        -in ./server.csr \
        -CA ./ca.pem \
        -CAkey ./ca.key \
        -set_serial 01 \
        -out ./server.pem
```