검색버튼을 누르면 url를 통해서 클라이언트가 서버측으로 get요청을 보낸다.
예) orders?memberName = & orderStatus =
그러면 서버에서
@ModerAttribute를 사용한다면 memberName, orderStatus를 변수로 가지고 있는 클래스에 자동으로 바인딩된다.
예)@ModerAttribute("orderSearch")OrderSearch orderSearch)

OrderSearch 
-> String memberName
-> String orderStatus


MPA - 여러 html이 응답 받는 형태, 서버에서 랜더링이 일어나서 서버 부하가 발생
SPA - react 같은 것, 사용자가 동적으로 랜더링, 지속적인 페이지 새로고침 없이 유연하고 반응이 빠른 웹앱을 개발가능, 초기 로딩이 걸림

SSR이란 Server Side Rendering -> 페이지 별로 렌더링하는 경우, 그래서 주로 MPA SSR이 묶인다. 
CSR이란 Client Side Rendering -> React 같은 js기반 프레임워크를 사용하면 기본적으로 하나의 HTML, CSS, JS파일이 나오기 때문에 자연스래 SPA면서 CSR이 된다.
