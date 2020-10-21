
## JVM 

### jConsole Remote Debugging 

```java 
java -Dcom.sun.management.jmxremote.port=9999 \
     -Dcom.sun.management.jmxremote.authenticate=false \
     -Dcom.sun.management.jmxremote.ssl=false \
     com.example.Main
```

####  Reference

- https://www.lesstif.com/java/apache-tomcat-jmx-monitoring-20776824.html
- https://docs.oracle.com/javase/tutorial/jmx/remote/jconsole.html
### OOM HeapDump
서버가 OutofMemoryError로 죽었을때 Heapdump를 할지 설정하는 옵션이다. 실제 was 인스턴스가 죽었을때 이 dump파일이 문제를 찾는 결정적인 역할을 한다. 

	-XX:-HeapDumpOnOutOfMemoryError
	-XX:HeapDumpPath=/user/dir


머신의 메모리 한계 알아내기 

	  java -XX:+PrintFlagsFinal -version 2>&1 | grep -i -E 'heapsize|permsize|version'
    uintx AdaptivePermSizeWeight                    = 20              {product}           
    uintx ErgoHeapSizeLimit                         = 0               {product}           
    uintx HeapSizePerGCThread                       = 87241520        {product}           
    uintx InitialHeapSize                          := 79079552        {product}           
    uintx LargePageHeapSizeThreshold                = 134217728       {product}           
    uintx MaxHeapSize                              := 1266679808      {product}           
    uintx MaxPermSize                               = 85983232        {pd product}        
    uintx PermSize                                  = 21757952        {pd product}        
	java version "1.7.0_17"

JDK 7 에서 callback 형태로 사용하는경우 로그 깔끔하게보기 

	-XX:-UseSplitVerifier	

## References 

- [OTN](http://www.oracle.com/technetwork/java/ergo5-140223.html)
