회사에서 **RxJava** 가 쓰이기 시작하여 이에 대해 알고 싶었다. RxJava 의 Rx 가 reactive extension 임을 알게 되었고, reactive = observer 패턴 + iterator 패턴 + 함수형 패러다임의 결합이라는 소개를 봤다. 그래서 observer 패턴에 대해 알아보기 시작했다.
## Observer Pattern
[한글 위키피디아](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4)를 보니 내용은 간단했다. **Subject** 와 **Observer** 의 두 주체간의 얘기였다. 

**Subject** 는 구독을 원하는 Observer 들이 자신을 등록/해제할 수 있는 인터페이스를 제공한다. 이를 통해서 Subject 는 자신을 구독하는 Observer 인스턴스들의 리스트를 가지게 된다.

**Observer** 는 자신이 구독하는 Subject 가 발행되었을 때, 실행되기를 원하는 로직을 시작시키는 인터페이스를 제공한다. 이는 자신이 구독하는 Subject 가 발행하면서 호출해준다.
굉장히 많이 요구되는 패턴이기 때문에 그 사용성이 매우 크겠다는 생각을 했다.
## java.util.Observable/Observer
Java 에서는 이러한 observer pattern 을 편리하게 사용할 수 있도록, **java.util** 패키지에 **Observable** 과 **Observer** 클래스를 제공해주고 있었다. Observable 은 concrete class 치고, Observer 는 interface 였다. 각각의 메서드 선언이 위에서 기술한 observer pattern 의 내용을 그대로 표현하고 있었다. 메서드의 이름과 시그니처를 보면 어떤 내용일지 거의 짐작할 수 있다.
```java
class Observable {
	void addObserver(Observer o) {...}
	protected void clearChanged() {...}
	int countObservers() {...}
	void deleteObserver(Observer o) {...}
	void deleteObservers() {...}
	boolean hasChanged() {...}
	void notifyObservers() {...}
	void notifyObservers(Object arg) {...}
	protected void setChanged() {...}
}

interface Observer {
	void update(Observable o, Object arg);
}
```
(더 자세히 알아봐야하겠지만) Observable 이 notifyObservers 메서드를 호출해서 등록된 모든 Observer 들의 update 메서드가 실행될 때, **어느 thread 에서 실행**될지가 궁금하다. 가장 단순하게는 모든 호출이 동기적으로 이뤄져서 등록된 순서대로 Observer 들의 update 메서드가 순차적으로 실행되는 것이다. 그러나 Java 에서 thread 들 간의 인스턴스 공유는 편리하게 이뤄지므로 여러 thread 가 서로 다른 Observer 들을 생성해서 하나의 Observable 을 구독하게 되는 경우에 그런 순차적인 실행이 될 것 같지는 않았다. 아마 이와 관련해서는 기본 Observable/Observer 의 사용성을 더 증진시켜주는 유틸리티들이 존재할 것이다. 