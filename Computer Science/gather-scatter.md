[Gather-scatter (vector addressing) Wikipedia](https://en.wikipedia.org/wiki/Gather-scatter_(vector_addressing)) 를 읽어보면 gather/scatter 가 무슨 말인지는 이해할 수 있다. sparse 하게 데이터를 들고있는 `y` 를 생각한다. `y` 에는 N 개의 non-empty 원소가 존재한다. `y` 는 sparse 하기 때문에 실제 `y` 가 점유하고있는 메모리 크기는 N 보다 훨씬 클 것이다. 이러한 `y` 는 크기 N 짜리 배열 2개로 표현할 수 있다. 각각을 `x`, `idx` 라고 했을때, gather / scatter 를 간단한 C 코드로 기술할 수 있다.

### gather

```c
for (i=0; i<N; i++) {
    x[i] = y[idx[i]];
}
```

### scatter

```c
for (i=0; i<N; i++) {
    y[idx[i]] = x[i];
}
```

두 경우 모두 `idx` 배열은 이미 계산되어있다고 가정한다. 이름을 좀 더 음미해보면, 명쾌하다. sparse 한 데이터 `y` 를 dense 한 데이터 `x` 로 바꾸는 과정이기 때문에 이름이 gather 이고, 그 반대과정이 scatter 가 된다. 

Wikipedia 문서만 읽고나서는 이게 `select` system call 과 대체 무슨 상관인가 싶었다. 아직 두 개념 사이에 연관성에 대해서 구글링해보지는 않았는데, gather / scatter 를 정리하다보니 무슨 맥락인지 추측해볼 수 있었다. 어제 `select` system call 에 대해서 정리한 내용을 떠올려보자. `select` system call 을 호출했을때 반환되는 `int` 값은, 파라미터로 넘긴 3가지 종류의 `fd_set` 들 중에서 read/write 가 준비되어있는 `file descriptor` 의 개수였다. 그러므로 read/write 가 준비된 `file descriptor` 들만 골라서 처리할 수 없었다. 예제에서는 0부터 시작해서 `select` system call 에 파라미터로 넘긴 (=system call 을 넘기기전에 알고있어야 하는) 가장 큰 `file descriptor` 값까지 순회하면서 준비된 `file descriptor` 들은 처리하고, 처리할때마다 `select` system call 반환값을 1씩 까는식으로 구현했었다. 그러므로 `file descriptor` 의 준비 여부를 파악하는 (아직 뭔지 모르는) system call 이 실제 필요한 횟수보다 많이 호출된다. 

이때 순회하며 호출하는 `file descriptor` 들의 집합을 `y` 에 대응시키면, read/write 가 준비된 `file descriptor` 들의 집합을 `x` 에 대응시킬 수 있다. 그러므로 gather 연산이 가능하다면 system call 을 딱 필요한만큼만 호출할 수 있고, 효율적인 처리가 가능해지는 것이다. 아마 이러한 맥락에서 나온 얘기가 아닐까 싶다.  
