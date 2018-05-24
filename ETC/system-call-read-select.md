### read

`read` system call 은 blocking 이라고 들었다. 그래서 Unix 문서를 찾아봤는데, 만약 file 의 끝이거나 파라미터로 넘긴 count 값보다 더 작은 크기의 데이터가 남아있다면 그만큼만 읽고 그 크기를 반환한다고 적혀있었다. 그래서 마치 blocking 이 아니라 즉시 반환하는 것으로 읽혀서 의아했다.

나와 비슷한 의문이 든 사람이 직접 존재하지 않는 file 을 만들어서 테스트하고, 그 결과가 이해되지 않는다면서 올린 [SO 포스트](https://stackoverflow.com/questions/14713800/how-to-block-the-read-system-call)가 있었다. 답변이 친절하게 달려있었다. `read` system call 을 file system 에 호출했을때는 즉시 반환되는게 맞다고 한다. **하지만 socket 이나 pipe 에 대해서 호출할때는 읽어들일 bytes 가 채워질때까지 blocking 된다고 한다.** 그러므로 blocking 이 문제가 되는 맥락이 따로 있는 것이었다.

### select

이에 반해 `select` system call 은 내가 관심을 두고있는 read/write/error `file descriptor` 들의 집합을 파라미터로 넘기면서 호출한다. 그러면 넘긴 `file descriptor` 들에 대해서 해당 연산을 처리할 수 있는 상황에 있는 `file descriptor` 의 개수를 바로 반환해준다. 그러므로 non-blocking 인 것이다.

그러므로 `select` system call 을 호출해서 가용 상태의 `file descriptor` 개수를 획득한 다음에, 0 부터 최대 `file descriptor` 값까지 순회하면서 가용 상태의 `file descriptor` 를 처리하고, 개수를 하나 까는 식으로 이후 작업이 이뤄진다.

기존에 `read` system call 을 사용하는 경우에는 가용 상태가 될때까지 blocking 되므로 서버 프로그램에서 클라이언트와 연결을 하나 맺을때마다 새로운 thread 를 만들어줘야했다. 그러나 `select` system call 을 사용하는 경우에는 새로운 클라이언트와 연결을 맺고 클라이언트와, 생성되는 `socket` 이 품고있는 `file descriptor` (`socket` 이 `file descriptor` 를 품는다는게 맞는 표현인지 모르겠다) 의 매핑만 잘 들고 있으면 `select` system call 의 반환값으로 이후 작업을 수행할때 하나의 thread 에서 여러 클라이언트와의 연결을 처리할 수 있다. **가용 상태의 `file descriptor` 에 대응되는 클라이언트와의 연결만 처리하는 것이 가능하기 때문이다.**
