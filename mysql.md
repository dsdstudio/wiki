## Mysql 


## Schema 있는지 확인하기 

	select schema_name from information_schema.schemata where schema_name='dbname';
	show databases like 'dbname';

## DDL 

	create database if not exists dbname;

## 사용자 접근 권한 설정 

