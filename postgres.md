# postgres

## docker
 https://hub.docker.com/_/postgres/
```
docker run --name postgres -e POSTGRES_PASSWORD=postgres -d postgres
```
## Create user and database
1. Connect as postgres:postgres
2. Create the user
```
CREATE USER registry PASSWORD 'registry' createdb;
```
3. Create database
```
CREATE DATABASE registry OWNER = registry;
```

## Find database version
```
select version();
```
