# How to Kill a Java Thread

어떤 class 를 작성할때의 얘기다. class 가 정상적으로 기능하기 위해서 주기적으로 어떤 작업을 실행할 필요가 있었다. main thread 를 block 하면 안되므로, background thread 에서 작업을 실행시키려고 했다. 

* 주기적으로 실행할 작업을 (`Thread#sleep` 으로 주기를 맞춤) `Runnable` 로 기술
* class 가 initialize 될때 `Runnable` 로 `Thread` 를 만들고 start
* class 가 destroy 될때(?) `Runnable` 을 중단하고 `Thread` 의 resource(?) 를 반납 

이때 마지막 **"resource 반납"** 이 정확히 무엇이 될지 의아했다. 구글링하다가 [How to Kill a Java Thread](http://www.baeldung.com/java-thread-stop) 를 읽었고, 충분한 내용이라 생각해서 적용했다.

* `Runnable` 의 while loop 이 `AtomicBoolean` 로 check 하게 수정
* `AtomicBoolean` 을 false 로 바꿔서 `Runnable` 중단
* `Thread#sleep` 도중에 중단할 때 sleep 이 끝날때까지 기다리기 싫으므로 `Thread#interrupt`
* while loop 을 빠져나와 `Runnable` 이 끝나서 `Thread` 가 없어질때까지`Thread#join` 으로 block