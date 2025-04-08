검색버튼을 누르면 url를 통해서 클라이언트가 서버측으로 get요청을 보낸다.

예) orders?memberName = & orderStatus =

그러면 서버에서

@ModerAttribute를 사용한다면 memberName, orderStatus를 변수로 가지고 있는 클래스에 자동으로 바인딩된다.

예)@ModerAttribute("orderSearch")OrderSearch orderSearch)

OrderSearch 

-> String memberName

-> String orderStatus
