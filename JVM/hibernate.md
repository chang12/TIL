## MySQL 설정

[Accessing data with MySQL](https://spring.io/guides/gs/accessing-data-mysql/) 가이드를 정독하면 된다.

### build.gradle

```groovy
dependencies {
    ...
    compile('mysql:mysql-connector-java')
}
```

### src/main/resources/application.properties

```
spring.jpa.hibernate.ddl-auto=create
spring.datasource.url=jdbc:mysql://localhost:3306/db_example
spring.datasource.username=springuser
spring.datasource.password=ThePassword
```

### storage engine issue

hibernate 는 기본 설정으로 MyISAM storage engine 을 사용한다. stack overflow 답변들을 보면 InnoDB 를 사용하기 위해선 아래와 같이 property 를 선언하라고 한다. **spring.jpa.properties** prefix 를 달아주면 spring 이 알아서 library 들에게 forward 해준다고 한다.

```
# application.properties
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect

# console
org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.MySQL5InnoDBDialect

# create table
create table Dummy1 (
    id integer not null, 
    primary key (id)
) engine=InnoDB
```

그런데 `MySQL5InnoDBDialect` 의 javadoc 을 보면 `@deprecated Use "hibernate.dialect.storage_engine=innodb" environment variable or JVM system property instead.` 라고 적혀있다. 그러니 application.properties 에 선언하는게 아니라, env 를 추가하거나 java 실행할때 system property 로 넘겨야할 것이다.
