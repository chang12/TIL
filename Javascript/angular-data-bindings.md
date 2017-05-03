공식 문서에서는 component-based architecture 를 위해서 Angular 1.5 부터 출시된 **"component"** 를 쓰라고 권장한다. component 를 활용해서 설계할 경우, Angular 2 로 마이그레이션하기 좋다는 코멘트도 덧붙여져 있었다. 이와 더불어 component 를 사용하는 가이드라인을 제시하면서, input / output 등 상황별로 사용해야할 data binding 종류를 설명한다. output 을 위해서 **'&'** 을 쓰라는 내용이 있었어서 한번 정리해보고 싶었다.

> Outputs are realized with **&** bindings, which function as **callbacks** to component events.

내가 구현하고 싶은 로직은 아래의 4단계로 구성된다.

* component scope 에서 event 가 발생 (ex: component 의 버튼이 눌렸다)
* component 를 감싸는 controller 에 데이터를 전달
* controller 는 sibling component 에게 이를 전달
* 전달받은 sibling component 의 어떤 동작이 트리거

