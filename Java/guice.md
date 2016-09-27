* [Guice GitHub repository](https://github.com/google/guice)
* [Guice User Guide](https://github.com/google/guice/wiki/Motivation)


프로그래밍 패턴중에 **의존성 주입(dependency injection)** 이라는 패턴이 있다고 한다. 어떤 클래스의 정의가 인터페이스(interface)나 추상 클래스(abstract class)에 의존성을 지닐 경우(필드의 타입이 인터페이스라던가), 그 클래스의 완벽한 정의를 위해서는 의존하는 인터페이스나 추상 클래스의 구현체가 필요할 것이다. 이 구현체를 주입한다는 점에서 의존성 주입이라는 이름이 매겨지지 않았나 생각해봤다. 

**Guice**는 이 의존성 패턴을 편리하게 사용할 수 있도록 도와주는 (무려 Google이 만든) Java 라이브러리이다. Guice 의 User Guide 문서에서 제시한 예를 간단히 요약해보자. 

아래의 **BillingService** 클래스는 주문을 처리해서 결과를 담은 Receipt를 반환하는 메소드를 포함하고 있다. 그리고 그 메소드의 구현에 **CreditCardProcessor** 인터페이스와 **TransactionLog** 인터페이스에 의존성을 지닌다고 가정하자.

```java
public class BillingService {
	public Receipt chargeOrder() {
		// CreditCardProcessor 인터페이스의 구현체가 필요
		// TransactionLog 인터페이스의 구현체가 필요
		...
	}
}
```
첫번째 방법은 의존성이 필요한 메소드 내부에서 **직접 생성자를 호출**하여 해소하는 것이다. 의존성의 해소가 고정되어버리므로 배포환경 / 테스트환경의 구분은 물론이고, BillingService 클래스의 재사용성이 매우 떨어진다. 다른 종류의 의존성 구현이 필요할때마다 BillingService 와 아주 유사한 클래스를 재정의해야 할 것이다. 나쁜 방법이다.
```java
public class BillingService {
	public Receipt chargeOrder() {
		// PayPal을 안쓴다면...?
		CreditCardProcessor processor = new PayPalCreditCardProcessor();
		// 로그를 HDD에 저장해야한다면...?
		TransactionLog transactionLog = new InMemoryTransactionLog();
		...
	}
}
```
두번째 방법은 **팩토리 패턴**을 이용한 방법이다. 팩토리 패턴에 대해서 처음 들어보지만, 예시 코드를 보고 싱글톤 패턴+외부에서 설정 가능한 패턴정도로 이해했다. CreditCardProcessor와 TransactionLog 각각에 대한 팩토리 클래스를 우선 정의한다. 동일한 패턴이니 CreditCardProcessor의 경우만 적는다. 

```java
public class CreditCardProcessorFactory {
	public static CreditCardProcessor processor;

	public static void setInstance(CreditCardProcessor processor) {
		this.processor = processor;
	}

	public static CreditCardProcessor getInstance() {
		if (processor == null) {
			// 기본값으로 VISA 카드를 쓴다고 가정!
			this.processor = new VisaCredirCardProcessor();
		}
		return processor;
	}
}
```
BillingService 에서는 직접 인스턴스를 생성하지 않고, 팩토리에게서 얻어온다.
```java
public class BillingService {
	public Receipt chargeOrder() {
		CreditCardProcessor processor = CreditCardProcessorFactory.getInstance();
		TransactionLog transactionLog = TransactionLogFactory.getInstance();
		...
	}
}
```
클래스의 의존성을 팩토리에게 위임했다는 점에서 발전했다. 하지만 이 방법을 쓰기 위해서는, BillingService 클래스를 사용하는 측에서 아래와 같은 세팅과 해제 과정을 거쳐야할 것이다. 이는 상당히 번거롭다. 또한 어떤 예외 발생에 의해서 해제 과정이 수행되지 않았을 경우에, 이는 다음번 BillingService 사용에 영향을 끼칠 수 있다.
```java
// 설정
CreditCardProcessorFactory.setInstance(new PayPalCreditCardProcessor());
TransactionLogFactory.setInstance(new InMemoryTransactionLog());
...
...
// 해제
CreditCardProcessorFactory.setInstance(null);
TransactionLogFactory.setInstance(null);
```
세번째 방법은 **의존성 주입 패턴을 직접 적용**해서 코드를 짜는 것이다. BillingService 는 해소해야할 의존성을 생성자를 통해 받아온다. 그러므로 의존성의 해소를 BillingService가 아니라 BillingService를 사용하는 클라이언트 쪽에 위임하는 셈이다. 이전의 두 패턴의 단점이 모두 해소된다.
```java
public class BillingService {
	private final CreditCardProcessor processor;
	private final TransactionLog transactioLog;

	public BillingService(CreditCardProcessor processor, TransactionLog transactionLog) {
		this.processor = processor;
		this.transactionLog = transactionLog;
	}

	public Receipt chargeOrder() {
		// 주입받은 인스턴스 변수를 사용하면 된다!
	}
}
```
세번째 방법의 문제점은 딱히 없어보였다. 그래서 Guice를 쓰는 의의가 무엇일까 생각해보다가, 저런 의존성 관계가 단순히 한번의 주입이 아니라, 수많은 클래스들이 서로 수많은 의존성으로 관계가 맺어져있다면 힘들겠다 싶었다. Guice 튜토리얼에서도 **Object Graph** 라는 용어로 이를 표현했다. 그러므로 한 문장으로 정리해서 Guice 란 Object Graph 를 편리하게 생성해주는 도구가 아닐까 생각했다.

간단한 예시로 100단계로 구성된 의존성을 생각해보자. 동일한 의존성으로 인스턴스를 2개 만든다고 가정했을 때 직접 의존성 주입 패턴으로 프로그래밍 한다면 아래와 같을 것이다.
```java
// 첫번재 인스턴스 생성
Interface1 objA1 = new Interface1Impl();
Interface2 objA2 = new Interface2Impl(objA1);
...
Interface100 objA100 = new Interface100Impl(objA99);
// 두번째 인스턴스 생성
Interface1 objB1 = new Interface1Impl();
Interface2 objB2 = new Interface2Impl(objB1);
...
Interface100 objB100 = new Interface100Impl(objB99);
```
동일한 구조의 의존성이지만, 새로운 인스턴스를 만들때마다 의존성 주입에 대한 행사코드를 매번 똑같이 작성해줘야 한다. Guice의 가장 기본적인 사용 형태인 **Module** 정의 + **Injector** 생성 + 인스턴스 생성의 흐름을 따르면 동일한 코드의 반복을 피할 수 있다.
```java
// Module 클래스 상속
// Linked Bingding 사용
public HundredLayersModule extends AbstractModule {
	@Override
	protected void config() {
		bind(Interface1.class).to(Interface1Impl.class);
		bind(Interface2.class).to(Interface2Impl.class);
		...
		bind(Interface100.class).to(Interface100Impl.class);
	}
} 
```
```java
...
Injector injector = Guice.createInjector(new HundredLayersModule);
Interface100 objA = injector.getInstance(Interface100.class);
Interface100 objB = injector.getInstance(Interface100.class);
``` 
AbstractModule을 상속받아 **config 메소드를 오버라이딩**하면서 binding 을 서술하는 부분은 길어질 수 있지만, 한번 정의한 Module로 부터 Injector 인스턴스를 만들고, Injector 인스턴스로부터 최종적으로 원하던 Interface100 인스턴스를 만드는 코드는 간단해진다.