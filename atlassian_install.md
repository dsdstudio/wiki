## Confluence 설치 

수정 

confluence/WEB-INF/classes/confluence-init.properties 
	
	/data/users/helloworld/atlassian/confluence/data
	
	
라이센스 문제때문에 이게 포함되어있지않다. 
mysql JDBC 드라이버 다운로드 

	> cd $atlassian_home/confluence/WEB-INF/lib
	> wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.25/mysql-connector-java-5.1.25.jar
	

문제 해결 

	InnoDB is limited to row-logging when transaction isolation level is READ 
	COMMITTED or READ UNCOMMITTED.; 
	

mysql.cnf 

	binlog_format=row
[mysql binary logging](https://confluence.atlassian.com/display/JIRAKB/MySQL+Binary+Logging+Problem+with+InnoDB+When+Creating+a+Workflow)


## jira 설치 


jira-home 설정 

	> cd $jirahome/atlassian-jira/WEB-INF/classes/jira-application.properties 