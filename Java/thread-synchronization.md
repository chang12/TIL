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
## 실행순서 동기화
앞서 정리한 동기화 메소드, 동기화 블록은 공유되는 자원에 여러 Thread 가 동시에 접근하는 것을 막아서 실행에 문제가 없도록 했다. 단순히 동시 접근을 막는것 뿐 아니라, 접근의 순서를 제어해야하는 경우도 있을 것이다.

`Object` 클래스의 **wait** / **notifyAll** / **notify** 메소드를 활용할 수 있다. 세 메소드가 하나의 `Object` 인스턴스에 대한 메소드임을 주목하자. 따라서 세 메소드는 열쇠를 공유한다. 이를 명시하기 위해서 세 메소드를 호출하는 코드는 필수적으로 동기화 메소드 or 동기화 블록으로 동기화되어야 한다.

* **wait:** notifyAll / notify 메소드에 의해 깨워질때까지 대기한다.
* **notifyAll / notify:** wait 를 호출하고 대기중인 Thread 들을 모두 / 하나만 깨운다.

이름이 직관적이므로 예제 코드는 싣지 않는다.
## ReentrantLock
JDK 5 부터 지원하는 클래스로, 동기화 메소드 / 동기화 블록을 대체할 수 있다고 한다.

### Basic Usage
동기화 블록에서 열쇠로 사용할 Object 를 생성하듯이, `ReentrantLock` 인스턴스를 기본 생성자로 만들 수 있다. 차이점은 명시적으로 lock / unlock 메소드를 호출해서 여러 Thread 의 동시접근을 막는다는 점이다.  
```java
class X {
	private final ReentrantLock lock = new ReentrantLock();
	...
	public void m() {
		lock.lock(); // block until condition holds
		try {
			// something
		} finally {
			lock.unlock();
		}
	}
	...
}
```
lock 메소드를 호출한 시점부터 unlock 메소드를 호출하기 전까지의 코드는 동시에 하나의 Thread 만 실행된다. 그러므로 unlock 메소드가 필히 호출되도록 위 코드처럼 finally 구문안에 배치하는 것이 일반적이다.
### 실행순서 동기화
`ReentrantLock` 에서도 마찬가지로 실행순서의 제어가 가능해야한다. 하나의 `ReentrantLock` 인스턴스에서 여러개의 `Condition` 인스턴스를 만들 수 있고, `Condition` 인스턴스에는 앞에서 다룬 wait / notify / notifyAll 에 대응되는 세 메소드를 호출할 수 있다. **await** / **signal** / **signalAll** 들이 그것이다.
```java
// 여러개의 Condition 을 만들 수 있다.
ReentrantLock lock = new ReentrantLock();
Condition readCond = lock.newCondition();
Condition writeCond = lock.newCondition();
...
```
wait / notify / notifyAll 메소드들이 동기화 메소드 / 동기화 블록 내부에서만 실행될 수 있었듯이, await / signal / signalAll 메소드들도 자신을 생성한 `ReentrantLock` 인스턴스의 **lock / unlock 호출 사이**에서만 호출될 수 있다.