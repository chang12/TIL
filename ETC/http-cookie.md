HTTP cookie 라는 키워드를 많이 들어봤지만 제대로 모르는 것 같아서 정리를 시작했다.

## [MDN 문서](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)를 간단히 읽어보기

* 서버에서 응답에 `Set-Cookie` 헤더를 포함시키면 브라우저는 이를 저장하고, 이후 **같은 서버**로 향하는 HTTP 요청의 `Cookie` 헤더에 그 값을 실어준다. 이 작업을 어플리케이션 프로그램에 상관없이 브라우저가 알아서 (혹은 HTTP 클라이언트 라이브러리에서) 해주기 때문에 편리할 것이다.
* `Set-Cookie` 헤더의 값은 **name=value** 형태의 값이다. 여러 값을 저장시키기 싶다면 `Set-Cookie` 헤더를 여러개 포함시키면 된다. 클라이언트가 요청할때는 `Cookie` 라는 단일 헤더에 `;` separated 로 적는 용법이 소개되었다. HTTP 요청에서만 `;` separated 가 사용되는지는 확실히 모른다.
* 보안과 관련해서 많은 이슈가 있는것으로 보인다. 크롬 브라우저 개발자 도구의 콘솔에서 `document.cookie` 코드로 cookie 값을 얻어올 수 있었다 (MDN 문서 페이지에서). 보안에 신경써야하는 cookie 라면 Javascript 코드로 접근할 수 없도록 막아주는 방법이 존재하는 것 같다. 보안 관련해서는 일단 자세히 보지 않고 넘어간다.

## 간단한 실습

Jersey 프레임워크를 사용해서 정말 간단한 실습을 해봤다. 실습을 위한 간단한 `Resource` 를 준비한다.

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.ws.rs.CookieParam;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.core.NewCookie;
import javax.ws.rs.core.Response;

@Path("/test")
public class SetCookieResource {
    private static final Logger LOGGER = LoggerFactory.getLogger(SetCookieResource.class);

    @GET
    public Response setCookie(@CookieParam("test-cookie") String cookie) {
        LOGGER.info("test-cookie: {}", cookie);
        return Response.ok().cookie(new NewCookie("test-cookie", "max")).build();
    }
}
```

Jersey 에 대해서 제대로 이해하지 못한 상황이다. 테스트 서버를 구동하기 위한 가장 간단한 방법은 아래와 같은 듯 하다.

```java
Server server = new Server(5000);
ServletContextHandler handler = new ServletContextHandler(server, "/");
ResourceConfig resourceConfig = new ResourceConfig().register(new SetCookieResource());
FilterHolder filterHolder = new FilterHolder(new ServletContainer(resourceConfig));
handler.addFilter(filterHolder, "/*", EnumSet.allOf(DispatcherType.class));
server.start();
```

브라우저로 테스트 해보면 프로그래머가 신경 쓸 필요 없이 브라우저 / Jersey 의 도움으로 HTTP cookie 를 프로토콜에 따라 주고받는 것을 확인할 수 있다.