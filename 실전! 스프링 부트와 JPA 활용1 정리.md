# 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발

#### 개발 순서: 서비스, 리포지토리 계층을 개발하고, 테스트 케이스를 작성해서 검증, 마지막에 웹 계층 적용

<br>

#### 도메인 모델 패턴, 트랜잭션 스크립트 패턴

> 주문 서비스의 주문과 주문 취소 메서드를 보면 비즈니스 로직 대부분이 엔티티에 있다. 서비스 계층은 단순히 엔티티에 필요한 요청을 위임하는 역할을 한다. 이처럼 엔티티가 비즈니스 로직을 가지고 객체 지향의 특성을 적극 활용하는 것을 도메인 모델 패턴(http://martinfowler.com/eaaCatalog/domainModel.html)이라 한다. 반대로 엔티티에는 비즈니스 로직이 거의 없고 서비스 계층에서 대부분의 비즈니스 로직을 처리하는 것을 트랜잭션 스크립트 패턴(http://martinfowler.com/eaaCatalog/transactionScript.html)이라 한다

#### 참고: 외래 키가 있는 곳을 연관관계의 주인으로 정해라.
> 연관관계의 주인은 단순히 외래 키를 누가 관리하냐의 문제이지 비즈니스상 우위에 있다고 주인으로 정하면 안된다.. 예를 들어서 자동차와 바퀴가 있으면, 일대다 관계에서 항상 다쪽에 외래 키가 있으므로 외래 키가 있는 바퀴를 연관관계의 주인으로 정하면 된다. 물론 자동차를 연관관계의 주인으로 정하는 것이 불가능 한 것은 아니지만, 자동차를 연관관계의 주인으로 정하면 자동차가 관리하지 않는 바퀴 테이블의 외래 키 값이 업데이트 되므로 관리와 유지보수가 어렵고, 추가적으로 별도의 업데이트 쿼리가 발생하는 성능 문제도 있다. 자세한 내용은 JPA 기본편을 참고하자.

#### 참고: 테이블명이 `ORDER`가 아니라 `ORDERS`인 것은 
> 데이터베이스가 `order by`때문에 예약어로 잡고 있는 경우가 많다. 그래서 관례상 `ORDERS`를 많이 사용한다.

#### 참고: 실제 코드에서는 DB에 소문자 + _(언더스코어) 스타일을 사용하겠다.
> 데이터베이스 테이블명, 컬럼명에 대한 관례는 회사마다 다르다. 보통은 대문자 + _(언더스코어)나 소문자 + _(언더스코어) 방식 중에 하나를 지정해서 일관성 있게 사용한다. 강의에서 설명할 때는 객체와 차이를 나타내기 위해 데이터베이스 테이블, 컬럼명은 대문자를 사용했지만, **실제 코드에서는 소문자 + _(언더스코어) 스타일을 사용하겠다.

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
> 참고: 스프링 데이터 JPA를 사용하면 EntityMenager도 주입 가능
```java
@Repository
@RequiredArgsConstructor
public class MemberRepository {

   private final EntityManager em;
   ...
}
```

<br>

#### Service, Repository, Controller 어노테이션
```
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor

@Repository
@RequiredArgsConstructor

@Controller
@RequiredArgsConstructor
```

<br>

**기술 설명**
- @Service
- @Transactional: 트랜잭션, 영속성 컨텍스트
  - readOnly=true: 데이터의 변경이 없는 읽기 전용 메서드에 사용, 영속성 컨텍스트를 플러시 하지 않으므로 약간의 성능 향상(읽기 전용에는 다 적용)
  - 데이터베이스 드라이버가 지원하면 DB에서 성능 향상

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

