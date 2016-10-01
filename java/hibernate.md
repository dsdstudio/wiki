## hibernate 관련 설정 

### Q) ConnectionProvider 는 언제 사용해야하는가 ? 

DBCP(DataBase Connection Pool)를 사용하고 있고, DB가 죽었다가 다시 복구되는 경우 connection pool 내부에 있는 connection 들은 자체적으로 IDLE connection으로 복구가 된다. 
하지만, `EntityManager` 를 직접 주입받아서 사용하는 경우에는 connection pool 에서 복구가 되더라도 이를 인지하지못하는데.. 여기에 대한 동기화 처리를 해주는녀석이 Connection Provider이다. 

SampleService.java 
```
class SampleService {
	@Autowired 
	EntityManager em;
	
	List<Customer> customerList() {
		JPAQuery jpaQuery = new JPAQuery(em);
		
		... 
	}
}
```

DB서버가 복구된 직후 위의 소스에서 `customerList()` 가 호출되는경우 pool에서 정상적인 connection이 리턴되지않고 `connection closed`오류를 보게 된다. 

이를 해결하려면 hibernate 4.x+ 의 경우 connectionProvider클래스를 사용하도록 설정을 바꾸면 된다. 
hibernate 3.x인경우 dbcp 설정에 `validationQuery` 섹션을 설정하여 동기화되지않은 connection객체를 다시 복구할수 있다. (매번 DB쪽에 validationQuery가 호출되어야되는 문제가 있음)



hibernate.properties , spring-boot 인경우 application-{profile}.properties 

```
hibernate.connection.provider_class=com.zaxxer.hikari.hibernate.HikariConnectionProvider
```

## References 

- [https://github.com/brettwooldridge/HikariCP/wiki/Hibernate4](https://github.com/brettwooldridge/HikariCP/wiki/Hibernate4)


