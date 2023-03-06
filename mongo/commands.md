# mongo

## introduction

See https://www.mongodb.com/docs/manual/introduction/

### Server

```
mongod
```

### Client 

< 6

```
mongo 
```

>= 6

```
mongosh
```

### connection string

```
mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[defaultauthdb][?options]]
```

## Dump a db

Use mongodump, which dumps a binary representation of the database

```
mongodump --uri="mongodb://mongodb0.example.com:27017" [additional options]
```

```
mongodump --host mongo -u root -p admin --authenticationDatabase admin -v -o /tmp/dump
```