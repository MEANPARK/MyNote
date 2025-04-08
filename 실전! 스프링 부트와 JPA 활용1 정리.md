# 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발

#### 개발 순서: 서비스, 리포지토리 계층을 개발하고, 테스트 케이스를 작성해서 검증, 마지막에 웹 계층 적용

<br>

#### 도메인 모델 패턴, 트랜잭션 스크립트 패턴

> 주문 서비스의 주문과 주문 취소 메서드를 보면 비즈니스 로직 대부분이 엔티티에 있다. 서비스 계층은 단순히 엔티티에 필요한 요청을 위임하는 역할을 한다. 이처럼 엔티티가 비즈니스 로직을 가지고 객체 지향의 특성을 적극 활용하는 것을 도메인 모델 패턴(http://martinfowler.com/eaaCatalog/domainModel.html)이라 한다. 반대로 엔티티에는 비즈니스 로직이 거의 없고 서비스 계층에서 대부분의 비즈니스 로직을 처리하는 것을 트랜잭션 스크립트 패턴(http://martinfowler.com/eaaCatalog/transactionScript.html)이라 한다

<br>

#### @PersistenceContext
영속성 컨택스트
> 주로 Repository에서 사용
```
@PersistenceContext
private EntityManager em;
```

<br>

#### @RequiredArgsConstructor
> 생성자 주입 방식으로 @Autowired를 생략가능하며, final 키워드를 추가한 변수를 자동으로 생성자 주입한다.
 ```java
public class MemberService {
    @Autowired
    MemberRepository memberRepository;
    ...
}
↓↓↓↓↓
@RequiredArgsConstructor
public class Memberservice {
    private final MemberRepository memberRepository;
    ...
}
```

<br>


**TDD**

```
@Test
public void [이름]() {
    //given

    //when

    //then
}
```

<br>

---

<br>

