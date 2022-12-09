# postgres grant

## listing attributes

with meta command

```
\du
```

with query

```
SELECT r.rolname, r.rolsuper, r.rolinherit,
  r.rolcreaterole, r.rolcreatedb, r.rolcanlogin,
  r.rolconnlimit, r.rolvaliduntil,
  ARRAY(SELECT b.rolname
        FROM pg_catalog.pg_auth_members m
        JOIN pg_catalog.pg_roles b ON (m.roleid = b.oid)
        WHERE m.member = r.oid) as memberof
, r.rolreplication
, r.rolbypassrls
FROM pg_catalog.pg_roles r
WHERE r.rolname !~ '^pg_'
ORDER BY 1;

```

## check superuser

```
SELECT rolname FROM pg_roles WHERE rolsuper;
```

## listing attributes for :USER

```
\du :USER
```

## list schema 

\dn

## list schema privileges

```
\dn+
```

