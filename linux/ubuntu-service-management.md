## Ubuntu에서 부팅시 서비스 등록하기 

기본스크립트는 `/etc/init.d/program_name` 으로 등록한다. 

여기에 실행 스크립트만 넣으면 되는것이 아니라,  

`update-rc.d defaults` 명령어로 등록을 해주어야한다. 
`update-rc.d` 요녀석의 역할은 /etc/rc{runlevel}.d/ 밑에 심볼릭 링크를 만들어주는 역할이다. 
이렇게 돌아가는걸로 봐서는 Linux가 부팅될때 /etc/rc{runlevel}.d 밑에 있는 녀석들을 실행 시켜주는 구조로 보인다.

	root@pethostel:/var/log# update-rc.d tomcat defaults
	update-rc.d: warning: /etc/init.d/tomcat missing LSB information
	update-rc.d: see <http://wiki.debian.org/LSBInitScripts>
	 Adding system startup for /etc/init.d/tomcat ...
	   /etc/rc0.d/K20tomcat -> ../init.d/tomcat
	   /etc/rc1.d/K20tomcat -> ../init.d/tomcat
	   /etc/rc6.d/K20tomcat -> ../init.d/tomcat
	   /etc/rc2.d/S20tomcat -> ../init.d/tomcat
	   /etc/rc3.d/S20tomcat -> ../init.d/tomcat
	   /etc/rc4.d/S20tomcat -> ../init.d/tomcat
	   /etc/rc5.d/S20tomcat -> ../init.d/tomcat

등록된 서비스를 해지할때 

	root@pethostel:/var/log# update-rc.d -f tomcat remove
	 Removing any system startup links for /etc/init.d/tomcat ...
	   /etc/rc0.d/K20tomcat
	   /etc/rc1.d/K20tomcat
	   /etc/rc2.d/S20tomcat
	   /etc/rc3.d/S20tomcat
	   /etc/rc4.d/S20tomcat
	   /etc/rc5.d/S20tomcat
	   /etc/rc6.d/K20tomcat

## 참조 링크 

- https://www.debian-administration.org/article/28/Making_scripts_run_at_boot_time_with_Debian
- https://wiki.debian.org/LSBInitScripts

