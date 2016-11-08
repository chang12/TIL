하나의 프로세스에서 파생된 **Thread** 들은 힙 영역을 공유한다고 했었다. 이는 편리하기도 하지만, 문제가 될 수도 있다. 하나의 인스턴스에 동시에 여러개의 Thread 가 접근해서 변화를 가할 경우 예상과 다른 변화가 나타날 수 있기 때문이다. 그러므로 **동기화(synchronization)**가 필요하다.

동기화란 무엇인가? 우선은 간단하게 이해했다. 동기화란, **"공유되는 자원에 동시에 둘 이상의 Thread 가 접근할 수 없도록 막는 장치"** 이다.

## 동기화 메소드
메소드 선언에 **synchronized** 키워드를 사용해서 동기화 메소드임을 명시할 수 있다.
```java
public synchronized void increment() {
	num++;
} 
```
synchronized 키워드는 메소드 선언에 사용되지만, 동기화의 단위는 해당 메소드가 속한 **인스턴스**라는 점에 유의하자. 어떤 인스턴스의 동기화된 메소드에는 둘 이상의 Thread 가 접근할 수 없다. 하지만 동기화되지 않은 메소드에는 접근이 가능하다. 

동기화된 메소드에 **자물쇠**가 채워져 있다고 생각한다면, 자물쇠를 열 수 있는 **열쇠**를 인스턴스가 하나만 가지고 있는 셈이다. OS 에서 많이 쓰이는 개념이며, 이를 **lock** 또는 **monitor** 라고 한다. Java 의 모든 Object 인스턴스들은 열쇠를 하나씩 가진다. 이 개념은 일반적인 개념이므로 이후 내용에서도 필요한 개념이다.
## 동기화 블록
어떤 경우에는 동기화 메소드를 선언하는 것만으로는 적절한 동기화를 선언하기에 부족할 수 있다. 아래와 같은 경우를 생각해보자.
```java
public class HasTwoNums {	
	int num1;
	int num2;

	public synchronized void increaseNumOne {
		num1++;
	}
	public synchronized void increaseNumTwo {
		num2++;
	}
}
```
`increaseNumOne` 메소드를 여러 Thread 가 호출하거나, `increaseNumTwo` 메소드를 여러 Thread 가 호출하는 것을 막기 위해서 메소드에 동기화가 되어있다. 하지만 어느 두 Thread 가 하나는 전자를, 다른 하나는 후자를 호출하는 것에는 문제가 없다. 하지만 lock 을 공유하므로 이러한 경우도 동기화에 의해서 제한된다. 이는 동기화가 지나치게 적용되어, 성능을 저하시키는 경우이다.

이러한 경우에 **동기화 블록**을 사용할 수 있다. 동기화 블록을 선언할때는 어떤 열쇠를 사용할지 지정할 수 있다. 앞서 Java 의 모든 Object 가 열쇠를 가진다고 했으므로, 열쇠를 2개 만들어서 나눠주면 된다.
```java
public class HasTwoNums {
	int num1;
	int num2;

	// 열쇠를 두 개 생성해서 나눠준다.
	Object key1 = new Object();
	Object key2 = new Object();

	public void increaseNumOne {
		synchronized(key1) {
			num1++;
		}
	}
	public void increaseNumTwo {
		synchronized(key2) {
			num2++;
		}
	}
}
``` 