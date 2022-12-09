# postgres basic commands


## show config file

```
sudo -u postgres psql -c 'SHOW config_file'
```

## connect with non-peer user (pass the host instead of the socket)

```
psql -U user -h localhost -d postgres
```

## posgresql config file

/etc/postgresql/14/main/postgresql.conf

## pg_hba file

pg_hba.conf controls the authentication method

/etc/postgresql/14/main/pg_hba.conf


## psql quit

```
\q
```

## vertical output

```
\x on
SELECT * FROM pg_roles WHERE rolename = CURRENT_ROLE;
-- Change back to normal output
\x off

```