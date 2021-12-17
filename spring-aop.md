# AOP ( Aspect Oriented Programming )

AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 

관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다.



예로들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 된다. 또한 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있다.
 
소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을 흩어진 관심사 (Crosscutting Concerns) 라 부른다. 

흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지다.

> Aspect는 부가될 기능을 정의한 Advice와, 해당 Advice를 어디에 적용할 지를 결정하는 Pointcut 정보를 가지고 있습니다.


# Spring AOP

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

1. Spring Container는 해당 후처리기가 Bean으로 등록되어 있으면 Bean들을 생성할 때 후처리기에 보내 후처리 작업을 요청합니다.
2. Bean이 프록시 적용 대상이라면, 후처리기는 타깃 Bean을 프록시로 감싼 오브젝트로 바꿔치기 하여 Spring Container에게 반환합니다.


---

Spring AOP 를 적용하는 방법엔 아래와 같은 방법 등이 있다.
- Spring AOP 이용
- 위 예제처럼 생성
- AspectJ 등을 사용해 구현


```
@Aspect
@Component
@RequiredArgsConstructor
public class TxAspect {

    private final PlatformTransactionManager transactionManager;

    @Pointcut("execution(* com.demo.user.UserService.send*(..))")
    public void getUsers() {
    }

    @Pointcut("execution(* com.demo.user.BankService.update*(..))")
    public void getBanks() {
    }

    @Around("getUsers() || getBakns()")
    public Object applyTx(ProceedingJoinPoint joinpoint) throws Throwable {
        TransactionStatus transaction = transactionManager.getTransaction(new DefaultTransactionDefinition());
        try {
            Object object = jointPoint.proceed();
            transactionManager.commit(transaction);
            return object;
        } catch (RuntimeException runtimeException) {
            transactionManager.rollback(transaction);
            throw runtimeException;
        }
    }
}
```

AspectJ 에서 트랜잭션 관련 관심사를 분리해 Aspect 로 구현한 모습은 위와 같다. (spring 엔 이미 @transactional 이 있고, 이는 사실 위와 같은 관심사 분리를 통해 도출 된 결과물이라고 봐도 된다)

관련해서 낯선 속성 값들이 보이는데 어떤 의미를 가지고 있는지 참고하면 좋을 용어들.


- Target (핵심 기능을 담고 있는 모듈로 타겟은 부가기능을 부여할 대상)
- Advice (타겟에 제공할 부가기능을 담고 있는 모듈)
- Join Point (어드바이스가 적용될 수 있는 위치, 타겟 객체가 구현한 인터페이스의 모든 메서드는 조인 포인트가 된다.)
- Pointcut (어드바이스를 적용할 타겟의 메서드를 선별하는 정규표현식이다. 포인트컷 표현식은 execution으로 시작하고 메서드의 Signature를 비교하는 방법을 주로 이용한다.)
- Aspect (애스펙트는 AOP의 기본 모듈, 싱글톤 형태의 객체로 존재한다.)
- Advisor (어드바이저 = 어드바이스 + 포인트컷, Spring AOP에서만 사용되는 특별한 용어)
- Weaving (위빙은 포인트컷에 의해서 결정된 타겟의 조인 포인트에 어드바이스를 삽입하는 과정을 뜻한다. AOP가 타겟의 코드에 영향을 주지 않으면서 필요한 어드바이스를 추가할 수 있도록해주는 핵심적인 처리과정이다.)



_참고_
- _https://tecoble.techcourse.co.kr/post/2021-06-25-aop-transaction/_
- _https://shlee0882.tistory.com/206_
