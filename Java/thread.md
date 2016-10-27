프로그램이 컴퓨터에서 실행중일때 이를 프로세스라고 부른다. 기초적인 프로그래밍을 배울때는 프로세스는 하나의 실행 흐름을 가진다. Java 의 경우라면 main 메소드의 시작과 함께 프로세스가 시작되고, main 메소드를 한줄 한줄 실행하다가 끝나면 프로세스도 함께 끝난다. 

그러나 프로세스는 여러개의 실행 흐름을 가질 수 있다. 이때 각각의 실행 흐름을 **Thread** 라고 한다. 사실 main 메소드도 하나의 실행 흐름이므로, Thread 이다. main 메소드가 곧 main Thread 인 것이다. 프로세스가 여러개의 Thread 로 구성된다면, 모든 Thread 들이 종료되어야 비로소 프로세스가 종료된다. 

프로세스가 JVM 위에서 실행될때 메모리를 지니고, 그 메모리는 크게 3개의 영역(메소드, 스택, 힙)으로 나눠져있다. 그러므로 하나의 프로세스에 속하는 여러개의 Thread 들은 프로세스의 메모리를 일부분 공유하면서, 또한 각자의 고유한 메모리를 가진다. 그러므로 Thread 의 실행을 위해서는 이러한 메모리 배정을 비롯한 사전/사후 작업들이 필요하다. 그러므로 Thread 인스턴스의 run 메소드를 직접 호출하는 것이 아니라, start 메소드를 호출하는 것이다.
## Thread 클래스를 정의하는 두 가지 방법
첫번째 방법은 Thread 클래스를 상속받는 것이다. Thread 클래스에 정의된 run 메소드를 오버라이딩하면 된다. 직관적이지만, Java 는 다중 상속을 지원하지 않으므로 이미 다른 클래스를 상속하는 클래스의 경우 문제가 된다.
```java
public class ExtendThread extends Thread {
	...
	@Overrides
	public void run() {
		// 실행될 로직
	}
	...
}
```
두번째 방법은 Runnable 인터페이스를 구현하는 것이다. Java 에서 다중 상속의 문제를 맞닿뜨렸다면, 가장 먼저 시도해봐야 할 방법은 인터페이스의 구현으로 대신하는 것이다. Runnable 인터페이스를 구현하고, 해당 클래스의 새로운 인스턴스를 생성하고, (Runnable 인터페이스의 인스턴스를 인자로 받는) Thread 클래스의 생성자를 사용할 수 있다.
```java
public class ImplThread extends SomeClass implements Runnable {
	...
	@Overrides
	public void run() {
		// 실행될 로직
	}
	...
}
...
...
ImplThread it = new ImplThread();
Thread tr = new Thread(it);
tr.start();
``` 
## 다른 Thread 의 종료를 기다리는 방법
Thread 들은 별도의 실행 흐름을 가지므로, 독립적으로 실행된다. 그러나 어떤 경우에는 Thread A 가 다른 Thread B 의 종료를 기다릴 필요가 있을 것이다. 이 경우 A 가 B 의 참조변수를 알고 있다면, A 의 실행 흐름에서 `B.join()` 을 호출하면 된다.
## Thread 의 Priority 와 Scheduling
Thread 들은 별도의 실행 흐름으로 독립적으로 실행되지만, 실제 CPU 가 하나인 경우(하나여도 멀티 코어라면 얘기가 좀 달라지겠지만)라면 실질적으로 특정 시점에 실행중인 Thread 는 하나일 수 밖에 없다. 그러므로 JVM 에는 Thread 들에게 CPU 할당을 지정해주는 Scheduler 가 존재한다. Scheduler 의 스케쥴링 기본 원칙은 2가지다.
* 우선순위가 높은 Thread 에게 CPU 를 배정한다.
* 우선순위가 동일한 Thread 들에 대해서는 시분할로 배정한다.

Java 의 Thread 는 10단계의 priority 를 제공한다. 하지만 Thread 의 실행은 OS 의존적인 면이 크므로, OS 가 덜 세분화된 priority 를 제공한다면 10단계는 실질적으로 10단계가 아니게 된다. 그러므로 보편적인 경우에는 Thread 의 정적 변수로 제공되는 3가지 priority 중에서 골라 사용하는것이 좋다.
* MAX_PRIORITY
* NORM_PRIORITY
* MIN_PRIORITY

여기까지 보면 priority 가 높은 Thread 들이 전부 종료되고 나서야 그 다음 Thread 가 실행될 것 같지만, 실제로는 꼭 그렇지 않다. 파일 입출력 / 네트워킹 같은 CPU 자원이 필요치 않은 실행 시점에는 Thread 는 자신에게 배정된 CPU 자원을 반납한다. 그러므로 높은 priority 의 Thread 와 낮은 priority 의 Thread 가 실행 흐름을 나눠 가질 수 있다.
## Thread 의 Life Cycle
[Java API 문서](https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.State.html#NEW) 의 Thread.Safe Enum 설명을 보면 총 6개의 상태가 존재한다. 책에서는 4가지만 얘기하고 있지만, 느낌을 받기에는 충분하다.
* **NEW:** Thread 인스턴스가 생성된 시점
* **RUNNABLE:** run 메소드가 호출된 이후. 실질적인 실행 흐름이 진행 중이다.
* **BLOCKED:** CPU 자원이 필요없는 작업을 실행 중이거나, join/sleep 메소드를 호출한 상황이다. 다시 RUNNABLE 이 될 수 있다.
* **DEAD:** 실행 흐름이 종료된 상태. RUNNABLE 로 돌아갈 수 없다.

## Thread 의 메모리 구성
Java 의 경우 어찌되었던 main Thread 는 main 메소드이다. 그러므로 main Thread 를 기준으로 메모리 구성을 생각해볼 수 있다. 간단하다. 메소드 영역 / 힙 영역은 다른 Thread 들도 main Thread 의 것을 공유한다. 그러므로 참조 변수를 알 수 있다면 다른 Thread 에서 같은 인스턴스에 접근할 수 있다. 정보 교환 측면에서 유익하다. 그러나 별도의 실행 흐름을 가지므로 스택 영역은 독립적으로 지닌다. 실행 흐름이라는 것이 결국 여러 메소드 호출의 연속이기 때문이다.