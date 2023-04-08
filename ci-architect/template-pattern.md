템플릿 메서드 패턴, 전략 패턴, 템플릿 콜백 패턴은 상속을 통한 구현 예제에 항상 비슷한 예제로 나오는 패턴이다.


# template method pattern

```java
public abstract class DoSomethingAbstractService {

    public void doSomething(String contents) {
        log.info("시작");

        // 그 외 이것 저것
        
        request(contents);
    }

    public abstract boolean request(String contents);
}


public class DoSomthingAService extends DoSomethingAbstractService {

    @Override
    public boolean request(String contents) {
        // DoSomthingAService 클래스에 국한 된 구현
    }
}

```

doSomething method (template) 자체는 변경 사항이 거의 없다. 이를 상속을 통해 구현한 클래스에 request 구현에만 변경사항이 있게 됨 

템플릿 메서드 패턴의 단점은 상속을 사용한다는 점이다. 상속을 사용하게 되면, 자식 클래스들은 부모 클래스에게 강하게 결합된다.
(만약 부모 클래스가 변경된다면 자식 클래스 모두를 변경해야 되는 상황도 올 수 있다.)

---

# strategy pattern

전략패턴은 흔히 우리가 제일 익숙한 interface 를 통한 추상화가 해당 된다.

```java
public interface DoSomethingStrategy {

    boolean request(String contents);
}

public class DoSomthingAStrategy implements DoSomethingStrategy {

    @Override
    public boolean execute(String contents) {
        log.info("시작");
        
        // A 에만 국한 된 구현
        return true;
    }
}

// DoSomthingAStrategy 와 같은 전략을 주입 해 사용
```

---

# template callback pattern

콜백 코드는 호출(call)한 곳에서 실행되지 않고, 호출을 요청한 곳(back or callee)에서 실행되는 코드를 말한다.

이 패턴은 GOF에서 나온 패턴이 아닌, 스프링 프레임워크에서 부르는 패턴이다.

템플릿 콜백 패턴은 전략 패턴의 용어 중 일부만 살짝 바꾸면 바로 같아진다.

Strategy Pattern → Template Callback Pattern
Strategy → Callback
Context → Template


