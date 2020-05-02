# Cassandra

## CQLSH

#### Connect
    % CQLSH_HOST=10.10.0.40 CQLSH_PORT=9042 ./cqlsh
---
#### Show keyspaces

    SELECT * FROM system.schema_keyspaces;
or 

    desc keyspaces;
---

#### Create keyspace
    CREATE  KEYSPACE test_ks 
        WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
---

#### Select keyspace
    use awesome;
---

#### Create table

CREATE TABLE test_tbl (
    id int,
    english text,
    symbol text, 
    PRIMARY KEY (id)
);

#### Insert data



#### Show tables in keyspace
    SELECT columnfamily_name FROM system.schema_columnfamilies WHERE keyspace_name = 'awesome';
---
#### Show columns for keyspace and table
    select * from system.schema_columns where keyspace_name='awesome' and columnfamily_name = 'taxonomy';
---
#### Select
    select device_id, manufacturer, mapped_manufacturer from taxonomy where device_id = 'fe91ed6f-d038-4e89-b47b-6bad3cc40477' limit 10;
---
#### Run query from file
    % CQLSH_HOST=10.10.0.40 CQLSH_PORT=9042 ./cqlsh -f <file>
---
#### Run query from command line
    % CQLSH_HOST=10.10.0.40 CQLSH_PORT=9042 ./cqlsh -e '<query>'


## Sample script

create keyspace test_ks with replication = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
use test_ks;
create table test_tbl (
    id int,
    english text,
    symbol text,
    primary key (id)
);
insert into test_ks.test_tbl (id, english, symbol) values (1, 'one', 'xxx');
insert into test_ks.test_tbl (id, english, symbol) values (2, 'two', 'yyy');
insert into test_ks.test_tbl (id, english, symbol) values (3, 'three', 'zzz');


## Resources

- [CQLSH reference](https://docs.datastax.com/en/cql-oss/3.3/cql/cql_reference/cqlReferenceTOC.html)
