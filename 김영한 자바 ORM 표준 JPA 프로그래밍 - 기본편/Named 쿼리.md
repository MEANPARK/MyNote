## Named 쿼리 - 정적 쿼리
- 미리 정의해서 이름을 부여해두고 사용하는 JPQL
- 정적 쿼리
- 어노테이션, XML에 정의
- 애플리케이션 로딩 시점에 초기화 후 재사용
- #### 애플리케이션 로딩 시점에 쿼리를 검증
  - 해당 쿼리를 스캔하고 캐싱한 상태로 진행
   
```java
//이건 잘 사용안하고 Spring Data JPA @Query로 자주 사용
@Entity
@NamedQuery(
       name = "Member.findByUsername",
       query="select m from Member m where m.username = :username")
public class Member {
   ...
}

List<Member> resultList = 
em.createNamedQuery("Member.findByUsername", Member.class)
    .setParameter("username", "회원1")
    .getResultList();

//Spring Data JPA
@Query("select u from User u where u.emailAddress = ?1")
User findByEmailAddress(String emailAddress);
```

##
글 및 코드의 출처는 김영한 강사님께 있습니다.
