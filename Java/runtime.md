`RunTime` 이라는 클래스가 있다. 어떻게 해야 제대로 사용하는건지 아직 모르겠지만, 재밌게 생겼다.

```java
Runtime runtime = Runtime.getRuntime();

System.out.println("Available Processors: " + runtime.availableProcessors());
System.out.println("Total Memory(MB): " + runtime.totalMemory() / (1024L * 1024L)); 
System.out.println("Free Memory(MB): " + runtime.freeMemory() / (1024L * 1024L));

// Process process = runtime.exec("java RuntimeMain");
```
```shell
Available Processors: 4
Total Memory(MB): 119
Free Memory(MB): 116
```

오버로딩된 여러 **exec 메서드**가 있고, `Process` 타입을 반환한다. 간단한 command 를 넣어서 호출해보고 싶었는데, IntellJ 상에서 classpath 관련 이슈라던가 조금 복잡-미묘한 것들이 섞여있어서 정상적으로 실행되지 않는다. 메뉴바에서 Run 했을때 가장 첫줄에 ... 줄임말로 쓰여진게 실제 command 라고 한다. 확인해보니 복잡했다.

하고나서 보니 `Process` 클래스에 대해서도 궁금해졌고, `Process` 인스턴스에서 뽑아낼 수 있는 입력/출력/에러에 대한 `InputStream` / `OutputStream` 인스턴스를 보고나니 Java 에서 I/O 관련 클래스들에 대해서 더 탐색해야할 필요성을 느꼈다.