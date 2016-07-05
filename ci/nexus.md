### Nexus에 배포할수 있는 환경 만들기 


## Repository position 

repository 위치는 아래와 같다고 가정한다.   

* Release Repository : http://hostname:port/nexus/content/repositories/releases/
* Snapshot Repository : http://hostname:port/nexus/content/repositories/snapshots


## 개인별 .m2/settings.xml 

사용자 아이디 및 비밀번호는 Nexus admin계정으로 로그인하여 계정발급,  권한은 Nexus Deployment, Nexus development를 가지고있어야 배포를 할수있다. 또한 server id 는 실제 repository의 id와 동일해야만 정상적으로 배포가 된다.  


	<?xml version="1.0" encoding="UTF-8"?>
	<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
			  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
			  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
		<servers>
		<server>
		  <id>releases</id>
		  <username>userid</username>
		  <password>password</password>
		</server>
		<server>
		  <id>snapshots</id>
		  <username>userid</username>
		  <password>password</password>
		</server>
	  </servers>
	  
	  <profiles>
		<profile>
		  <id>eywa-dev</id>
		  <activation>
			<jdk>1.6</jdk>
		  </activation>
		  <repositories>
			<repository>
			  <id>releases</id>
			  <url>http://hostname/nexus/content/repositories/releases</url>
			  <snapshots>
				<enabled>false</enabled>
			  </snapshots>
			</repository>
			<repository>
			  <id>snapshots</id>
			  <url>http://hostname/nexus/content/repositories/releases</url>
			  <releases>
				<enabled>false</enabled>
			  </releases>
			</repository>
		  </repositories>
		</profile>
	  </profiles>
	  
	  <activeProfiles>
		<activeProfile>eywa-dev</activeProfile>
	  </activeProfiles>
	</settings>

## 프로젝트 Deployment 설정 

pom.xml 최소한 아래 내용이 들어가있어야 배포를 할수있다.  
deploy phase시 snapshot, releases에 배포할지에대한 판단은 `${project.version}` 의 Naming에따라 판단한다. 

* releases : 1.0.0 
* snapshots : 0.0.1-SNAPSHOT 

pom.xml 예제

	  <build>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-deploy-plugin</artifactId>
					<version>2.7</version>
					<configuration>
						<updateReleaseInfo>true</updateReleaseInfo>
					</configuration>
				</plugin>
			</plugins>
		</build>
		<distributionManagement>
			<repository>
				<id>releases</id>
				<url>http://hostname/nexus/content/repositories/releases</url>
			</repository>
			<snapshotRepository>
				<id>snapshots</id>
				<url>http://hostname/nexus/content/repositories/snapshots</url>
			</snapshotRepository>
		</distributionManagement>


## Trouble shooting 

deploy시 Return code 40x 하면서 에러가나는 문제는 거진 설정문제다

* 서버 아이디가 넥서스쪽 repositoryid와 같지않은경우 
  * id를 똑같이 맞추어준다. 
* 이미 배포된 archetype 을 다시 배포하려는 경우 
  * 넥서스에 로그인해서 기존에 있던 모듈을 지우고 다시 배포하면 된다. 단 이미사용하고있는 public repository일경우에는 신중해야함 

## References 

* [Nexus deploy](http://swtest.co.kr/72) 

