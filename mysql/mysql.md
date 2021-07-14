# mysql

```
create database database_name;

create user 'username'@'localhost' IDENTIFIED BY 'PASSWORD';

GRANT ALL ON database_name.* TO 'username'@'localhost';

flush privileges;
```