# Nginx surbey 

## Feature 

* Opensource 고성능 HttpServer, reverse proxy, IMAP/POP3 proxy Server 
* 러시아 개발자인 Igor Sysoev 가 2002년에 만들고, 2004년에 공개버전을 릴리즈함  
* 현재 전세계사이트중 (점유율)12% 정도가 nginx로 운영되고있음 
* C10K Problem 을 해결하기위해 만들어진 몇 안되는서버중 하나 
* Async Event based(ioctl, send, recv, epoll)
	* Thread에 의존적이지않으므로 대용량 Request에서도 메모리를 적게 사용합니다.
	* 실제 아파치와 같은환경에서의 벤치마크를 보면 대용량 request에서도 메모리 사용량이 압도적으로 적습니다. 
* 설정이 단순, 명료하다 (개인적인 시각)  

단점 

* 아파치에비해서 모듈이 별로 없다 (그래도 왠만한것들은 다 갖추고있음)


> [C10k Problem](http://www.kegel.com/c10k.html)란 80년대에 기가비트 통신망이 구축되지않았을때 대용량 트래픽에 대해 미리 여러가지 이슈들에 대비하자고 꼭 해결해야할 이슈들을 정리해놓은 것들입니다. 

**점유율** 

![](http://news.netcraft.com/wp-content/uploads/2011/12/wpid-overallc1.png)

### 기능들

* name, ip , virtualhost 
* SSL 
* load balancer
* gzip request 
* url rewrite
* webdav
* access authentication (BASIC DIGEST … )
* smtp, pop3, imap 
* flv/mp4 streming 
* speed limitation 

## nginx로 운영되는 Reference Site

* Wordpress
* Hulu
* Github 
* Ohloh
* SourceForge

## 벤치마크 데이터 

![](http://www.webfaction.com/blog/nginx-apache-reqs-sec.png)

![](http://www.webfaction.com/blog/nginx-apache-memory.png)

## 설정 예제 

**1. Load Balancing**
	
	http {
		upstream myproject {
    		server 127.0.0.1:8000 weight=3;
    		server 127.0.0.1:8001;
    		server 127.0.0.1:8002;    
    		server 127.0.0.1:8003;
    	}
    	
    	server {
    	    listen 80;
    	    server_name www.domain.com;
    	    location / {
      			proxy_pass http://myproject;
      		}
      	}
    }
   
**2. 속도 제한 예제**

	limit_req_zone  $binary_remote_addr  zone=one:10m   rate=1r/s;
 
    server {
        location /search/ {
            limit_req   zone=one  burst=5;
        }

**3. reverse proxy**

    server { 
        listen 80;
        server_name ci.wiselog.com;
        location / {
            proxy_pass  http://localhost:8009; // tomcat 과 연결 
        }       
    }

**4. mp4 streming**
	
	location /video/ {
    	mp4;
    	mp4_buffer_size     1m;
    	mp4_max_buffer_size 5m;
	}
### Advance Load balancer Configuration 

기본적으로 nginx는 simple round-robin 방식으로 운영됩니다.  
Syntax 

	upstream [lbName(alias)] {
		server [hostname or ipaddr] [option=value|option]
		....	
	}
	
`upstream argument`를정리해보면 

* weight 
	* Number으로 입력 가능하고 별도로 지정하지않으면 **1**의 가중치로 작동합니다. 
	트래픽의 큰 비중을 받게 하고싶다면 높은 가중치를 설정하면 됩니다.(장비빨이 좋은놈이 Target이 되겠지요) 
* max_fails
	*  연결 실패시에 몇번이나 재시도 할것인지를 설정합니다 기본값은 **1**이고 `서버가 죽으면 nginx 에서 0으로 바꾸어줍니다.` 0은 이서버는 죽었다라는 의미로 사용합니다 ^^, 비운영모드는 기본적으로 10초동안 유지됩니다. 
* fail_timeout
	* second단위로 입력합니다. 서버가 비운영모드에서 운영모드로 살아나는데 까지의 시간을 의미합니다, 참고로 404응답을 반환받은 서버는 운영서버로 간주되고, proxy뒷단 서버에까지 영향을 주진 않습니다. `기본값은 10입니다.`
* down 
	* 명시적으로 서버를 offline상태로 설정합니다. 
* backup 
	* 운영모드로 있는 서버들이 모두 offline,혹은 대역폭을 모두 사용할때만 사용합니다.   

예제  
 
	upstream appcluster {
		server app.wiselog.com:8801;
		server app.wiselog.com:8802 weight=1;
		server app.wiselog.com:8803 weight=2 max_fails=2;
		server app.wiselog.com:8804 weight=2 max_fails=2 fail_timeout=20;
		server app.wiselog.com:8805 weight=4;
		server app.wiselog.com:8806 weight=4 fail_timeout=4;
		server app.wiselog.com:8807 weight=2 fail_timeout=20;
	}
	
**해설** 

편의를 위해서 세 그룹으로 나누었습니다. 

* AGroup (8801,8802)
* BGroup (8803,8804,8807)
* CGroup (8805,8806)


**Traffic balance 기준 해설** 

A그룹은 nginx로 부터 가중치 1로 취급될것입니다. BGroup은 A그룹보다 2배의 request를 더 받을것이고, C그룹은 A그룹의 4배 트래픽을 받을 것입니다. 

**fail over** 

* 8801,8802,8805,8806,8807 은 connection이 끊기면 비운영모드로 전환됩니다. (max_fails=0), 
* 8803,8804 는 2번 연결 실패시 비운영모드로 전환
* 8801,8802,8803,8805 는 기본 fail_timeout이 10초입니다. 
* 8804,8807은 명시적으로 설정해두었으므로 fail_timeout 이 20초입니다. 
* 8806은 fail_timeout reset 시간이 4초입니다.  


> 정리하면 nginx 는 설정을 통해 서버별 트래픽양, request실패횟수, fail_timeout시간 등을 설정할수 있습니다. (자동으로 해주진 않고 적절한 값을 찾아내는건 System operator의 몫이겠지요 ;) 

## References 

* [Nginx Wiki](http://wiki.nginx.org/Main)
* [Nginx,Varnish,G-Wan,Lighthttpd,ApacheTraffic Server Benchmark](http://nbonvin.wordpress.com/2011/03/24/serving-small-static-files-which-server-to-use/)
* [Apache-nginx-performance deathmatch](http://joeandmotorboat.com/2008/02/28/apache-vs-nginx-web-server-performance-deathmatch/)
* [2012 Web Server 점유율](http://news.netcraft.com/archives/2012/01/03/january-2012-web-server-survey.html)
* [nginx ajp thirdparty module](https://github.com/yaoweibin/nginx_ajp_module)
* [Nginx vs Apache](http://blog.webfaction.com/a-little-holiday-present)
* [Nginx Advanced LoadBalance](http://library.linode.com/web-servers/nginx/configuration/front-end-proxy-and-software-load-balancing)
* [Nginx loadbalance upstream Module](http://wiki.nginx.org/NginxHttpUpstreamModule)