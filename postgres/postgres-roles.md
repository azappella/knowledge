# postgres roles

* CREATE ROLE and its counterpart, DROP ROLE, are your go-to commands for implementing and removing roles.
* ALTER ROLE handles changing the attributes of a role.
* Roles are valid within all databases due to definition at the database cluster level.
* Keep in mind, creating a role name beginning with a special character, requires you to 'address' it with double quotes.
* Roles and their privileges are established using attributes.
* To establish roles needing the LOGIN attribute by default, CREATE USER is an optional command at your disposal. Used in lieu of CREATE ROLE role_name LOGIN, they are essentially equal.

## create a user/role

    CREATE ROLE rolename WITH LOGIN ENCRYPTED PASSWORD 'pass';

    createuser -PE rolename

## drop a user/role

    DROP ROLE rolename;

    dropuser -i rolename

## create a superuser

    CREATE ROLE super WITH SUPERUSER CREATEDB CREATEROLE LOGIN ENCRYPTED PASSWORD 'pass';

    createuser -sPE superuser


## Role attributes 

Roles have a number of fixed attributes that can be set:

    LOGIN - can this role be used to login to the database server?
    SUPERUSER - is this role a superuser?
    CREATEDB - can this role create databases?
    CREATEROLE - can this role create new roles?
    REPLICATION - can this role initiate streaming replication?
    PASSWORD - the password for the role, if set.
    BYPASSRLS - can this role bypass Row Level Security checks?
    VALID UNTIL - an optional timestamp after which time the password will no longer be valid.

Roles with the SUPERUSER flag set automatically bypass all permission checks except the right to login.

## Check privileges for a certain table

```
SELECT grantee, privilege_type 
FROM information_schema.role_table_grants 
WHERE table_name='table';
```

## ACLs

Access Control Lists or ACLs are somewhat cryptic strings that are attached to objects such as tables,functions, views, and even columns in Postgres. They actually contain a list of privileges such as select, insert, execute and so on that are granted to each role, as well as an additional optional flag for each privilege that, if present, denotes that the role has the ability to grant this privilege to other roles, and the name of the role that granted the privileges. 

An example of an ACL for a table created by Joe might be as follows.

```
joe=arwdDxt/joe =r/joe sales_team=arw/joe
```

The first section tells us that Joe has all the available privileges on the table (INSERT, SELECT, UPDATE, DELETE, TRUNCATE, REFERENCES and TRIGGER), originally granted by Joe (when he created the table).

The second section tells us that read access has been granted to PUBLIC (a special, pseudorole that means everyone) by Joe, and the third section tells us that the Sales Team has been granted INSERT, SELECT and UPDATE privileges, again, by Joe.

See [Privileges Reference Documentation](https://www.postgresql.org/docs/current/ddl-priv.html)

