withCredentials
================

공식문서에 의하면 `XMLHttpRequest.withCredentials`은 cross-site `Access-Control` 요청이 쿠키, 인증 헤더 또는 TLS 클라이언트 자격증명을 사용해야 요청인지 boolean 값으로 나타내는 속성이라고 한다. same-origin 요청에서는 `withCredentials`를 설정해도 아무 효과가 없다.
<br/>

마주친 문제
----------
Next.js의 Client 사이드에서 특정 URL로 HTTP 요청을 하였는데, CORS 에러가 발생하였다. 해당 URL은 기존에 통신하던 서버가 아니라 새로운 서버이긴 했다.
<br/>

원인
----
Node.js 서버에서는 HTTP 요청에 대한 credential 설정이 별도로 되어 있지 않았다. 그러나 Next.js에서는 Axios를 통해서 HTTP 통신을 처리하고 있었는데, 무분별하게 모든 요청에 `withCredentials = true` 옵션을 추가하도록 interceptor에 처리하고 있었다. 따라서 브라우저-서버 간 credential 속성이 맞지 않아 CORS에러가 발생하였다.

기존의 서버는 `next.config.js`의 `rewrites`를 통해서 브라우저의 요청을 Proxy하여 서버-서버간 통신으로 처리하도록 되어 있었다. 따라서 브라우저-서버 간에서 발생하는 CORS에러는 발생하지 않았던 것.
<br/>

해결방안
---
- 현상적인 해결책
  - Next.js의 Axios interceptor에서 `withCredentials = true` 속성을 제거한다.
  - Authorization Header나 Cookies를 사용하지 않기 때문에 해당 속성은 필요가 없다.
  <br/>

- 근원적인 해결책 1
  - JWT토큰을 Authorization Header나 Cookie가 아닌 일반 Header에 담아서 통신하는 것 차제가 문제이다.
  - 따라서 토큰의 통신 방법 자체를 Authorization Header나 Cookie에 담는 방식으로 변경한다.
  - Client 사이드에서는 `withCredentials = true`를 설정하고 토큰을 담아 서버로 요청하도록 한다.
  - Server 사이드에서는 Client URL에 대한 cors 설정을 하고 credential 옵션을 true로 설정해준다.
    <br/>

- 근원적인 해결책 2
  - CORS에러는 브라우저-서버 간 통신에서만 발생한다. 따라서 서버-서버 간 통신 방식으로 변경해준다면 CORS에러는 발생하지 않는다.
  - `next.config.js`에서 통신해야하는 서버 주소를 `rewrites` 설정해주고 특정 패턴의 주소로 요청시 해당 서버로 요청가도록 한다. (이렇게 하면 브라우저에서의 요청을 Next.js 서버가 Proxy 역할을 하여 서버 쪽으로 요청하도록 바꾸어준다.)
