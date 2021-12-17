# AOP ( Aspect Oriented Programming )

AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 

관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다.



예로들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 된다. 또한 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있다.
 
소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을 흩어진 관심사 (Crosscutting Concerns) 라 부른다. 

흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지다.


# 스프링 AOP

스프링 AOP 를 이해하려면 Proxy 에 대한 이해가 필요하다. 아래의 코드를 살펴보자.

프록시 객체에 트랜잭션 등 부가 기능 관련 로직을 위치시키고, 클라이언트 요청이 발생하면 실제 타깃 객체는 프록시로부터 요청을 위임받아 핵심 비즈니스 로직을 실행시키는 형태이며 이는 데코레이터 패턴을 생각하면 될 것 같다.
(상속을 통한 구현이고 흐름제어 외의 부가적인 부가기능 (cross-cutting concern, 흩어진 관심사) 에 대한 구현이 들어가져서 이름과 동일한 프록시패턴으로 바라볼 수 있을지는..)

```
@Service
@RequiredArgsConstructor
public class UserServiceProxy implements UserService {

    private final UserService target;
    private final PlatformTransactionManager transactionManager;

    @Override
    public void sendMoneyToAnotherUser(Long senderId, Long receiverId, Long money) {
        TransactionStatus transaction = transactionManager.getTransaction(new DefaultTransactionDefinition());
        try {
            //로깅 관련 로직 추가
            //보안 관련 로직 추가
            target.sendMoneyToAnotherUser(senderId, receiverId, money);
            transactionManager.commit(transaction);
        } catch (RuntimeException runtimeException) {
            transactionManager.rollback(transaction);
            throw runtimeException;
        }
    }
}
```

이는 그런데.. 이런 관심사를 분리해 구현할 객체가 100개면 프록시라고 칭할 객체가 100개가 생겨버리는 문제가 발생한다. 프레임워크는 이런 반복적인 작업들을 손쉽게 처리할 수 있는 방법을 항상 제공한다.

스프링에선 ProxyFactoryBean 이 있다. (Java 엔 Refrection API 라는 게 있다고 한다)

`DefaultAdvisorAutoProxyCreator` 라는 클래스가 Bean 을 자동으로 프록시로 만들어준다. `BeanPostProcessor` 라는 Bean 후처리기 인터페이스를 확장한 클래스이며, 동작 플로우는 다음과 같다.




프록시 패턴 기반의 AOP 구현체, 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위해서임
스프링 빈에만 AOP를 적용 가능
모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가 ...)에 대한 해결책을 지원하는 것이 목적




_참고_
- _https://tecoble.techcourse.co.kr/post/2021-06-25-aop-transaction/_
