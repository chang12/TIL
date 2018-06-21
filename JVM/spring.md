## Web Client

[34. Calling REST Services with WebClient](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-webclient.html) 로 처음 접했다. 

* `WebClient` 랑 `RestTemplate` 를 비교하는걸보니 서로 대체할 수 있는 관계인가보다. `WebClient` 가 더 functional 하고 reactive 하단다. 
* `WebClient.Builder` 도 interface 인데 spring boot 가 auto configure 해준단다.

## @SpringBootApplication [[link](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-using-springbootapplication-annotation.html)]

`@SpringBootApplication` = `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan` 이다. 따라서,

* `@Bean` annotation 이 달린 method 를 구현함으로써 **새로운 bean 을 context 에 등록**할 수 있다.
* **Spring Boot's auto-configuration mechanism** 을 enable 시킨다는데... 사실 정확히 모르겠다.
* **application 이 위치한 package** 에 대한 component scan 을 enable 시킨다.