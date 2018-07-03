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

