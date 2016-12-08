세진이형 도와드리는 작업하면서 느낀점
* pure javascript 가 임베딩되서 HTML DOM element 를 생성하는 이번같은 경우는 어떻게 코드를 정규표현식으로 파싱하는 방법이 가능했다. 하지만 만약에 제대로 된 javascript 코드가 AJAX 호출하고 난리쳐서 생성해낸 DOM 을 파싱할 생각하니 아찔하다.
* 그런 슬픈 경우에는 어쩔 수 없이 **Selenium** 같은걸 써야한다. 얘는 browser 인스턴스 하나 만들고 get 메서드 호출했을 뿐인데 정말 브라우저가 뜬다. 여기서 적절히 trigger 에 event 를 걸어서 복잡한 동적 javascript 에 대응해야할 것이다.
* 크롤링 해오고, 이를 Python 으로 파일로 저장하는 등의 작업에서 **encoding / codec** 을 잘 신경써줘야 한다. 한글과 관련되서 UTF-8 을 처리하기 위해서는 file 을 'w' 가 아니라 **"wb"** 모드로 열었어야 했다. "wb" 모드로 열면 write 메서드의 인자도 bytes 로 넘겨줘야 하기때문에 str 을 encode 해서 넘긴다. 