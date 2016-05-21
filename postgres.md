# postgres

## docker
https://hub.docker.com/_/postgres/
```
docker run --name postgres -e POSTGRES_PASSWORD=postgres -d postgres
```
## Create user and database using SQL
1. Connect as postgres:postgres
2. Create the user
```
CREATE USER registry PASSWORD 'registry' createdb;
```
3. Create database
```
CREATE DATABASE registry OWNER = registry;
```

## Create user and database using command
1.  Create the user:

`createuser -h <host> -U <user_to_connect_as> <new_user>`

Example:

`createuser -h localhost -U postgres registry_qa`

2. Create the database:

`createdb -h <host> -O <new_database_owner> -U <user_to_connect_as> <new_database>`

Example:

`createdb -h localhost -O registry -U registry registry_qa`


## Backup
`pg_dump -U <user_to_connect_as> -W -F t <database_to_dump> > <dump.tar>`

Example:

`pg_dump -U registry -W -F t registry > registry.tar`

## Restore
`pg_restore -h <host> -U <user_to_connect_as> <database_to_restore_to> -C -W <dump.tar>`

Example:

`pg_restore -h localhost -U registry -d registry_qa -C -W registry.tar`


## Find database version
```
select version();
```
