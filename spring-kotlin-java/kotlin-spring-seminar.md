# 우아한 테크 코프링 세미나

## 코틀린?

-정적 타입 지정 언어 => 컴파일 시점에 타입이 정해져 있다. (자바와 동일)
-젯브레인즈에서 개발한 오픈소스
-OO, FP 스타일 모두 사용 가능
-간결하고 실용적, 코루틴(코틀린 라이브러리)


val => 읽기 전용 프로퍼티
var => 변경 가능한 프로퍼티


코틀린과 자바파일이 같이 있을 땐 코틀린 컴파일러가 먼저 동작해 class 파일 (바이트 파일) 로 변환됨. 그 이후 애너테이션 프로세싱(롬복) 등이 일어나 기존 자바 파일또한 class 파일로 변환됨 
=> 코틀린 파일에서 롬복을 사용하는 자바 파일에 접근 할 수 없음
=> 빌드 순서를 변경해 코틀린 파일을 그 이후에 컴파일  할 수 있지만, 그럼 그 반대의 상황이 발생함
=> 롬복 대신 데이터클래스를 사용하자


## 스프링부트 in Kotlin

CGLIB 을 사용하여 @configuration 클래스에 대한 프록시를 생성하는데, 코틀린은 컴파일 과정에 final 을 자동으로 붙여서 이름 방지하려면 open 변경자를 추가해야함.
=> All-open 컴파일러 플러그인 (kotlin-spring 플러그인은 해당 플러그인을 포함하고 있음)

필드 주입 (@autowired) lateint 변경자를 붙이면 프로퍼티를 나중에 초기화 할 수 있다. (var 여야함) (ObjectMapper 같은..)
=> 뒷받침 하는 필드(backing field)가 존재하는 프로퍼티는 인스터화 될 때 초기화 되어야함. null 로 초기화 하지 않아도 됨.

json string 을 객체로 역직렬화 할 때 사용하는 jackson 코틀린 모듈이 있음
=> 빈 생성자를 초기화 해줘야하는 자바상의 불편함이 있는데 해당 모듈을 쓰면 없어도 직렬화와 역직렬화를 지원함


## Persistence

JPA 에서 엔티티 클래스를 생성할 때 매개변수가 없는 생성자가 필요한데, No-arg 플러그인을 사용하면 @entity, @embeddable, @mappedSuperClass 가 기본적으로 적용됨

@transient => get() 을 통해 대체 가능. 접근 시 게터를 통해 계산 (영속화 하지 않음)

## Tips

demo https://github.com/woowacourse/service-apply

kotlin dsl https://developer-munny.tistory.com/5

DSL ?
특정 도메인(산업, 분야등 특정 영역)에 특화된 언어를 말한다. 
"문제 영역의 해결에는 그 영역의 언어를 전제로 둬야하며, 거기에서 프로그래밍 솔루션을 꺼내는 것이 중요하다." 라고 Dave Thomas가 한 말을 생각하면 이해하기 쉽다.

우리 주변에 있는 DSL
- java 
    . ANT, Maven, struts-config.xml, Seasar2 S2DAO, HQL(Hibernate Query Language), JMock
- Ruby
    . Rails Validations, Rails ActiveRecord, Rake, RSpec, Capistrano, Cucumber
- 기타
    . SQL, CSS, Regular Expression(정규식), Make, graphviz
    
    
확장 함수를 JPA 메서드와 함께 활용하자. 
=> Optional => Nullable 을 코틀린 jpa 확장함수로 제공하는데, 이를 또 확장해 Nullable => Exception 과 같이 활용 가능함

ktlint 코틀린 린트 컨밴션 적용이 쉽다. (자바의 소나린트보다 훨씬) 위 데모 Readme 에 예제가 있다.
