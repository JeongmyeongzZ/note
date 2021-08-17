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


*Instantiating Beans*
* 생성자를 사용한 초기화
* 정적 팩토리 메소드를 사용한 초기화
* 인스턴스 팩토리 메소드를 사용한 초기화 (서비스 로케이터 패턴)



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




Dependency Resolution Process

- configuration 메타데이터를 통해 생성되고 구성 된 ApplicationContext 



### 참고 링크 
- _[이펙티브 자바](http://www.yes24.com/Product/Goods/65551284)_
- _[종립님 블로그 #스프링 문서 번역](https://johngrib.github.io/wiki/spring/document/core/)_
- _[스프링 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html)_
