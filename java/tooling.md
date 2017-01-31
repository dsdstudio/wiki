## Maven / Gradle의 dependency scope에 관하여 .. 


빌드와 실행과정에는 아래와 같은 생명주기가 있다. 

1. compile 

일반적인 의존 라이브러리들이 대부분 여기에 속한다.  
컴파일시 (javac) classpath에 걸린다.   
실행관련 플러그인으로 실행시 classpath에 걸린다 `java -cp your_deps.jar....`  
war, 혹은 fat jar 패키징시 같이 묶인다.   


2. runtime 

실행시에만 classpath 에 걸린다.   
war, fat jar 패키징시 같이 묶인다.   

3. provided 

대표적인 예로 embedded tomcat이 여기에 속한다.  
war 파일 패키징후 배포해서 사용할때는 해당라이브러리가 포함되어 있지 않아도 되므로   
컴파일시 classpath에 걸린다.   
실행 / 패키징시에는 같이 묶이지 않는다. 

| | 컴파일 | 실행 | 패키징 | 
|---|---|---|---|
| compile | O | O | O |
| runtime | x | O | O |
| provided | O | x | x |
