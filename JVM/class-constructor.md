```scala
// 아래처럼 정의하면 adId, version 필드에 접근할 수 없다.
// <console>:68: error: value adId is not a member of Canada
class Canada(adId: String, version: String) 

// val 키워드를 붙이면 필드에 접근할 수 있다.
class Canada(val adId: String, val version: String)

// auxiliary 생성자를 만들 수 있다. 여러줄에 걸쳐 만드는게 안된다.
class Canada(val adId: String, val version: String, val os: String) {
	def this(adId: String, os: String) = this(adId, "5.0.0", os)
}

// default parameter 도 설정할 수 있다. 다만 시그니처에서 맨 뒤에 받아야 한다.
class Canada(val adId: String, val country: String, val os: String = "5.0.0")
new Canada("id", "KR", "4.4.4")
new Canada("id", "KR")
```