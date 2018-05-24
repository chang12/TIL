## Install
[설치 링크](https://maven.apache.org/install.html)
```shell
> JAVA_HOME = C:\Program Files\Java\jdk1.8.0_91 # maven이 JDK를 인식하도록 환경변수 설정
> unzip apache-maven-3.3.9-bin.zip # 다운로드받은 zip 파일의 압축을 풀고 적절한 위치로 이동
> PATH = $PATH;폴더를_옮긴_경로\bin # bin 폴더를 Path에 추가  
```
## Run
[커맨드 기본 설명 링크](https://maven.apache.org/run.html)
```
mvn clean install
```
- 모든 **module** 빌드
- **local repository**에 설치

**local repository**는 다운로드한 모든 바이너리 파일과, 빌드한 프로젝트가 저장되는 장소이다. 명령어 실행 후에 target 서브 디렉토리를 보면 빌드 결과물과, 최종 라이브러리 혹은 어플리케이션이 있을 것이다.

가장 **standard convention**이라고 한다. 그러나 내 목적과 거리가 있어보인다. 나는 다른 라이브러리를 가져다가 쓰고싶은 것인데, 굳이 내 프로젝트를 빌드해서 **local repository**에 저장할 필요가 있을까? 가장 간단한 튜토리얼을 체험해보고 싶다. 

궁금해서 `mvn clean install` 을 실행하니 `pom.xml` 파일이 존재하지 않아 프로젝트를 찾을 수 없다고 한다.

## Maven in 5 Minutes
[가이드 링크](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
* maven 프로젝트를 생성하는 법
* plugin-goal과 phase의 간단한 소개

`mvn package` 커맨드를 실행하니 target 디렉토리 아래에 jar 파일이 생성되었다. 이를 `java -jar` 커맨드로 실행하니 기본 Manifest 속성이 없다며 실행되지 않았다.

가이드 문서에 소개된 개념들(plugin-goal, phase, archetype 등)을 하나하나 정리하고 넘어가거나, 혹은 바로 **Maven Getting Started Guide**로 넘어가볼 수 있겠다.

## Maven Getting Started Guide
[링크] (https://maven.apache.org/guides/getting-started/)
* change cache location
* are behind a HTTP proxy

같은 상황이라면 추가적인 configuration이 필요하다. [링크](https://maven.apache.org/guides/mini/guide-configuring-maven.html)
```
mvn -B archetype:generate \
  -DarchetypeGroupId=org.apache.maven.archetypes \
  -DgroupId=com.mycompany.app \
  -DartifactId=my-app
```
**groudId** 는 프로젝트를 개발한 기관이나 그룹의 이름이다. 유일해야하므로 보통 도메인을 사용해서 짓는다. Eclipse 처음 사용할 때 패키지 네이밍하는 방식과 동일하다. (예: org.apache.maven.plugins)

**artifaceId** 는 primary arfifact 의 base name 이다. 보통 primary arfifact 는 jar 파일이다. source bundle 같은 secondary artifact 도 이름을 지을때 이 artifactId 를 사용한다. myapp-1.0.jar 같은 이름이 보통이다.

**packaging** 은 package type 을 명시한다. artifact 이 생성되는 포맷 뿐 아니라 build lifecycle 도 결정하는 내용이다.

**version** 에서 1.0-SNAPSHOT 처럼 뒤에 SNAPSHOT 을 붙이면 development 단계임을 의미한다.
```
mvn compile
```
pom.xml 파일이 있는 위치에서 컴파일 명령을 실행할 수 있다. src 디렉토리와 같은 계층에 target 디렉토리가 생기고 .class 파일이 생성되며, 컴파일 상태(status)가 기록된 디렉토리와 파일도 생긴다. 예를 들어, 어떤 .java 파일들을 컴파일 했고, 그 결과로 어떤 .class 파일이 생성됬는지가 .lst 포맷의 문서로 생성된다.
```
mvn test 
```
main 코드를 (다시) 컴파일 하고, test 코드도 컴파일 하고, 테스트를 실행한다.
```
mvn test-compile
```
테스트를 위한 컴파일 과정만 진행하고, 테스트는 하지 않고 싶은 경우에 사용할 수 있는 커맨드이다.
```
mvn package
```
main 코드 컴파일 하고, test 코드 컴파일 하고, 테스트 실행하고, jar 파일을 생성한다. target 디렉토리에 arfifactId-version.jar 이름으로 생성된다.
```
mvn install
```
main 코드 컴파일, test 코드 컴파일, 테스트 실행, jar 파일 생성하고 .m2 디렉토리 아래에 pom.xml과 jar 파일을 설치한다.
```
mvn clean
```
target 디렉토리의 내용물을 지운다.

**${basedir}/src/main/resources** 하위에 파일들을 집어넣으면, JAR 파일 생성때 함께 패키징 된다. resources 하위 경로가 JAR 파일에 그대로 적용된다. 
**${basedir}/src/test/resources** 하위에 넣으면 테스트 환경에서 사용할 수 있다. 만약 resources 디렉토리에 test.properties 라는 파일을 집어넣었다고 한다면, Java 코드에서 이를 꺼내쓸 수 있다.
```java
// Retrieve resource
InputStream is = getClass().getResourceAsStream( "/test.properties" );
 
// Do something with the resource
```

dependencies 들은 pom.xml 에 dependencies section에 하나 하나의 dependency section 에 기술된다. 처음에 local repository 에 존재하는지 찾고, 없다면 remote repository 에서 local repository 로 다운로드 받으려 시도한다.