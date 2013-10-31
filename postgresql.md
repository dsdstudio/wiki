Mysql 에서의 DB & Schema 의 개념이 Postgresql 에서는 조금 다르다.

Mysql db == schema

Postgres에서의 DB : 실제 물리적인 데이터베이스.  
Postgres에서의 schema : 논리적인 Region



	CREATE USER == CREATE ROLE WITH LOGIN 과 동일
    SUPERUSER ROLE 은 모든 권한을 포함하고있는 alias이다. 이 role 키워드를 사용하는경우 다른 role keyword와 혼용할경우 오류가 발생한다.




## Initialize db Directory

	$ initdb -D /path/to/datadir -E utf8

## 사용자 계정 설정

	$ psql -l localhost postgresql
    $ create user 'username' with password 'password';
    $ grant all privileges on database 'dbname' to tester;

## 스키마

	$ select schema_name from information_schema.schemata

## 테이블

	$ select * from pg_shadow;


## client app 이용하여 db 보기

	$ psql -h localhost -l

## Start db SERVER

	$ postgres -D /path/to/datadir


## db목록 보기

	$ select * from pg_database;
    $ select count(*) from pg_catalog.pg_database where datname = 'test';

## db 이름 바꾸기

	$ alter database {dbname} rename to {newdb}

## db 제거하기(In command line)

	$ dropdb -h localhost --username={username} {dbname}

## create db

	$ create database {dbname} owner {username} encoding 'utf8';
    $ createdb -h localhost --username={username} {dbname} --encoding='UTF8' // for command line

## 테이블 목록 보기

	$ select * from pg_tables;

## 권한 보기

	$ select * from pg_roles;

## db생성 - 사용자 생성 - db 만들기 - search_path 설정
	$ initdb.exe --encoding=utf8 --pgdata={postgre_datadir}
    $ psql.exe -h localhost -p 5432 -d postgres -c "create user wenest with superuser password 'wenest';"
	$ createdb.exe --host=localhost --port=5432 -E utf8 -O wenest wenest
    $ dropdb.exe --host=localhost --port=5432 postgres
    $ psql.exe --host=localhost --port=5432 --dbname=wenest --username=wenest --command="create schema wenest_manager authorization wenest"
    $ psql.exe --host=localhost --port=5432 --dbname=wenest --username=wenest --command="drop schema public cascade"
    $ psql.exe --host=localhost --port=5432 --dbname=wenest --username=wenest --command="alter user wenest set search_path to wenest_manager";


## Schema create & user add

	$ select * from pg_shadow where username='{username}' // 사용자 있는지 확인
    $ create user {username} with password '{password} nosuperuser'; //사용자 생성
    $ grant all privileges on all tables in schema {schemaname} to {username}; // schema에 권한 주기
    $ alter user {username} set search_path to {schemaname}
    $ SELECT count(*) FROM information_schema.schemata WHERE schema_name = '{schemaname}';// 스키마 있는지 확인
    $ create schema {schemaname} authorization {usename}

`alter user` == `alter role` user -> role 의 alias

## 사용자 권한제거, 삭제, 스키마 제거

	$ drop owned by {username};
    $ drop user {username};
    $ drop schema {schemaname} cascade;

## Windows Service 등록

	$ pg_ctl register [-N servicename] [-U username] [-P password] [-D datadir] [-S a[uto] | d[emand] ] [-w] [-t seconds] [-s] [-o options]
    $ pg_ctl unregister [-N servicename]

## Issues

remaining connection slots are reserved for non-replication superuser connections


	datadir/postgresql.conf max_connection 값 변경
    shmmax 커널 설정 변경

바이너리 버전 윈도우에서 설치시 Visual studio 가 설치되어있지않은경우 msvcr100.dll 없어서 생기는 dll 의존성 문제 발생함(2013.10.28 19:29)

* 해결방안 : vc2010 재배포 패키지 설치하면 해결됨, 단 msvcr100.dll이 존재하는지 확인하는 루틴이 필요하다. 
