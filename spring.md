# Spring 

---

## 공식 문서 탐구

### Bean & Context


`org.springframework.beans`와 `org.springframework.context`는 스프링 프레임워크 IoC 컨테이너의 기반이 되는 패키지다.

Spring IoC 컨테이너가 관리하는 자바 객체를 `Bean`이라고 부른다. 익숙한 라라벨로 생각하면 서비스 컨테이너에 올라가있는 인스턴스들.

`new` 라는 생성 키워드를 통해 생성된 클래스는 스프링에서 빈이라고 볼 수 없으며, `ApplicationContext`가 만들고, 그 안에 담겨 관리되는 객체를 빈이라고 말한다.

ApplicationContext 는 `IoC 엔진` `싱글톤 레지스트리`, IoC 방식을 따라 만들어진 일종의 빈 팩토리이다. 

빈 팩토리라고 말할 때는 빈을 생성하고 관계를 설정하는 IoC의 기본 기능에 초점을 맞춘 것이고, ApplicationContext 는 별도의 정보를 참고해서 빈의 생성(인스턴스화), 관계설정(configurationing) 등의 제어를 총괄하는 기능에 초점을 맞춘 것 이다.

_(onfiguration meta data = XML, Java 애노테이션, Java 코드로 작성할 수 있으며 이 것을 컨테이너가 읽고 인스턴스화 하고 설정하고, 조립한다) XML에 일일이 명시하는 방식도 있지만, @Bean과 같은 어노테이션 형태로 더 많이 이용 됨_




### 참고 링크 
- _[이펙티브 자바](http://www.yes24.com/Product/Goods/65551284)_
- _[종립님 블로그 #스프링 문서 번역](https://johngrib.github.io/wiki/spring/document/core/)_
- _[스프링 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html)_
