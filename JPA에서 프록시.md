## 프록시 기초 ##
- em.find() vs em.getReference()
- em.find(): 데이터베이스를 통해서 실제 엔티티 객체 조회
- em.getReference(): 데이터베이스 조회를 미루는 가짜(프록시) 엔티티 객체 조회
  - DB에 쿼리가 안 나가는데 객체가 조회가 되는 것
  - 객체에서 값을 가져오려고 하면 쿼리가 발생

### 프록시 특징
- 실제 클래스를 상속 받아서 만들어짐
- 실제 클래스와 겉 모양이 같다.
- 사용하는 입장에서는 진짜 객체인지 프록시 객체인지 구분하지 않고  사용하면 됨(이론상)
- 프록시 객체는 실제 객체의 참조(target)를 보관
- 프록시 객체를 호출하면 프록시 객체는 실제 객체의 메소드 호출

> ### 한번에 이해하기
> - 프록시 객체는 처음 사용할 때 한 번만 초기화
> - 프록시 객체를 초기화 할 때, 프록시 객체가 실제 엔티티로 바뀌는 것은 아님, 초기화되면 프록시 객체를 통해서 실제 엔티티에 접근 가능
> - 프록시 객체는 원본 엔티티를 상속받음, 따라서 타입 체크시 주의해야함 (== 비교 실패, 대신 instance of 사용)
> - 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 em.getReference()를 호출해도 실제 엔티티 반환
> - 영속성 컨텍스트의 도움을 받을 수 없는 준영속 상태일 때, 프록시를 초기화하면 문제 발생 (하이버네이트는 org.hibernate.LazyInitializationException 예외를 터트림)

### 프록시 확인
- 프록시 인스턴스의 초기화 여부 확인
  - PersistenceUnitUtil.isLoaded(Object entity)
- 프록시 클래스 확인 방법
  - entity.getClass().getName() 출력(..javasist.. or HibernateProxy…)
- 프록시 강제 초기화
  - org.hibernate.Hibernate.initialize(entity);
  - 하이버네이트에 존재, 초기화 => 객체를 생성하고 프록시와 연결한다.
- 참고: JPA 표준은 강제 초기화 없음
  - 강제 호출: member.getName()


## 즉시로딩과 지연로딩 ##
### (fetch = FetchType.EAGER)
- JPA 구현체는 가능하면 조인을 사용해서 SQL 한번에 함께 조회

- 프록시와 즉시로딩 주의
  - 가급적 지연 로딩만 사용(특히 실무에서)
  - 즉시 로딩을 적용하면 예상하지 못한 SQL이 발생
  - 즉시 로딩은 JPQL에서 N+1 문제를 일으킨다.
    - 1 : 처음 쿼리, N : 결과의 수  
  - @ManyToOne, @OneToOne은 기본이 즉시 로딩
    
    -> LAZY로 설정
  
  - @OneToMany, @ManyToMany는 기본이 지연 로딩

### (fetch = FetchType.LAZY)
- Column에 해당하는 객체를 프록시로 생성
- 그 객체를 조회할 때, 객체를 생성하고 프록시와 연결
  - Member member = em.find(Member.class, 1L);
  - Team team = member.getTeam();
  - team.getName(); // 실제 team을 사용하는 시점에 초기화(DB 조회)

- 지연 로딩 활용 - 실무
  - 모든 연관관계에 지연 로딩을 사용해라!
  - 실무에서 즉시 로딩을 사용하지 마라!
  - JPQL fetch 조인이나, 엔티티 그래프 기능을 사용해라!(뒤에서 설명)
  - 즉시 로딩은 상상하지 못한 쿼리가 나간다.

