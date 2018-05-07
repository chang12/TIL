git 으로 관리하는 프로젝트에서 collection 에 element 를 하나 추가하면 git diff 에는 두 줄 수정된걸로 나와서 불편하다.

```
--- a/file.scala
+++ b/file.scala
@@ -270,7 +270,8 @@ object O {
     "e1",
     "e2",
     "e3",
-    "last"
+    "last",
+    "new"
   )
``` 

그러므로 collection 의 마지막 element 에도 뒤에 comma 를 붙일 수 있다면 편리하다. 이를 **trailing comma** 라고 부르는데, scala 도 2.12.2 부터 이를 제공한다. 관련 내용은 [SIP-27 - Trailing Commas](http://docs.scala-lang.org/sips/completed/trailing-commas.html) 에서 확인할 수 있다.