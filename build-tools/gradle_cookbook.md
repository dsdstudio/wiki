## Gradle Cookbook 

War project 가 아닌 Jar 프로젝트에서 providedScope Dependency 이용하기 

[http://stackoverflow.com/questions/13925724/providedcompile-without-war-plugin](http://stackoverflow.com/questions/13925724/providedcompile-without-war-plugin)

	
    compile("org.apache.tomcat:tomcat-coyote:7.0.33") { provided = true }
    compile("org.apache.tomcat:tomcat-catalina:7.0.33") { provided = true }
    
    

## with nodejs 

```
buildscript {
	dependencies {
		classpath "com.moowork.gradle:gradle-node-plugin:0.13"
	}
}
apply plugin: 'com.moowork.node'
node {
	version = '6.6.0'
	npmVersion = '3.10.3'

	distBaseUrl = 'https://nodejs.org/dist'
	download = true
}
```

## intellij 에서 작업시 


최근에 사용되는 도구들을 유심히 살펴보면 전처리기가 상당히 많이 도입되었음을 알 수 있다. 
지금 현재 사용하는 annotation processor만 하더라도 lombok, hibernate jpa static model generator, QueryDSL 정도이다. 

annotation processor 를 구동하려면 별도의 gradle plugin 을 설치해야하는데, gradle task로 직접 돌릴때는 상관이 없으나, 개발자가 사용하는 IDE에서 돌릴때는 완전히 다른 환경에서 구동이 되게된다. 즉 환경의 불일치가 일어나게되므로 IDE내부에서 초기 세팅시 개발자가 직접 설정을 해야하는 문제가 있다. 

이를 자동화해주기위해서 gradle은  `gradle idea` 혹은 `gradle eclipse` 와 같은 task들을 제공한다.

intellij 의 경우에는 `.idea/compiler.xml`을 수정하여 `annotation process 활성화` 및 generated source 경로를 지정할수 있는데 gradle에서 제공하는 API로는 해당 `compiler.xml`을 수정할수 있는 방법이 없다. 

그렇다면 결국 `gradle idea` task 수행후 compiler.xml 을 직접 generate 한뒤 복사하는형태로 구현하면 이 문제를 해결할수 있지 않을까 ?

```
$ ls -l .idea
-rw-r--r--   1 bhkim  staff   1101  8 28 23:24 compiler.xml
drwxr-xr-x   3 bhkim  staff    102  8 28 23:04 copyright
-rw-r--r--   1 bhkim  staff    705  8 28 23:04 gradle.xml
drwxr-xr-x  91 bhkim  staff   3094  8 28 23:15 libraries
-rw-r--r--   1 bhkim  staff   2895  8 28 23:17 misc.xml
drwxr-xr-x   7 bhkim  staff    238  8 28 23:04 modules
-rw-r--r--   1 bhkim  staff   1571  9 23 02:26 modules.xml
-rw-r--r--   1 bhkim  staff  95227  9 23 06:34 workspace.xml
```
