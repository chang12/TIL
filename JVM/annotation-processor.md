**Hannes Dorfmann** 이 작성한 [ANNOTATION PROCESSING 101](http://hannesdorfmann.com/annotation-processing/annotationprocessing101) 이라는 블로그 포스트를 [번역한 Jason Kim 의 블로그 포스트를](https://medium.com/@jason_kim/annotation-processing-101-%EB%B2%88%EC%97%AD-be333c7b913) 읽었다. Java 의 annotation processing 에 대한 이해도를 높일 수 있었던 좋은 글이었다. ([저자의 GitHub](https://github.com/sockeqwe) 을 들어가보니 Android 에 상당한 내공이 있는 사람인것 같다) 유익했던 포인트 몇 가지를 정리해보자.

### annotation processor 의 처리 대상

`AbstractProcessor` 의 `process` 메서드를 구현하면서 처리하는 `Element` 인스턴스들은 두 가지 유형인데, 이미 컴파일된 class 파일에 속한걸수도 있고, 컴파일 전의 java 파일에 속한걸 수 있다. 전자는 JAR 파일로 classpath 에 포함시킨 라이브러리의 소스 코드에서 해당 annotation 을 사용한 경우가 될 것이다.

### Code Generation

annotation processing 의 궁극적인 목표는 code generation 이다. [Square 가 만든 JavaPoet 라이브러리가](https://github.com/square/javapoet) 이 맥락에서 좋은 도움이 될 것이다.

### Round

annotation processing 은 여러 round 에 걸쳐 이뤄지는데, `AbstractProcessor` 구현체의 생성자는 **최초 1번만 호출되고**, 이후 매 라운드마다 `process` 메서드가 호출된다. 그러므로 이전 `process` 메서드에서 조작된 `AbstractProcessor` 구현체의 필드들이 이후 `process` 메서드가 호출되는 시점에 그대로 남아있게 된다. 이로 인해 이후 round 의 `process` 메서드에서 동일한 code generation 을 반복하려고 시도할 수 있다. 이를 방지하기 위해서 `process` 메서드가 정상적으로 완료되고나면 적절한 clearing 을 해줘야 할 것이다.
