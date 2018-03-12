# `sbt console`

brew 로 개발 머신에 scala 를 설치했다. clone 받은 scala project 을 sbt 로 build 해서 jar 파일을 만들었다. build 한 jar 파일을 `-cp` 옵션으로 주면서 scala REPL 을 실행했다. 그리고 원하는 코드를 import 하니 `NoClassDefFoundError` 가 발생한다. brew 로 설치한 scala version 과 scala project 의 `scalaVersion` 이 불일치하기 때문이었다. scala version 은 `util.Properties.versionString` 으로 확인할 수 있다.

어떻게 해결해야할지 고민이었는데, scala REPL 대신 `sbt console` 을 실행하면 문제가 해결된다. sbt 가 scala project 의 `scalaVersion` 에 맞는 scala REPL 을 실행해준다.

[sbt 홈페이지의 Command Line Reference 문서](https://www.scala-sbt.org/1.x/docs/Command-Line-Reference.html) 에는 아래처럼 적혀있다.

> **console** Starts the Scala interpreter with a classpath including the compiled sources, all jars in the lib directory, and managed libraries. To return to sbt, type :quit, Ctrl+D (Unix), or Ctrl+Z (Windows). Similarly, test:console starts the interpreter with the test classes and classpath.
