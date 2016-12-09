## 클래스 정의
["자바스크립트 클래스를 정의하는 3가지 방법"](http://steadypost.net/post/lecture/id/13/) 포스트에서 3가지 방법을 제시한다. 그 중에서 **prototype 에 메서드를 추가**하는 아래 방법이 가장 매력적으로 다가왔다. 그래서 당분간은 이 방법으로 JS 에서 클래스 개념을 사용해보려고 한다.
```javascript
// 필드 선언
function Point() {
    this.x = 0;
    this.y = 0;
}

// 메서드 선언
Point.prototype = {
    setPoint: function(x, y) {
        this.x = x;
        this.y = y;
    },
    movePoint: function(x, y) {
        this.x += x;
        this.y += y;
    }
}

var p = new Point();
p.setPoint(1, 2); // 1, 2
p.movePoint(3, 4); // 4, 6
```