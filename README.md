
# 자바 ORM 표준 JPA 프로그래밍 리뷰

# 1장. JPA 소개

<aside>
💡 객체 모델링, 구현, 유지보수 작업의 생산성 향상

</aside>

## 가. 객체 모델링 작업의 생산성 향상

- 객체 모델링 시 설계지향의 충돌에 따른 부작용을 고려하지 않아도 됨으로 모델링 작업의 생산성이 향상된다. ORM을 도입하면 객체지향 패러다임에 따라 객체 모델링한 결과를 그대로 관계형데이터베이스에 적용할 수 있다. ORM을 도입하기 전에는 RDB에 저장할 객체의 경우 온전히 객체지향 패러다임에 따라 모델링할 수 없었다.


## 나. 구현 및 유지보수 작업의 생산성 향상

- SQL 기반으로 영속적 데이터를 다루는 작업이 사라지면 따라 새로운 기능을 구현하고 유지보수하는 작업의 생산성이 향상된다.
- MyBatis 사용한 경험과 비교해보면 JPA 도입에 따라 확실히 작업량이 줄었음. 하지만 JPA에 대한 이해가 낮으면 오히려 성능이 안나오기도 함
- ORM 도입에 따라 서비스 전체를 높은 수준으로 올릴 수 있었음. ORM이 없으면 서비스의 데이터 저장과 비지니스 로직이 너무 엉켜서 직관성이 매우 떨어졌음

## 다. 추가 질문

- JPA가 해결하지 못한 패러다임의 불일치 상황을 어떻게 분류할 수 있는가? 데이터 탐색, 상속, 동일성, 연관성, 세분성
  
- ORM을 도입한다면 더 이상 관계형 모델을 고려하지 않고 객체 모델링해도 부작용이 없는가? 있었다. BG의 태그의 경우, 객체지향적으로 모델링했지만 일부 성능 상 이슈가 존재함. 여러 객체에 태그가 걸쳐 있기 때문에 태그가 너무 많이 걸쳐있는 경우 락이 발생하여 다른 작업이 불가한 경우가 있었음

# 2장. JPA 시작

<aside>
💡 패러다임 불일치를 해소하기 위한 수단으로 객체와 RDB 사이의 중간 매개체

</aside>

## 가. 엔티티 매니저: 영속성 객체 가상 관리

- 엔티티 매니저는 영속성 객체와 RDB 사이에서 객체의 갱신과 조회 작업을 중재한다. 각각의 작업에 맞는 EntityManager의 메소드를 호출하면 이에 대응하는 SQL이 생성되어 RDB에 반영된다. 단, 수정작업에 대응되는 EntityManger의 메소드는 존재하지 않는다. EntityManger는 객체의 변경사항을 자동감지하여 UPDATE 쿼리를 생성하기 때문에 별도의 메소드가 필요하지 않다.

## 나. JPQL: 영속성 객체 대상 조회

- JPQL은 객체 중심으로 RDB에 질의하기 위한 매개체다. 영속성 객체 대상 조회 시 엔티티 매니저는 createQuery 메소드의 인자로 JPQL을 넘긴다. JPA는 JPQL에 기반해서 SQL을 생성한다. 단, 기본키만으로 영속성 객체 자체를 조회할 때 엔티티 매니저는 JPQL을 사용하지 않고 find 메소드를 호출하여 직접 SQL을 생성하고 이를 RDB에 반영시킨다.

## 다. 추가 질문

- 엔티티 매니저를 쓰레드 간 공유했을 때 발생할 수 있는 부작용에는 어떤 것이 있는가? 엔티티 매니저 내 캐싱된 데이터가 있는데 이를 쓰레드 간 공유하면 데이터 불일치가 발생할 수 있음
- JPQL은 사용에 어떤 제약이 존재하는가? 일단 러닝 커브가 존재함. 조건절을 걸 때 일 부 복잡한 경우가 있음. JPQL 복잡성을 낮추기 위해 Query DSL을 채택하는 조직도 존재함. 
