# Spring 

---

## 공식 문서 탐구

### Bean & Context


`org.springframework.beans`와 `org.springframework.context`는 스프링 프레임워크 IoC 컨테이너의 기반이 되는 패키지다.

Spring IoC 컨테이너가 관리하는 자바 객체를 `Bean`이라고 부른다. 익숙한 라라벨로 생각하면 서비스 컨테이너에 올라가있는 인스턴스들.

`new` 라는 생성 키워드를 통해 생성된 클래스는 스프링에서 빈이라고 볼 수 없으며, `ApplicationContext`가 만들고, 그 안에 담겨 관리되는 객체를 빈이라고 말한다.

ApplicationContext 는 `IoC 엔진` `싱글톤 레지스트리`, IoC 방식을 따라 만들어진 일종의 빈 팩토리이다. 

빈 팩토리라고 말할 때는 빈을 생성하고 관계를 설정하는 IoC의 기본 기능에 초점을 맞춘 것이고, ApplicationContext 는 별도의 정보를 참고해서 빈의 생성(인스턴스화), 관계설정(configurationing) 등의 제어를 총괄하는 기능에 초점을 맞춘 것 이다.

_Configuration meta data XML, Java 애노테이션, Java 코드로 작성할 수 있으며 이 것을 컨테이너가 읽고 인스턴스화 하고 설정하고, 조립한다. XML에 일일이 명시하는 방식도 있지만, @Bean과 같은 어노테이션 형태로 더 많이 이용 됨_


####Instantiating Beans
* 생성자를 사용한 초기화
* 정적 팩토리 메소드를 사용한 초기화
* 인스턴스 팩토리 메소드를 사용한 초기화 (서비스 로케이터 패턴)

####Bean scope
* singleton -> (기본값) bean 정의를 하나의 객체 인스턴스로 생성하고, 모든 Spring IoC 컨테이너에 공유한다.
  *  싱글톤 인스턴스는 싱글톤 bean을 모아두는 캐시에 저장됩니다. (요청이 들어오면 캐싱된 bean을 리턴하는 방식)
  *  Spring의 싱글톤 bean의 개념은 GoF의 디자인 패턴 책에 나오는 싱글톤 패턴과는 다릅니다.
    *  GoF 싱글턴 개념 : 클래스 로더를 2개 이상 사용하는 경우, 인스턴스가 2개 이상 생성될 수 있습니다. 이런 경우에는 클래스 로더를 지정해야 합니다. 
    *  Spring 싱글턴 개념 : ApplicationContext 기준이라는 것은 web.xml 에서 root context 하나와 servlet cotnext 여러개를 등록할 수 있는데, 이 각각의 context 들이 싱글턴 범위가 됩니다.
* prototype -> 하나의 bean 정의를 여러 개의 객체 인스턴스로 생성. bean을 요청할 때마다 새로운 bean 인스턴스를 생성한다.
  * 일반적으로 DAO는 프로토타입으로 구성되지 않습니다. 보통 DAO는 상태를 저장하지 않기 때문입니다
  * 어떤 면에서는, 프로토타입 스코프 bean에 대해서 Spring 컨테이너는 Java의 new 연산자를 대신하고 있을 뿐이기도 합니다.
  * 컨테이너는 프로토타입 객체를 초기화하고, 구성하고, 조립하고 클라이언트에 전달하기만 합니다. (인스턴스를 컨테이너에 저장하지 않음)
* request -> bean의 스코프를 HTTP request의 생명주기에 맞춘다. request scope bean은 각각의 HTTP request가 있을 때마다 생성됩니다. 
* session -> bean의 스코프를 HTTP Session의 생명주기에 맞춘다. 
* application -> bean의 스코프를 ServletContext의 생명주기에 맞춥니다.
* websocket -> bean의 스코프를 WebSocket의 생명주기에 맞춥니다.

singleton, prototype 을 제외한 모든 스코프는 web-aware Spring ApplicationContext에서만 쓸 수 있습니다.


_Servlet ?_

> 자바를 사용하여 웹 페이지를 동적으로 생성하는 서버측 프로그램 혹은 그 사양
> Servlet Container에 의해 관리, 실행된다
> 개발자는 Servlet을 만들어 HTTP 요청을 받아 처리하는 부분을 구현한다 (`HttpServlet` class 를 통해 구현)
> HttpServlet class 엔 doGet, doPost 등의 메서드가 있는데, 이 메서드들은 HttpServletRequest req, HttpServletResponse resp 를 인자로 갖는다.
> 메서드를 참고하면 알 수 있듯 요청(Request)과 응답(Response) 즉, Http 웹 서버 기능을 맡는다.

> Tomcat은 Servlet Container, Servlet Engine 이라고 표현할 수 있으며 개발자가 작성한 Servlet을 관리한다 (어떤 요청 `request` 냐의 따라 어떤 Servlet 을 실행시킬지를 결정)



### DI (Dependency Injection)

IoC ? 

객체간 결합도를 줄이기 위해 제어를 역전시키는 개념

상위 레벨 모듈(고수준 모듈, 응용계층 or 도메인 계층)과 하위 레벨의 모듈(인프라스트럭처 계층)에 대한 의존성이 있을 때, 두 객체간 결합도가 높기 때문에 하위레벨 모듈 변경에 의해 상위 레벨의 모듈이 변경 될 수 있다.

의존성을 자동으로 주입하기 위한 프레임워크라고 볼 수 있다. (객체의 생성, 라이프 사이클 관리, 의존성 주입등의 역할)

DI가 바로 IoC 를 구현하는 하나의 패턴이다. (이 외에 Factory 패턴등이 있음)

