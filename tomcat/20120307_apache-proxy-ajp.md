## apache proxy-ajp tomcat 연동 

apache(v2.2.15) tomcat(v6.0.26) 

apache 기본포트 80 tomcat ajp 포트 8009 라는 가정하에 작성 되었다. 

apache 의 mod_proxy_ajp 모듈을 이용한 연동 방식. 

apache 설치

	#>cd /app/src
	#>tar xfvpz httpd-2.2.15.tar.gz 
	#>cd /app/src/httpd-2.2.15  (소스파일 압축 풀린 디렉토리) 

컴파일 및 설치 


	#> CC="gcc" CFLAGS="-O2" ./configure --prefix=/app/apache --enable-so --enable-proxy-ajp --enable-cgi --enable-rewrite --enable-speling --enable-usertrack --enable-deflate --enable-ssl --enable-cache --enable-disk-cache --enable-expires --enable-file-cache --enable-headers --enable-mem-cache --enable-mime-magic --enable-proxy --enable-mods-shared=all --with-mpm=worker
	#>make 
	#>make install 
	#>cd /app/apache 


/app/apache/conf/httpd.conf 설정 수정 

vhost 사용부분 주석 해제 

	Include conf/extra/httpd-vhosts.conf


맨 아랫쪽 라인에 추가할 내용. 

	# tomcat 연동을 위한 mod_proxy_ajp 사용설정  
	ProxyRequests On  
	ProxyVia On
	# 여기에 추가될 컨텍스트를 만들면 된다. 
	ProxyPass /oms/ ajp://localhost:9090/oms/
	# .jsp|.do 요청에대해서만 tomcat 쪽으로 request 를 넘긴다. 
	ProxyPassMatch ^/.*\.(jsp|oms)$ ajp://localhost:9090/
	# Proxy 요청 localhost에서만 가능하도록 처리  
	<Proxy *>
	    Order Deny,Allow
	    Deny from all 
	    Allow from localhost
	</Proxy>
	# 보안을 위해 webapp 하부 특정 디렉토리 접근 불가 설정
	<LocationMatch "/WEB-INF">
	    deny from all
	</LocationMatch>
	<LocationMatch "/META-INF">
	    deny from all
	</LocationMatch>


conf/extras/httpd-vhosts.conf 수정 


	<VirtualHost *:80>
	    ServerAdmin omdev@omsdev.wiselog.com
	    DocumentRoot /adata/coreapp/omsweb
	    ServerName omsdev.wiselog.com
	    ErrorLog "logs/omsdev.wiselog.com-error_log"
	    CustomLog "logs/omsdev.wiselog.com-access_log" common
	    RewriteEngine On
	    RewriteCond %{REQUEST_FILENAME} \.(htm|html|xhtml|js$|css|jpg|gif|png|swf)$
	    RewriteRule (.*) - [L] 
	    RewriteRule (.*) ajp://localhost:9090$1 [P] 
	</VirtualHost>

static resource 확장자는 얼마든지 추가하면 된다. 


## tomcat 설치 
tar 풀기 


	#>cd /app/src 
	#>tar xfvpz apache-tomcat-6.0.26.tar.gz 
	#>mv apache-tomcat-6.0.26 /app/tomcat 
	#>cd /app/tomcat/conf

server.xml 설정 

	<?xml version='1.0' encoding='utf-8'?>
	<Server port="8005" shutdown="SHUTDOWN">
	  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
	  <Listener className="org.apache.catalina.core.JasperListener" />
	  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
	  <Listener className="org.apache.catalina.mbeans.ServerLifecycleListener" />
	  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
	  <Service name="Catalina">
	    <Connector port="8080" protocol="HTTP/1.1" 
	               connectionTimeout="20000" 
	               redirectPort="8443" />
	    <Connector port="9090" protocol="AJP/1.3" address="127.0.0.1" maxThreads="150" minSpareThreads="25" maxSpareThreads="75" enableLookups="false" redirectPort="8443" acceptCount="100" debug="0" connectionTimeout="20000" disableUploadTimeout="true" URIEncoding="UTF-8" />    
	    <Engine name="Catalina" defaultHost="localhost">
	      <Host name="localhost"  appBase="webapps"
	            unpackWARs="true" autoDeploy="false"
	            xmlValidation="false" xmlNamespaceAware="false">
	      </Host>
	    </Engine>
	  </Service>
	</Server>

8009 포트 설정하면 셧다운 안되는 문제 있음 

Connector 태그에 address를 loopback interface 로 변경하여 처리하였음. 

운영시에는 HTTP/1.1 포트인 8080쪽 Connector 태그는 주석처리한다. 

catalina.out 파일 계속 append 되는 문제 수정 

/app/tomcat/catalina.sh Line 2에 내용 추가 


	export CATALINA_OUT="/dev/null" 


##로깅 간소화 

catalina관련 로그만 남도록 처리

/app/tomcat/conf/logging.properties 


	handlers = 1catalina.org.apache.juli.FileHandler, java.util.logging.ConsoleHandler
	.handlers = 1catalina.org.apache.juli.FileHandler, java.util.logging.ConsoleHandler
	1catalina.org.apache.juli.FileHandler.level = FINE
	1catalina.org.apache.juli.FileHandler.directory = ${catalina.base}/logs
	1catalina.org.apache.juli.FileHandler.prefix = catalina.
	java.util.logging.ConsoleHandler.level = FINE
	java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
