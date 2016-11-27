**"라벨링"** 기능을 구현하고 싶었다. `Task` 라는 이름의 model 이 있고, `Label` 이라는 이름의 model 이 있다. `Task` 에 `Label` 로 라벨링하고 싶었으므로 서로를 **ManyToManyField** 로 관계를 맺었다. 웹 페이지에서 Django 서버로는 AJAX 요청을 보내게 만들었다.
## JavaScript
http://stackoverflow.com/questions/18045867/post-jquery-array-to-django

간단하다고 생각했는데, 예상치 못하게 기능이 제대로 동작하지 않아서 애를 먹었다. 그 와중에 위 링크의 답변을 찾아서 큰 도움을 받았다. 흥미로우면서도 잘 이해가 되지 않는 syntax 였지만, AJAX 로 데이터를 보낼때 데이터가 **Array** 타입일 경우 변수명을 `label_ids` 가 아니라 `label_ids[]` 처럼 지어줘야 했다.
## Django
마찬가지로 위 링크에서 도움을 받았는데, `request.POST` 의 **QueryDict** 에서 리스트 타입의 데이터를 받아오고 싶을때는 단순히 **get** 메서드가 아니라 **getlist** 메서드를 사용해야 했다.   

**ManyToManyField** 와 관련해서는 필드를 선언한 쪽이 어느곳이냐에 따라서 접근 메서드가 달라진다. `Task` model 에 **labels** 라는 이름으로 `Label` 을 **ManyToManyField** 로 추가했을 경우에는 아래처럼 된다.
```python
task = Task.objects.get(...)
task.labels.add(label1) # label 하나 추가
task.labels.remove(label2) # label 하나 제거
task.labels.set([label3, label4, label5]) # 기존 관계를 한번에 덮어써버릴 수 있다.

label = Labels.objects.get(...)
label.task_set.all() # Label 쪽에서는 task_set 메서드로 접근한다.
```
set 메서드를 사용하면 한줄로 여러개의 라벨링이 가능했다. 하지만 성능상으로는 좀 따져봐야 할 것이다. ORM 으로는 한 줄이지만, 실제 여러번의 Query 를 호출할 가능성이 농후하다. 