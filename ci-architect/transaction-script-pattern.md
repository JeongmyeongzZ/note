# Transaction script pattern

은행의 계좌 이체 서비스를 생각해보면 '잔고 확인 -> 받는 사람 확인 -> 이체 실행 -> 잔고 감소'의 로직 순서가 하나의 로직으로 처리되어야 하고, 이 과정에서 한번이라도 오류가 발생하면 모든 처리가 취소되고, 모든 과정이 성공해야 비로서 처리가 완료된다. 

즉, All-or-Nothing 개념의 트랜잭션 처리가 되는 것이다.

트랜잭션 스크립트(Transaction Script) 패턴은 이렇게 하나의 트랜잭션으로 구성된 로직을 단일 함수 또는 단일 스크립트에서 처리하는 구조를 갖는다. 




Enterprise Application Architecture 을 설계할 때,

- Domain Logic Patterns: 
- Data Source Architectural Patterns: 
- Object-Relational Behavioral Patterns

등등 다양한 개발 패턴들이 존재한다.

그 중에서 도메인 모델 패턴(Domain Model)과 트랜잭션 스크립트 패턴(Transaction Script)이 배교대상이 되는 경우가 많은데 이런 차이가 있다는 글도 있다..

엔티티가 비즈니스 로직을 가지고 객체 지향의 특성을 적극 활용하는 것을 도메인 모델 패턴이라 한다.

-> 쉽게 말해 Domain 부분에서 비즈니스 로직을 이용해 개발을 진행하는 것을 얘기한다.

 
엔티티에는 비즈니스 로직이 거의 없고 서비스 계층에서 대부분의 비즈니스 로직을 처리하는 것을 트랜잭션 스크립트 패턴이라 한다.

-> 쉽게 말해 Domain부분이 아닌 Service 부분에서 비즈니스 로직을 이용해 개발을 진행하는 것을 얘기한다.

---

도메인이 비즈니스 로직의 주도권을 가지고 개발하는 것을 도메인 주도 설계라 합니다. 이렇게 해두면 서비스의 많은 로직이 엔티티로 이동하고, 서비스는 엔티티를 호출하는 정도의 얇은 비즈니스 로직을 가지게 됩니다.

이렇게 하면 information expert pattern을 지키면서 개발할 수 있습니다. GRASP (General Responsibility Assignment Software Patterns, 객체 설계 및 책임 할당의 9가지 기본 원칙, object-oriented design)

반대로 엔티티는 단순히 getter, setter만 제공하고, 서비스에 비즈니스 로직이 모두 있어도 됩니다.

이렇게 되면 서비스 로직이 커지고, 엔티티는 단순히 데이터를 전달하는 역할만 담당하게 됩니다.

전자는 엔티티를 객체로 사용하는 것이고, 후자는 엔티티를 자료 구조로 사용하는 방식이지요.

그러면 항상 전자가 정답인가? 라고 하면 그렇지는 않습니다. 둘다 장단점이 있기 때문에, 상황에 맞는 적절한 방법을 선택하는 것이 중요합니다.

트랜잭션 스크립트 패턴은 논리가 거의 또는 전혀 없고 앞으로 괴물처럼 성장할 가능성이 거의 없는 소규모 응용 프로그램에 적합합니다..

---

```
public class SomeTransactionScript {
    public Result do(...) {
        try {

            SomeTransaction tx = ...;
            tx.begin();

            // 1. 잔고확인
            ...
            // 2. 받는 사람 확인
            ...
            // 3. 이체 실행
            ...
            // 4. 잔고 감소
            ...
            tx.commit();

        } catch(..) {
            tx.rollback();
            ...
        } finally {
            ...
            ...
        }
    }
}
```

위와 같이 '한 서비스 메서드에 트랜잭션 관련 코드를 몰아놓는 방식' 이라고 이해할 여지가 있는데, 이것보다 도메인로직에 대한 처리 주체가 애플리케이션 계층단에서 이뤄지냐, 도메인 계층단에서 이뤄지냐의 관점으로 바라보는 것이 좋겠다.

이는 DDD 라는 설계 근본에 대한 예제로도 적당하게 쓰일 수 있을 것 같다. 이렇게 되면 커지는 서비스와 파편화된 로직들의 관리가 너무 어려워지지 않을까? 





_참고_
- _https://javacan.tistory.com/entry/94_
- _https://www.inflearn.com/questions/117315_

