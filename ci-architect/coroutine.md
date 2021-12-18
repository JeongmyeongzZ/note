# 코루틴(Coroutine)?

협력형 멀티태스킹을 프로그래밍 언어로 표현하자면 Co + Routine 이다. Co라는 접두어는 “협력”, “함께”라는 의미를 지니고 있다. 

Routine 은 하나의 태스크, 함수 정도로 생각하면 된다. 즉 협력하는 함수다. Routine 에는 우리가 흔히 알고있는 main routine과 sub routine이 존재한다.

```

function main () {
  return sub("coroutine");
}

function sub (someString) {
  return someString + "!";
}

```

코드로 보면 이해하기 쉽다. 우리가 정말 흔히 알고 있는 개념이다. main method 가 곧 메인루틴, sub method 가 곧 서브루틴이다.

Sub Routine은 루틴에 진입하는 지점과 루틴을 빠져나오는 지점이 명확하다. 

즉, 메인 루틴이 서브루틴을 호출하면, 서브루틴의 맨 처음 부분에 진입하여 return문 을 만나거나 서브루틴의 닫는 괄호를 만나면 해당 서브루틴을 빠져나오게 된다.

---

## 그러나 코루틴(Coroutine)은 조금 다르다.

코루틴도 routine이기 때문에 하나의 함수로 생각하자. 그런데 이 함수에 진입할 수 있는 진입점도 여러개고, 함수를 빠져나갈 수 있는 탈출점도 여러개다. 

즉, 코루틴 함수는 꼭 return 문이나 마지막 닫는 괄호를 만나지 않더라도 언제든지 중간에 나갈 수 있고, 언제든지 다시 나갔던 그 지점으로 들어올 수 있다.

```

function routine () {
  startCoRoutine {
     first();
    
     second();
  }
}

suspend function first () {
  //something
}

suspend function second () {
  //something
}

```

다음과 같은 코드가 있다.

routine method 안에는 startCoRoutine 이라는 코루틴 빌더가 있다. (실제로 이런 이름의 빌더는 아니며 실제 코루틴 라이브러리에는 다른방식으로 코루틴을 만든다.)

이런 코루틴 빌더를 만나게 되면 해당 함수는 코루틴으로 작동할 수 있다. 따라서 언제든 함수 실행 중간에 나갈 수도 있고, 다시 들어올 수도 있는 자격이 부여되는 것이다. 

언제 코루틴을 중간에 나갈수 있을까? suspend로 선언된 함수를 만나면 코루틴 밖으로 잠시 나갈 수 있다.

이제 순서를 따라 가보자.

쓰레드의 main 함수가 routine method 를 호출하면 코루틴 블럭을 만나 하나의 코루틴을 만들어 시작한다.

suspend 로 정의된 first() 부분에서 더 이상 아래 코드를 실행하지 않고 first() 라는 코루틴 함수를 (잠시) 탈출한다.

메인 쓰레드가 해당 코루틴을 탈출했다. 그렇다고 쓰레드가 놀고 있을리는 없다. 우리가 짜 놓은 다른 코드들을 실행할 수도 있고, 안드로이드라면 UI 애니메이션을 처리 할 수도 있다. 

그러나 first method 는 어디선가 계속 동작되고 있다. suspend를 만나 코루틴을 탈출했지만, first method 의 기능은 메인쓰레드에서 동시성 프로그래밍으로 작동하고 있을수도 있고, 다른 쓰레드에서 돌아가고 있을 수도 있다. 그것은 개발자가 자유롭게 선택할 수 있다.

그렇게 메인쓰레드가 다른 코드들을 실행하다가도, first()가 제 역할을 다 끝내면 다시 아까 탈출했던 코루틴 routine()으로 돌아온다. 아까 멈추어놓았던 first() 아래인 second()부터 재개(resume)된다.

위의 과정에서 보았듯이 코루틴 함수는 언제든지 나왔다가 다시 들어올 수 있다. 코루틴의 이런 성향은 동시성 프로그래밍과 밀접한 관계가 있다.


이런 코루틴을 하나의 쓰레드, 멀티 쓰레드에서 활용할 수 있다. 멀티쓰레드 환경에서도 컨텍스트 스위칭에 드는 비용이 상당히 적다는 게 또 코루틴의 장점이라고 한다!

ReactiveX(rx) 를 활용해 Kotlin, Java, Go, Javascript 등에선 이런 동시성 프로그래밍을 코루틴이란 개념을 통해 해결하고 있다.

다들 접해봤을 "콜백 지옥" 이라는 용어가 나온 배경처럼 만약, first 가 동작한 이후에 second 함수가 동작해야된다는 가정이 있다면 어떨까 라는 생각이 들 수 있다.

코드상 순차적으로 동작하겠다고 당연히 느낄 수 있도록 처리가 가능하다. (javascript Promise 와 같은 느낌..)

그러나 Rx 는 러닝커브가 상당하다고 알려져 있다. Observable이 무엇인지, just가 무엇인지, flatMap은 무엇인지 operation 들이 상당히 많다.

Kotlin 에선 이 코루틴을 정말 보기 쉽게 아래와 같이 작성이 가능하다! 그래서 코틀린 코틀린 코루틴 코루틴 하는구나..

```
suspend goWork() {
  val person = 'Kim'
  
  val availablePerson = wakeUp(person);
  
  val cleanPerson = wash(availablePerson);
  
  drive(cleanPerson);
}
```


_참고_
- _https://wooooooak.github.io/kotlin/2019/08/25/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%BD%94%EB%A3%A8%ED%8B%B4-%EA%B0%9C%EB%85%90-%EC%9D%B5%ED%9E%88%EA%B8%B0/_
