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

intellij 최신버전에서는 directory based 형태로 설정파일이 변경되었다 `${projectDir}/.idea`   
아래 디렉토리를 열어보면 다음과 같은 파일들이 있는데 . modules.xml , workspaces.xml 은 현재 gradle idea plugin 에서 customizing이 가능하나 compiler.xml 에는 대응되는 클래스가 없다. 그래서 xml을 직접 생성하여 복사하는형태로 작업을 해야하지않을까.. 하는 생각이.. 


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