IoC Container(DI Container)가 해당 역할을 수행하는데 Spring 에서의 컨테이너는 ApplicationContext 라고 볼 수 있다. 

---

ApplicationContext - BeanFactory - BeanScanner

Reflection에서 getTypesAnnotatedWith 메서드가 Reflection을 통해 Base Package에 지정한 하위 패키지에 있는 모든 Class들을 가져와서 @Component 라는 어노테이션이 달린 Class들을 scan 하는 일을 수행한다. (Laravel의 의 Reflection 의 build 와 상당히 유사한 듯)

해당 과정을 통해 Bean Factory 에 Map<Class<?> Object> 로 Bean 객체들을 가지고 있는다.

registerBean을 시행할 때 beanInitializeHistory를 통해 객체들이 이미 등록되어 있는지 아닌지 검증하고 순환 참조에 대한 유효성 검사를 실시한다.

```
1. Container가 로드되면, Bean에 해당하는 객체들을 scan 하여, 해당 Bean들을 생성하려고 한다. (이 과정에서 순환 참조로 인한 BeanCurrentlyInCreationException 예외가 발생할 수 있음.)
2. Bean 들이 생성 될 때 사용자가 지정한 init method가 존재한다면, 객체가 생성되고 init이 실행되게 된다. (@PostConstruct)
3. 그 뒤에 사용자가 지정한 utility method(afterPropertiesSet 과 같은 메서드)가 존재한다면 해당 메서드가 실행 된다. (InitializingBean interface)
4. 프로그램이 종료되기 전에 (Container 종료 전) destory 메서드가 존재한다면, 실행하고 정상적으로 종료한다. (@PreDestory)
```

* Constructor 기반의 DI
* Setter 기반의 DI
* Field 기반의 DI (@Autowired, 바람직하지 않음)

생성자 기반 DI와 setter 기반의 DI를 조합해서 사용할 수 있다. 필수적인 의존관계에는 생성자 방식을 사용하고, 옵셔널한 의존관계에는 setter 메소드 방식을 사용하는 것은 좋은 규칙일 수 있다. 그러나 스프링은 생성자 기반의 의존성 주입을 추천한다. 

객체들을 불변 객체로 만들 수 있고, 필수 의존관계들이 null이 되지 않도록 해주며 생성된 컴포넌트는 초기화가 완전히 끝난 상태로 클라이언트 코드로 리턴된다.

그러나 생성자 인자가 너무 많으면 코드에서 나쁜 code smell 을 유발한다. 이런 경우에는 클래스에 너무 많은 책임이 부여되었을 가능성이 높으므로, 관심사를 적절히 분리하기 위해 리팩토링을 해야한다.



### Annotation

Spring은 @Component, @Service, @Controller와 같은 추가적인 스테레오 타입을 제공합니다. @Component 는 Spring이 관리하는 모든 컴포넌트에 대한 일반적인 스테레오 타입입니다.

@Repository, @Service, @Controller는 좀 더 구체적인 유즈 케이스(각각 persistence, service, presentation 레이어)를 위한 전문화된 @Component입니다.

@Repository는 persistence 레이어에서 자동 예외 번역을 위한 marker로도 지원을 받고 있습니다.

_예외 번역?_

> DAO에서 Hibernate 또는 JPA를 사용할 때 지속성 기술의 기본 예외 클래스를 처리하는 방법을 결정해야 합니다
> DAO는 기술에 따라 HibernateException 또는 의 하위 클래스를 throw합니다 PersistenceException. 이러한 예외는 모두 런타임 예외이며 선언하거나 catch할 필요가 없습니다.
> 이것은 호출자가 예외를 일반적으로 치명적인 것으로만 처리할 수 있음을 의미합니다. 호출자를 구현 전략에 묶지 않고 특정 원인(예: 낙관적 잠금 실패)을 포착하는 것은 불가능합니다.
> Spring에서는 @Repository주석을 통해 투명하게 예외 번역을 적용할 수 있습니다 .
> 포스트 프로세서는 자동으로 모든 예외 변환기( PersistenceExceptionTranslator인터페이스 구현)를 찾고 @Repository주석으로 표시된 모든 빈에 조언(advice) 하여 발견된 변환기가 던져진 예외에 적절한 번역을 가로채고 적용할 수 있도록 합니다.
> 요약하면, Spring 관리 트랜잭션, 종속성 주입 및 Spring의 사용자 정의 예외 계층으로의 투명한 예외 변환(원하는 경우)의 이점을 계속 누리면서 일반 지속성 기술의 API 및 주석을 기반으로 DAO를 구현할 수 있습니다.


Spring이 제공하는 애노테이션들 중 많은 수가 여러분의 코드에서 meta-annotation으로 사용될 수 있습니다. meta-annotation은 다른 애노테이션에 적용할 수 있는 애노테이션을 말합니다. 예를 들어 앞에서 이야기했던 @Service 애노테이션의 경우 @Component가 meta-annotation으로 붙어 있습니다. (@RestController는 @Controller와 @ResponseBody가 조합된 애노테이션)

이를 `meta-annotation`을 조합한 `composed annotation` 이라고 한다. `composed annotation`은 커스터마이즈를 허용하기 위해 meta-annotation의 attribute를 재선언하는 옵션을 고려할 수 있습니다.


### 참고 링크 
- _[이펙티브 자바](http://www.yes24.com/Product/Goods/65551284)_
- _[종립님 블로그 #스프링 문서 번역](https://johngrib.github.io/wiki/spring/document/core/)_
- _[스프링 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html)_
- _[Servlet에 대한 개념](https://jeong-pro.tistory.com/222)_

