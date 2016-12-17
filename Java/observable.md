예전에 TIL 에 Design Pattern 항목으로 **observer pattern** 을 정리했었다. 그때도 언급했던 Java 의 Observable / Observer API 를 간단히 사용해보고 싶어서 정리해본다.

한 가지 해맸던 부분은, Observable 이 **concrete class** 로 정의되어있길래 바로 사용할 수 있을 줄 알고 새로운 인스턴스를 만들고, notifyObservers 메서드를 호출했는데 아무 반응이 없었던 것. .java 파일에 들어가서 메서드의 주석을 읽어보니 해당 인스턴스가 **hasChanged** 여야 등록된 Observer 들에게 알림을 주는 로직이었다. 메서드의 이름만 보고 지레짐작 하지 말아야겠다는 교훈을 얻었다.

Observable 의 setChanged 메서드는 접근자가 protected 이므로 결국 Observer 를 상속해서 setChanged 메서드를 호출해주는 wrapper 메서드의 정의가 필요했다.

```java
// setChanged 의 접근자가 protected 라서 wrapping
public class ObservableImpl extends Observable {
    public void setChanged() {
        System.out.println("observable has changed");
        super.setChanged();
    }
}

// Observer 는 interface 라서 구현해줘야한다.
public class ObserverImpl implements Observer{
    @Override
    public void update(Observable o, Object arg) {
        System.out.println(Thread.currentThread().getName() + " updated!");
    }
}

// 메인 코드
ObservableImpl observable = new ObservableImpl();

int numOfObservers = 5;
for (int i = 0; i < numOfObservers; i++) {
    observable.addObserver(new ObserverImpl());
}

observable.setChanged();
observable.notifyObservers();
Thread.sleep(1000);
```
```shell
# 콘솔 출력
observable has changed
main updated!
main updated!
main updated!
main updated!
main updated!
```

Observable 의 notifyObservers 메서드를 호출했을 때, Observer 의 **update 메서드가 어느 thread 에서 호출**되는지 궁금해서 확인을 위한 코드를 끼워넣었다. 콘솔 출력을 통해 확인해보니 별도의 설정이 없다면 Observer 의 update 메서드도 main thread 에서 실행됨을 확인할 수 있었다. 