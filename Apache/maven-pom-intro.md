## Introduction to the POM
[문서 링크](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)

pom 은 Project Object Model 의 줄임말이다. 뉴비들은 POM 에 대해서 먼저 익히는 것이 순서라고 하여 이 문서를 정독했다.

모든 pom.xml 파일이 기본적으로 상속하고있는 POM을 **Super POM** 이라고 한다. 링크에는 2.1.x 버전의 Super POM 까지만 나와있다. 최소 커맨드로 생성한 프로젝트의 pom.xml이 3.3.9 버전의 Super POM 일까?

## Minimal POM
```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
</project>
```
여기에 적힌 내용으로 project의 **fully qualified artifact name** 을 결정한다. 위의 경우 "com.mycompany.app:my-app:1" 로 결정된다.

포함되지 않은 내용들은 Super POM 의 값을 상속받는다. 예를 들어 **packaging type** 이 적혀있지 않으므로 기본값인 **jar** 로 설정된다.
## Project Inheritance
```
<project>
  <parent>
    <groupId>com.mycompany.app</groupId>
    <artifactId>my-app</artifactId>
    <version>1</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-module</artifactId>
  <version>1</version>
</project>
```
parent POM 을 설정하려면 parent section 을 추가하면 된다. fully qualified artifact name 을 정할 수 있는 값들을 명시하면 된다. groudId 혹은 version 의 경우 parent 와 같은 값을 가지는 경우가 있으므로 parent section 에만 적고, 밑의 section 에서는 생략할 수 있다.

위의 기술은 parent project 가 이미 local repository 에 존재하거나, 정확히 한 계층 위의 디렉토리에 parent POM 이 존재하는 경우에 유효하다. 둘 다 아닌 경우에는 parent POM 의 경로를 명시해줘야 한다.

```shell
# 계층 구조가 아래와 같은 경우
# .
# |-- my-module
# |   `-- pom.xml
# `-- parent
#     `-- pom.xml
...
  <parent>
  ...
  <relativePath>../parent/pom.xml</relativePath>
  </parent>
...
```
## Project Aggregation
반대로 parent POM 에서 child POM 을 기술할 수도 있다. child POM 에서 parent POM 의 설정값(?)을 가져다 쓰는 경우에 inheritance 를 하고, parent project 을 실행할 때, child project 도 연계시키는 경우 aggregation 을 쓰는 듯 하다.

두 가지만 신경쓰면 된다. parent POM 의 **packaging type** 을 "pom" 으로 변경하고, child POM 이 어디에 위치하는지 (마찬가지로) 상대경로를 기술한다.

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
  <packaging>pom</packaging>
 
  <modules>
    <module>../my-module</module>
  </modules>
</project>
```
## Three Rules
* Specify in every child POM who their parent POM is.
* Change the parent POMs packaging to the value "pom" .
* Specify in the parent POM the directories of its modules (children POMs)

## Project Interpolation and Variables
세 가지 유형의 변수를 접근하고 사용할 수 있다.
### project model variables
project model 의 단일 값을 지니는 모든 값을 변수로 활용할 수 있다. 
* `${project.groudId}` 
* `${project.version}` 
* `${project.build.sourceDirectory}`

### special variables
딱 3개 있다. timestamp 의 경우 properties 에 `maven.build.timestamp.format` 을 지정하면 포맷을 변경할 수 있다.
* `${project.basedir}` : The directory that the current project resides in.
* `${project.baseUri}` : The directory that the current project resides in, represented as an URI.
* `${maven.build.timestamp}` : The timestamp that denotes the start of the build.

### properties
그냥 이름-값 으로 자유롭게 지정할 수도 있다.
```shell
<project>
  ...
  <properties>
    <mavenVersion>2.1</mavenVersion>
  </properties>
  <dependencies>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-artifact</artifactId>
      <version>${mavenVersion}</version>
    </dependency>

  </dependencies>
  ...
</project>
```