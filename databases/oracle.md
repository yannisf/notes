# Oracle technics

## Reclaim space after null-ing LOBs (CLOB, BLOB)
After nulling LOBs disk space is not reclaimed.

```ALTER TABLE ECP.MESSAGESTOREQUEUE MODIFY LOB (TICKET) (SHRINK SPACE);```

source: (http://www.idevelopment.info/data/Oracle/DBA_tips/LOBs/LOBS_85.shtml)

## Print table sizes in filesystem
[table_sizes.sql](databases/oracle/table_sizes.sql)

## Create a new user
[new_user.sql](databases/oracle/new_user.sql)

## Disable password expiration

* First find the profile the user you want to disable password expiration has:
```
select profile from DBA_USERS where username = 'USER1';
```
* Then for this profile (DEFAULT in this case) execute the following:
```
alter profile DEFAULT limit password_life_time UNLIMITED;
```
