요새 보는 코드의 거의 모든 클래스마다 `Logger` 가 붙어있다. 클래스의 class literal 을 인자로 넘겨서 Logger 를 받아오므로, singleton 이 아닐까 추측했다. 어느 API 인지 package 를 들여다보니, **SLF4J** 라는 이름의 프로젝트였다.

> [**Simple Logging Facade for Java (SLF4J)**](http://www.slf4j.org/)

한번 훑어봐서는 정확한 의의를 파악하지 못했다. 대략 이해하기로, 여러 Logging 라이브러리들을 wrapping 하는 인터페이스를 제공하는 라이브러리였다. java.util.logging, logback, log4j 등과 binding 하는 라이브러리들을 제공한다. 

SLF4J 의 의의와는 별개로, 프로젝트가 커지면 디버깅을 비롯한 유지보수를 위해서 로그 데이터를 잘 관리해서 효과적으로 분석할 필요성이 있겠다 싶었다. 프로젝트가 크면 단일 규칙으로는 로그 기록 방식을 기술하지 못할 것이다. 그러므로 구조화된 **configuration file** 파일을 작성해서 규칙을 관리하고, 잘 연계되는 Logging API 가 필요할 것이다. 라이브러리 별 configuration file 작성에 관해서는 추후 살펴봐야할 주제.

```pom
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.0.13</version>
</dependency>
``` 

```java
Logger logger = LoggerFactory.getLogger(LoggerMain.class);

logger.trace("Trace!"); // default 설정에서 trace level 은 안찍힌다.
logger.warn("Warn!");
logger.debug("Debug!");
logger.error("Error!");
logger.info("Info!");
```

```
23:50:35.429 [main] WARN  kr.fakenerd.study.java.LoggerMain - Warn!
23:50:35.444 [main] DEBUG kr.fakenerd.study.java.LoggerMain - Debug!
23:50:35.444 [main] ERROR kr.fakenerd.study.java.LoggerMain - Error!
23:50:35.444 [main] INFO  kr.fakenerd.study.java.LoggerMain - Info!
```