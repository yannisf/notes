# Oracle technics

## Reclaim space after null-ing LOBs (CLOB, BLOB)
After nulling LOBs disk space is not reclaimed.

```ALTER TABLE ECP.MESSAGESTOREQUEUE MODIFY LOB (TICKET) (SHRINK SPACE);```

source: (http://www.idevelopment.info/data/Oracle/DBA_tips/LOBs/LOBS_85.shtml)
