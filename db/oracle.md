## Oracle Docker Storage 관리 

- https://blog.purestorage.com/purely-technical/docker-oracle-12c-and-persistent-storage/

## Version 확인 query

```sql
SELECT * FROM V$VERSION;
```


### 사용자 생성 
```
ALTER SESSION SET "_ORACLE_SCRIPT"=true;
create tablespace {TS_NAME} datafile 'admin.dbf' size 500M;
create user {USER_NAME} IDENTIFIED BY '{PASSWORD}' DEFAULT TABLESPACE {TS_NAME}
grant CREATE SESSION to {USER_NAME} WITH ADMIN OPTION
grant CREATE TABLE to {USER_NAME} WITH ADMIN OPTION
alter user PLMS default tablespace TS_PLMS quota unlimited on TS_PLMS;

```
