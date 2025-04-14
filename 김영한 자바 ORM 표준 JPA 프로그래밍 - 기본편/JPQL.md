## JPQL(Java Persistence Query Language) 소개
- JPQL은 객체지향 쿼리 언어다.따라서 테이블을 대상으로 쿼리하는 것이 아니라 엔티티 객체를 대상으로 쿼리한다.
- JPQL은 SQL을 추상화해서 특정데이터베이스 SQL에 의존하지 않는다.
- JPQL은 결국 SQL로 변환된다.

## 파라미터 바인딩 - 이름 기준, 위치 기준
```java
em.createQuery("SELECT m FROM Member m where m.username=:username", Member.Class); 
query.setParameter("username", usernameParam);
```
## 프로젝션
- SELECT 절에 조회할 대상을 지정하는 것
- 프로젝션 대상: 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자등 기본 데이터 타입) 

### 프로젝션 - 여러 값 조회
- SELECT m.username, m.age FROM Member m
1. Query 타입으로 조회
2. Object[] 타입으로 조회 
3. new 명령어로 조회
   - 단순 값을 DTO로 바로 조회
   - SELECT new jpabook.jpql.UserDTO(m.username, m.age) FROM Member m
   - 패키지 명을 포함한 전체 클래스 명 입력
   - 순서와 타입이 일치하는 생성자 필요

## 페이징 API
- JPA는 페이징을 다음 두 API로 추상화 S
- setFirstResult(int startPosition) : 조회 시작 위치 (0부터 시작)
- setMaxResults(int maxResult) : 조회할 데이터 수
```java
 //페이징 쿼리 
  String jpql = "select m from Member m order by m.name desc"; 
  List<Member> resultList = em.createQuery(jpql, Member.class) 
        .setFirstResult(10) 
        .setMaxResults(20) 
        .getResultList();
```
