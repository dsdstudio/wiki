## Mysql

## Windows 서비스 등록

```
// 등록
mysqld --install <service_name>
// 제거
mysqld --remove <service_name>
```


```
mysql --defaults-file= --explicit-defaults-for-timestamp=TRUE --log-error=logfilepath --character-set-server=utf8 --collation-server=utf8_general_ci
```

## Schema 있는지 확인하기

	select schema_name from information_schema.schemata where schema_name='dbname';
	show databases like 'dbname';

## DDL

	create database if not exists dbname;

## 사용자 접근 권한 설정


mysql 바이너리버전 설치 및 설정

## mysql 설치

	#>groupadd mysql
	#>useradd -g mysql mysql
	#>cd /app/src/
	#>tar xfvpz mysql-5.5.1-m2-linux-x86_64-glibc23.tar.gz 
	#>mv mysql-5.5.1-m2-linux-x86_64-glibc23 /usr/local/mysql
	#>ln -s /usr/local/mysql /app/mysql 
	#>cd ../mysql
	#>chown -R mysql:mysql . 
	#>scripts/mysql_install_db --user=mysql 

	#>chown -R root:root . 
	#>chown -R mysql:mysql data 

## my.cnf 설정 (utf8 기반으로 변경) 

	#>mv support-files/my-huge.cnf /etc/my.cnf 

	[client] 항목을 찾아 아래 라인을 추가한다. 
	default-character-set=utf8 

	[mysqld] 항목을 찾아 아래 라인을 추가한다. 
	character_set_server = utf8


## 서버 시작과 shutdown 
서버 시작과 shutdown 은 루트계정으로 실행한다. 

mysql 서버 시작 

	#>bin/mysqld_safe --user=mysql & 
	[1] 13188
	[root@omsdev bin]# 100621 19:13:53 mysqld_safe Logging to '/usr/local/mysql/data/omsdev.wiselog.com.err'.
	100621 19:13:53 mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql/data

혹은 

	#>support-files/mysql.server start 
	Starting MySQL.                                            [  OK  ]



mysql 서버 shutdown

```
# bin/mysqladmin shutdown
100621 19:14:16 mysqld_safe mysqld from pid file /usr/local/mysql/data/omsdev.wiselog.com.pid ended
[1]+  Done                    ./mysqld_safe --user=mysql
```

혹은

```
#>support-files/mysql.server stop
Shutting down MySQL.                [  OK  ]
```

## 권한 설정

	#>bin/mysql -uroot mysql 
	루트계정 패스워드 설정 
	mysql> update user set password=password('{password}') where user='root';

기존 사용하지않는 사용자 제거 

	mysql> delete from user where user='';

설정 업데이트 

	mysql>commit;
	mysql>flush privileges; 

사용자 추가하기 

    mysql> GRANT ALL PRIVILEGES ON schemaname.* TO username@localhost IDENTIFIED BY 'password' WITH GRANT OPTION; 


## db dump 

sqlfile -> load to db 

	$ mysql -u<username> -p<password> <dbname> < <sqlfilename> 

## 테이블의 datadir 을 다른곳으로 해서 만들기 


	mysql> USE test;
	Database changed

	mysql> SHOW VARIABLES LIKE 'innodb_file_per_table';
	+-----------------------+-------+
	| Variable_name         | Value |
	+-----------------------+-------+
	| innodb_file_per_table | ON    |
	+-----------------------+-------+
	1 row in set (0.00 sec)

	mysql> CREATE TABLE t1 (c1 INT PRIMARY KEY) DATA DIRECTORY = '/alternative/directory';
	Query OK, 0 rows affected (0.03 sec)

	bhkim@ubuntu:~/alternative/directory/test$ ls
	t1.ibd

	bhkim@ubuntu:~/mysql/data/test$ ls
	db.opt  t1.frm  t1.isl


## mysql 5.7 업그레이드시 hibernate 관련 에러 
	
	org.springframework.jdbc.CannotGetJdbcConnectionException: Could not get JDBC Connection; nested exception is org.apache.tomcat.dbcp.dbcp.SQLNestedException: Cannot create PoolableConnectionFactory (Table 'performance_schema.session_variables' doesn't exist)
		at org.springframework.jdbc.datasource.DataSourceUtils.getConnection(DataSourceUtils.java:80)
		at org.springframework.jdbc.core.JdbcTemplate.execute(JdbcTemplate.java:630)

내부적으로 사용하고 있는 performance_schema가 갱신되지않아 생기는 문제이다 

아래 명령어로 복구가 가능 

나의 경우는 osx를 사용중이므로 `brew services`로 재시작, OS별 daemon 관리자는 다르므로 플랫폼별로 알아서 ^^;

	# mysql_upgrade -uroot -p 
	# brew services restart mysql 
