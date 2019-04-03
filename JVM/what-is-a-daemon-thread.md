gRPC 홈페이지의 [gRPC ❤ Kotlin](https://grpc.io/blog/kotlin-gradle-projects) 블로그 글의 [example 코드를](https://github.com/grpc/grpc-java/blob/60a0b0c471d720b0546ee3a5b4fa4283635dfbcf/examples/src/main/java/io/grpc/examples/helloworld/HelloWorldServer.java#L58-L60) 읽다가, `Await termination on the main thread since the grpc library uses daemon threads.` 라는 주석을 읽었다. `daemon thread` 라는게 뭘까?

## kotlin.concurrent.Thread.kt 의 thread 함수의 주석

> @param isDaemon if `true`, the thread is created as a daemon thread. The Java Virtual Machine exits when the only threads running are all daemon threads.

main thread 의 코드가 모두 실행되었을 때 실행 중인 (`실행 중` 이라는건 어떻게 정의될까?) thread 가 남아있을 때, daemon thread 가 아닌 thread 가 하나라도 실행 중이면 끝날 때 까지 기다리고, 모두 daemon thread 라면 이를 무시하고 jvm 을 종료한다는 뜻인가 보다.

## 실험

kotlin 에서 제공해주는 `thread` 함수를 활용해서 main 함수를 작성하고 `isDaemon` 값을 달리해서 실행해본다. [Passing a lambda to the last parameter](https://kotlinlang.org/docs/reference/lambdas.html#passing-a-lambda-to-the-last-parameter) 문서에 적혀있듯이, function 의 마지막 parameter 가 function 인 경우 {...} 로 분리할 수 있어서 보기에 좋다.

```kotlin
package kr.fakenerd.grpc.kotlin.example

import java.time.ZoneId
import java.time.ZonedDateTime
import kotlin.concurrent.thread

fun main(args: Array<String>) {
//    thread(start = true, isDaemon = false) {
    thread(start = true, isDaemon = true) {
        while (true) {
            Thread.sleep(1000L)
            println(ZonedDateTime.now(ZoneId.of("Asia/Seoul")))
        }
    }
    println("main finished.")
}

```

`isDaemon = true` 인 경우에는 process 가 바로 종료된다.

```
main finished.

Process finished with exit code 0
```

`isDaemon = false` 인 경우에는 main thread 가 종료됬을 지라도, process 가 종료되지 않고 thread 의 코드가 계속 실행된다.

```
main finished.
2019-04-04T01:30:01.337+09:00[Asia/Seoul]
2019-04-04T01:30:02.340+09:00[Asia/Seoul]
2019-04-04T01:30:03.342+09:00[Asia/Seoul]
2019-04-04T01:30:04.347+09:00[Asia/Seoul]
...
```
