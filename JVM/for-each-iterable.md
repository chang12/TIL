반복문의 index 가 필요없는 경우 for-each 구문으로 더 간편하게 for loop 를 구성할 수 있다. 지금까지는 보통 주어지거나 계산된 List 에 대해 for-each 구문을 사용했는데, 문득 for-each 구문을 사용하기 위한 최소 요건이 무엇인지 의문이 들었다.

정답은 **"Iterable\<T\> 인터페이스의 구현체"** 였다.

```java
class MyIterable implements Iterable<Integer> {
    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<Integer>() {
            private int count = 0;

            @Override
            public boolean hasNext() {
                return count < 10;
            }

            @Override
            public Integer next() {
                return count++;
            }
        };
    }
}
```
```java
for (int i : new MyIterable()) {
    System.out.print(" " + i);
}
```
```shell
0 1 2 3 4 5 6 7 8 9
```