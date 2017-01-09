어떤 `Component` 인터페이스가 존재한다. 여기에 **Decorator Pattern** 을 적용해볼 것이다.

```java
public interface Component {
    public void operation1();
    public void operation2();
}
```

`Decorator` 는 `Component` 를 구현함과 동시에, `Component` 타입의 필드를 지니는 추상 클래스이다. 실질적인 동작은 생성자에서 주입받은 `Component` 타입 필드의 메서드를 호출하는 것(delegation) 뿐이다. `Decorator` 클래스는 자신을 상속할 concrete class 들을 위한 틀의 역할을 한다. (간단히 실험해보니, 추상 메서드가 없는 추상 클래스도 인스턴스를 만들수는 없다. 아무 메서드라도 하나 오버라이딩하면서 익명 클래스의 형태로 인스턴스를 만들어줘야 한다.)
  
```java
abstract class Decorator implements Component {
    protected Component component;
    
    public Decorator(Component component) {
    	this.component = component;
    }
    
    ReturnType operation1(int num) {
    	component.operation1(num); // Delegation
    }
    ReturnType operation2(double num) {
    	component.operation2(num); // Delegation
    }
}
```

`Decorator` 를 상속하는 concrete class 들은 자신이 원하는, 기능을 추가하고 싶은 `Component` 의 메서드를 오버라이딩하여 원하는 바를 성취할 수 있다.

```java
class ExtendedDecorator extends Decorator {
    @Override
    ReturnType operation1(int num) {
    	beforeOperation1(...);
    	super.operation1(num);
        afterOperation1(...);
    }
}
```
