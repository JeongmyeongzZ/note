# Transactional

>  스프링에서는 트랜잭션 처리를 지원하는데 그중 어노테이션 방식으로 @Transactional을 선언하여 사용하는 방법이 일반적이며, 선언적 트랜잭션이라 부른다.

클래스, 메서드위에 @Transactional 이 추가되면, 이 클래스에 트랜잭션 기능이 적용된 프록시 객체가 생성된다.

이 프록시 객체는 @Transactional이 포함된 메소드가 호출 될 경우, PlatformTransactionManager를 사용하여 트랜잭션을 시작하고, 정상 여부에 따라 Commit 또는 Rollback 한다.

선언적 트랜잭션에서는 런타임 예외가 발생하면 롤백한다.


# 문제점

- Dirty Read
  - 트랜잭션 A가 어떤 값을 1에서 2로 변경하고 아직 커밋하지 않은 상황에서 트랜잭션B가 같은 값을 읽는 경우 트랜잭션 B는 2가 조회 된다.
  - 트랜잭션 B가 2를 조회 한 후 혹시 A가 롤백된면 결국 트랜잭션B는 잘못된 값을 읽게 된 것이다. 즉, 아직 트랜잭션이 완료되지 않은 상황에서 데이터에 접근을 허용할 경우 발생할 수 있는 데이터 불일치이다.
- Non-Repeatable Read
  - 트랜잭션 A가 어떤 값 1을 읽었다. 이후 A는 같은 쿼리를 또 실행할 예정인데, 그 사이에 트랜잭션 B가 값 1을 2로 바꾸고 커밋해버리면 A가 같은 쿼리 두번을 날리는 사이 두 쿼리의 결과가 다르게 되어 버린다.
  - 즉, 한 트랜잭션에서 같은 쿼리를 두번 실행했을 때 발생할 수 있는 데이터 불일치이다.
- Phantom Read
  - 트랜잭션 A가 어떤 조건을 사용하여 특정 범위의 값들[0,1,2,3,4]을 읽었다. 이후 A는 같은 쿼리를 실행할 예정인데, 그 사이에 트랜잭션 B가 같은 테이블에 값[5,6,7]을 추가해버리면 A가 같은 쿼리 두번을 날리는 사이 두 쿼리의 결과가 다르게 되어 버린다.
  - 즉, 한 트랜잭션에서 일정 범위의 레코드를 두번 이상 읽을 때 발생하는 데이터 불일치이다.


## 트랜잭션 격리 (isolation)

> @Transactional(isolation=Isolation.DEFAULT)

- DEFAULT 기본 격리 수준(기본설정, DB의 Isolation Level을 따름)
- READ_UNCOMMITTED (level 0, 커밋되지 않는(트랜잭션 처리중인) 데이터에 대한 읽기를 허용)
- READ_COMMITTED (level 1, 트랜잭션이 커밋 된 확정 데이터만 읽기 허용)
- REPEATABLE_READ (level 2, 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock 그 영역에 해당되는 데이터에 대한 `수정`이 불가능하다.)
- SERIALIZABLE (level 3, 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 다른 사용자는 그 영역에 해당되는 데이터에 대한 `수정 및 입력`이 불가능하다.)



## 트랜잭션 전파 (propagation)

@Transactional은 해당 메서드를 하나의 트랜잭션 안에서 진행할 수 있도록 만들어주는 역할을 한다. 이때 트랜잭션 내부에서 트랜잭션을 또 호출한다면 스프링에서는 어떻게 처리하는가?
진행되고 있는 트랜잭션에서 다른 트랜잭션이 호출될 때 어떻게 처리할지 정하는 것을 '트랜잭션의 전파 설정'이라고 부릅니다.

트랜잭션의 전파 설정은 '@Transactional'의 옵션 'propagation'을 통해 설정할 수 있다

- REQUIRED (디폴트 속성, 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성한다.)
- SUPPORTS (이미 시작된 트랜잭션이 있으면 참여하고 그렇지 않으면 트랜잭션 없이 진행하게 만든다.)
- REQUIRES_NEW (부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성)
- MANDATORY (REQUIRED와 비슷하게 이미 시작된 트랜잭션이 있으면 참여한다. 반면에 트랜잭션이 시작된 것이 없으면 새로 시작하는 대신 예외를 발생시킨다.)
- REQUIRES_NEW (항상 새로운 트랜잭션을 시작한다. 이미 진행 중인 트랜잭션이 있으면 트랜잭션을 잠시 보류시킨다.)
- NOT_SUPPORTED (트랜잭션을 사용하지 않게 한다. 이미 진행 중인 트랜잭션이 있으면 보류시킨다.)
- NEVER (트랜잭션을 사용하지 않도록 강제한다. 이미 진행 중인 트랜잭션도 존재하면 안된다 있다면 예외를 발생시킨다.)
- NESTED (이미 진행중인 트랜잭션이 있으면 중첩 트랜잭션을 시작한다. 중첩 트랜잭션은 트랜잭션 안에 다시 트랜잭션을 만드는 것이다. 하지만 독립적인 트랜잭션을 만드는 REQUIRES_NEW와는 다르다.)



_출처_
- https://goddaehee.tistory.com/167
- https://deveric.tistory.com/86
- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Propagation.html
