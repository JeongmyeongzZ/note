# Action Domain Responder

action domain responder 라는 패턴이 있다는 것을 접하게 됐다. 직역하면 액션 도메인 응답기 정도가 되겠다.. MVC 와 같이 ADR 이라고 표현하는 듯.

Action 은 Controller, Domain 은 Model, Responder 는 Http Response Factory 로 이해하면 쉽다.

이는 MVC 와 유사하다고 보여지는지, 검색하면 vs MVC 와 같은 키워드가 보이는데 ADR 은 HTTP 통신에 대한 부분에 깊이 관여하고 있다.

`MVC to the context of web REST APIs` 이라는 표현에 걸맞게 MVC pattern 에 대한 변형, 변화 (variants) 중 한 모습이다.



## Action

Domain 과 Responder 의 연결고리입니다. HTTP 요청에서 수집 된 데이터를 Responder 에게 전달합니다.

https://github.com/pmjones/adr-example/blob/master/src/Web/Blog/Add/BlogAddAction.php


## Dmain

필요에 따라 상태와 지속성을 수정하는 응용 프로그램의 핵심을 형성하는 도메인 논리의 진입점입니다. 이것은 트랜잭션 스크립트, 서비스 계층, 애플리케이션 서비스 또는 이와 유사한 것일 수 있습니다.

https://github.com/pmjones/adr-example/blob/master/src/Domain/Blog/BlogService.php


## Responder

Action 에서 수신한 데이터에서 HTTP 응답을 빌드하는 Presentation logic 입니다. 상태 코드, 헤더 및 쿠키, 콘텐츠, 형식 지정 및 변환, 템플릿 및 View 등을 다룹니다.

https://github.com/pmjones/adr-example/blob/master/src/Web/Blog/Add/BlogAddResponder.php



PHP 생태계, 그러니까 BE, FE 의 경계가 모호하며, 웹 개발이라고 뭉뚱그려 표현 될 만한 생태계에서 찾아 볼 수 있는 패턴인 것 같다.

PHP 생태계에도 View Model 구현에 대한 여러 패키지가 있지만, (예를 들어 spatie 그룹이나..) MVC , 많게는 MVCS 정도의 레이어드 기반 아키텍처가 성행하는 PHP 판에서 충분히 선택해볼만한 패턴인 것 같다.

프레젠테이션 로직과, 서비스로직 (여기선 Domain 이라고 표현했지만, Application 인..)  그리고 그 사이를 중재하는 객체까지 역할에 맞게 나눠서 사용할 수 있는 장점이 있다. 

ADR 이라는 이름과는 별개로 서비스, 중재자, 리스폰더 내지는 뷰 팩터리 정도로 의미나 컨벤션에 맞게 잘 활용하면 괜찮을 것 같다.



_참고_
- _https://herbertograca.com/2018/09/03/action-domain-responder/_
- _https://github.com/pmjones/adr#action-domain-responder_
