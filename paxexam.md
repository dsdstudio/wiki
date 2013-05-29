## Pax-exam 3 로 OSGi Application 테스트하기 

[Pax exam 3](https://ops4j1.jira.com/wiki/display/PAXEXAM3/Pax+Exam) 은 OSGi application 을 테스트하기 위한 프레임워크이다. 예전에 pax-runner 로 엄청 삽질했던 기억이 있는데 apache-felix-ipojo 소스코드를 들여다보다 pax-exam 으로 깔끔하게 통합테스트가 구성된것을 볼수 있었다.  이를 보고 영향을 받아 한번 정리해보려한다. 실제 지금도 OSGi 환경에서 작업을 하고있기도하고 이후에 통합테스트를 작성해야하므로 .. 미리미리 준비해두자 ㅋㅋ 

여기서 중요하게 짚고 넘어가야할것이 있다. 

우리가 생각하는 일반적인 테스트는 Unit 테스트이다. (해당 클래스들의 기능이 정상적으로 동작하나 Test Suite 를 작성해서 확인하는방법..)

하지만 OSGi 환경에서의 테스트는 초기에 OSGi container 가 올라오고, 번들이 설치되고, 번들내에 직접 개발한 class instance 가 service registry 에 등록되어야되고, 서로 다른 번들에서 providing 한 service 끼리 서로 얽힌다 .. 헥헥 과정이 많다 =_= 이런 작업들이 선행되어야하는데 ***OSGi 환경에서의 테스트는 통합 테스트***이다. 

서로 다른 이기종의 번들간의 의존성, 의존이 주입된상태의 기능 테스트 등을 할것이기 때문이다.  

## 의존성 명시하기 

초 간단한 OSGi 프로젝트를 만들어보자. 

이 프로젝트는 기본적으로 Pax Exam 3 과 native container를 사용할것이다. 기본 의존성은 [Pax exam 문서](https://ops4j1.jira.com/wiki/display/PAXEXAM3/Maven+Dependencies#MavenDependencies-NativeContainerExample)에 잘 정리되어있다.  또한 이것이 동작하려면 기본 OSGi 컨테이너에 대한 의존성을 명시해줘야되는데 여기에서는 Apache Felix 를 사용하겠다. 

	<dependency>
    	<groupId>org.apache.felix</groupId>
    	<artifactId>org.apache.felix.framework</artifactId>
    	<version>4.2.1</version>
    	<scope>test</scope>
	</dependency>
	

## 테스트코드 작성하기 

정말 간단한 테스트코드를 작성해보도록 하자. 이 테스트는 간단히 OSGi 컨테이너에 설치된 번들들 리스트를 출력하는 테스트이다. 


	@RunWith(PaxExam.class)
	@ExamReactorStrategy(PerMethod.class)
	public class OSGiIntegrationTest {

    	@Inject
    	private BundleContext ctx;

    	@Configuration
    	public Option[] config() {
        	// Pax exam 은 로그를 너무많이 남기므로 로깅 레벨을 INFO로 조절한다.
        	Logger root = (Logger) LoggerFactory.getLogger(Logger.ROOT_LOGGER_NAME);
        	root.setLevel(Level.INFO);
        	return options(
                junitBundles(),
				systemProperty("org.ops4j.pax.logging.DefaultServiceLog.level").value("WARN")
        	);
    	}

    	@Test
    	public void test() {
        	for (Bundle bundle : this.ctx.getBundles()) {
            	System.out.println(bundle);
        	}
    	}
	}
	
	
기본적으로 Junit test annotation 을 이용해서 테스트를 작성할수있다. 

최 상단 코드를 보면 `@RunWith` Annotation 을 이용하여 PaxExam class 로 실행하도록 설정되어있다. 아마 내부적으로 OSGi Container 를 초기화하고 Field injection 을 수행할것이다. 
두번째 라인에있는 `@ExamReactorStrategy(PerClass.class)` 요것은 아마도 실행시에 OSGi container 초기화 단위를 Class 단위로 할것인지 Method 단위로 할것인지 설정하는 패러미터이다. `PerMethod.class`도 있는데 아마 문제가 많았던지 현재 Deprecated 상태다. 


그다음 `@Inject` Annotation 을 이용하여 BundleContext 를 주입받았다. BundleContext 뿐만이 아니라 OSGi Service Registry 에 등록된 객체라면 모두 `@Inject`Annotaion 을 이용해서 주입받을수 있다. 


`@Configuration` annotation 을 준 Method 에서는 OSGi 컨테이너가 시작하기전에 기본적인 설정이나, 어떤 번들들이 설치될지 명시할수있다. 그밖에 자세한것은 `org.ops4j.pax.exam.CoreOptions.*`를 살펴보자 


maven test phase 로 실행해보자 

	> mvn test
	-------------------------------------------------------
	 T E S T S
	-------------------------------------------------------
	Running test.OSGiTest
	02:18:21.875 [main] INFO  org.ops4j.pax.exam.junit.PaxExam - creating PaxExam runner for class test.OSGiTest
	02:18:21.900 [main] INFO  o.o.pax.exam.spi.DefaultExamSystem - Pax Exam System (Version: 3.0.0) created.
	02:18:21.905 [main] DEBUG o.ops4j.store.intern.TemporaryStore - Storage Area is /var/folders/mp/nkqq8grs5rq0wg0594tyvpcc0000gn/T/1369761501904-0
	02:18:21.914 [main] DEBUG o.ops4j.pax.exam.spi.PaxExamRuntime - Found TestContainerFactory: org.ops4j.pax.exam.forked.ForkedTestContainerFactory
	02:18:21.963 [main] INFO  org.ops4j.pax.exam.junit.PaxExam - running test class test.OSGiTest
	02:18:22.043 [main] INFO  org.ops4j.exec.DefaultJavaRunner - DefaultJavaRunner completed successfully

	org.apache.felix.framework [0]
	….
	02:18:23.763 [main] INFO  org.ops4j.exec.DefaultJavaRunner - Platform has been shutdown.
	02:18:23.763 [main] INFO  o.o.p.e.spi.reactors.ReactorManager - suite finished
	Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 2.192 sec
	…. 
	
자 Test method 에서 의도한대로 Bundle list 목록들을 볼수있었다 :) 

[Source Code Link](https://github.com/dsdstudio/blog-paxexam3-test/commit/b8556da3182a365a73379364fadbb81c901268c9)

## Maven Profile을 이용하여 다양한 Container 위에서 실행하기 

유감스럽게도 Pax exam 3 은 테스트들을 실행할때 여러 OSGi framework 상에서 실행하는것이 불가능하다. 
다행스럽게도 OSGi 구현체가 classpath(정확하게는 Maven dependencies)내에 있다는 것을 감지할수있다. 여기에 약간의 설정을 더하면 의도하던대로 특정 OSGi framework 위에서 테스트가 동작하게 할수있다. 

Maven profile 을 이용하자. 아래 3개의 설정은 각각의 다른 OSGi framework 구현체들위에서 동작하도록 설정한것이다. 

	<profile>
		<id>knopflerfish</id>
		<activation>
			<activeByDefault>false</activeByDefault>
			<property>
				<name>pax.exam.framework</name>
				<value>knopflerfish</value>
			</property>
		</activation>
		<properties>
			<pax.exam.framework>knopflerfish</pax.exam.framework>
		</properties>
		<repositories>
			<repository>
				<id>knopflerfish-releases</id>
				<url>http://www.knopflerfish.org/maven2</url>
			</repository>
		</repositories>
		<dependencies>
			<dependency>
				<groupId>org.knopflerfish</groupId>
				<artifactId>framework</artifactId>
				<version>5.2.0</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</profile>

	<profile>
		<id>equinox</id>
		<activation>
			<activeByDefault>false</activeByDefault>
			<property>
				<name>pax.exam.framework</name>
				<value>equinox</value>
			</property>
		</activation>
		<properties>
			<pax.exam.framework>equinox</pax.exam.framework>
		</properties>
		<dependencies>
			<dependency>
				<groupId>org.eclipse.tycho</groupId>
				<artifactId>org.eclipse.osgi</artifactId>
				<version>3.8.1.v20120830-144521</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</profile>

	<profile>
		<id>felix</id>
		<activation>
			<activeByDefault>true</activeByDefault>
			<property>
				<name>pax.exam.framework</name>
				<value>felix</value>
			</property>
		</activation>
		<properties>
			<pax.exam.framework>felix</pax.exam.framework>
		</properties>
		<dependencies>
			<dependency>
				<groupId>org.apache.felix</groupId>
				<artifactId>org.apache.felix.framework</artifactId>
				<version>4.2.0</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</profile>  
	
요 3가지 설정중 `<profile>` element 섹션 내부에 붙여넣으면 된다. 별도의 프로파일설정이 없다면 기본적으로 default 로 설정된 profile을 기준으로(felix) 동작할것이다. 

실행 결과 

	> mvn clean test 
	결과로그 생략 :) 
	=> felix 로 구동 
	
	> mvn clean test -Pequinox 
	=> equinox 로 구동  
	
	> mvn clean test -Pknopflerfish
	=> knopflerfish 로 구동 
	

## Conclusion 
의존관계가 얽혀있는 OSGi 환경에서도 Pax exam 3 + maven 을 이용하여 다양한 OSGi 구현체 환경에서 테스트해볼수 있었다. 혹시라도 OSGi 환경에서 통합테스트가 막막했던 분들에게 도움됐기를 바라며 이글을 마친다. 

PS. Apache felix / felix- ipojo 프로젝트도 pax-exam3 으로 구성되어있으니 참고하면 좋을듯 하다. 

### References 


- [ipojo 커미터인 Clement Escoffier 의 paxexam3 정리글](http://www.dynamis-technologies.com/blog/testing-osgi-applications-on-several-frameworks-with-pax-exam-3/)
- [Apache Felix project 실제 통합테스트 설정파일](https://github.com/apache/felix/blob/trunk/ipojo/runtime/api-it/pom.xml)