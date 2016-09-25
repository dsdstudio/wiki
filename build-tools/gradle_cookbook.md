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
