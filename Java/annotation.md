http://www.nextree.co.kr/p5864/
http://kang594.blog.me/39704853

## Basic Concepts
annotation 은 비즈니스 로직에는 영향을 주지 않는다. 하지만 해당 타겟의 연결 방법이나 소스코드의 구조를 변경할 수 있는 유용한 도구이다.
### annotation of annotation
* `@Target` 은 annotation 의 타겟을 지정. (예: ElementType.FIELD)
* `@Retention` 은 annotation 의 지속기간. (예: RetentionPolicy.RUNTIME)

annotation 을 "제품에 붙이는 라벨" 로 이해하면 좋다고 한다. 예를 들어 사과에 `@CanSale` 라벨이 붙어있다면 **포장하는 시점**에 이 라벨이 붙은 사과들만 골라서 포장할 수 있다. 이때 얘기하는 시점이 `@Rentention` 과 연관이 있을 것이다.
## 특정 Annotation 달려있는지 여부 확인
annotation 이 reflection 과 연관되는 부분은 어떤 타겟에 해당 annotation 이 붙어있는지 확인하는 과정에서다. `Class` 나 `Method` 타입을 활용할 수 있다.
```java
Class targetClass = Class.forName("타겟클래스이름");

for (Method m : targetClass.getDeclaredMethods()) {
	if (m.isAnnotationPresent(Annotation.class) {
		...
	}
}
```
## Annotation 의 필드값 획득
```java
public class AnnotationStudy {
    public static void main(String[] args) throws ClassNotFoundException {
        Class one = Class.forName("AnnotationStudy$AnnotatedClassOne");
        Class two = Class.forName("AnnotationStudy$AnnotatedClassTwo");

        // 구체적인 Annotation 타입으로 캐스팅해주지 않으면, 필드에 대한 getter 를 호출할 수 없다.
        MyAnnotation forOne = (MyAnnotation) one.getAnnotation(MyAnnotation.class);
        MyAnnotation forTwo = (MyAnnotation) two.getAnnotation(MyAnnotation.class);

        System.out.println("MyAnnotation #1: desc = " + forOne.desc());
        System.out.println("MyAnnotation #2: desc = " + forTwo.desc());
    }

    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    @interface MyAnnotation {
        String desc();
    }

    @MyAnnotation(desc = "one")
    public static class AnnotatedClassOne {}
    @MyAnnotation(desc = "two")
    public static class AnnotatedClassTwo {}
}
```
## 구현해볼만한 예제
* Django 의 `login_required` 같은 데코레이터를 **Java annotation** 으로 구현