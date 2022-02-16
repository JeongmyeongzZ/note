- https://medium.com/riiid-teamblog-kr/gradle%EA%B3%BC-%ED%95%A8%EA%BB%98%ED%95%98%EB%8A%94-backend-layered-architecture-97117b344ba8
- https://docs.microsoft.com/ko-kr/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/ddd-oriented-microservice
- https://softwareengineering.stackexchange.com/questions/396151/which-layer-do-ddd-repositories-belong-to
- https://medium.com/@SlackBeck/%EC%95%A0%EA%B7%B8%EB%A6%AC%EA%B2%8C%EC%9E%87-%ED%95%98%EB%82%98%EC%97%90-%EB%A6%AC%ED%8C%8C%EC%A7%80%ED%86%A0%EB%A6%AC-%ED%95%98%EB%82%98-f97a69662f63


Can Entities, Value Objects or Domain Services call Repositories?
No. The domain layer should preferably remain agnostic to repositories. Application services should take on the responsibility of transactions and repository interactions.
요건 도메인 서비스가 레파지터리를 호출해도 되는가에 대한 얘기인데
트랜잭션하고, 레파지터리와의 상호작용은 애플리케이션의 역할이라고 하는 내용인데

도메인 레이어에도 서비스라는 용어가 존재할 수 있고 거기서 행위를 녹일 수 있는데
레파지터리 웅앵 하려면 진작 도메인이라는 명제 자체가 잘못됐다 입니다 전
도메인 레이어엔 말그대로 도메인로직이 있어야된다
VO 에 서픽스 프리픽스 어쩌구 얘기나온거부터 도메인 모델을 제대로 관리하지 못했다.
Account, Model, Address 처럼 이름만으로 역할이 분명하고 의미가 분명한게 도메인 모델이고 Param 어쩌구 하는건 계층간 데이터를 옮기는 DTO 일 뿐이다. 입니다

애플리케이션 (우리가 흔히 말하는 서비스) 에서 레파지터리를 호출하고, 호출하기 위한 파라미터 (대게 엔티티) 에 대한 비즈니스 로직은 도메인에서 구현되어야 한다


Can Entities, Value Objects or Domain Services call Repositories?
No. The domain layer should preferably remain agnostic to repositories. Application services should take on the responsibility of transactions and repository interactions.

애플리케이션이 책임져야 한다.
트랜잭션 및 레파지터리와의 상호관계 조율을


_2022 02 08 컬리 회의 속 내 생각들 .._


- _http://jaynewho.com/post/45_

엔티티가 비대해질 수 있다. 이는 바운디드 컨텍스트 `같은 도메인이어도, 사용되는 맥락이 다르면 엔티티를 별도로 매핑하라` 의 원칙을 지키면 생각보다 쉽다.
각 엔티티를 따로 만들면 된다.
코드 중복이 심하지 않냐? 효용성이 더 크다. 이는 응집성 있는 도메인 모델을 만들 수 있다

---

- https://github.com/rogelio-o/spring-ddd/blob/master/spring-ddd-infrastructure/src/main/java/com/rogelioorts/training/spring/ddd/repositories/MoviesFeignRepository.java
- https://softwareengineering.stackexchange.com/questions/426874/what-layer-do-third-party-api-request-response-models-go-in-and-what-do-you-call

외부 API 를 호출하는 영역은 Infrastructure 계층임이 분명해보인다.
그런데, 요청에 필요한 객체와 응답을 매핑하는 객체는 어느 레이어에 속하는가? Infra 객체에 model 들을 때려 박아야하는가, 아님 이것을 도메인 객체로 간주해야하는가, 아니면 단순 DTO 관점으로 애플리케이션 레벨에 있어야하는가 ?


It somewhat depends on whether you are using inverted dependencies or not. If you aren't, then the models can live together with the service in the Infrastructure layer, since your Application will depend on Infrastructure and have access to all of its services and models at the same time.

However, if you're using inverted dependencies, that means that the Application (or Domain) layer defines the service's interface, which needs access to the in/output models, which means that you can't just plop those models in Infrastructure. In this case, they belong to the service's contract, and should be kept closeby to where you store the service interface itself. I personally would put them in the same directory, since they tend to uniquely belong to each other.

이는 말한다. 의존성 역전을 사용하고 있냐 아니냐에 따라 조금 다르다고.
사용하고 있지 않은경우는 Infra 레이어에 그냥 두어도 된다고 말한다. 인프라계층에 종속되고 나선 서비스나 모델에 대해서 접근할 수 있기 때문에..

One internal (i.e. assembly-private) model in Infrastructure for the service logic
One public model in Infrastructure to be consumed by its dependents (i.e. Application)
The service class in Infrastructure will map from one to the other as it needs it.
인프라 계층의 하나의 private 모델 
하나의 퍼블릭한 인프라 모델, 그리고 인프라 계층에서 그들을 매핑..

=

그러나 의존성 역전이 이루어져있는 경우는 두면 안된다고 한다.
애플리케이션 계층의 서비스가 해당 모델에 직접 접근 할 수 없기 때문에.. 
이 경우엔 service's contract 에 속해야된다고 한다. 그리고 서비스 인터페이스와 가깝게 있어야한다고 말한다. 둘은 되게 가깝고 붙어있기 때문에.. 

One model with the service interface in Application (just like before)
One internal (i.e. assembly-private) model in Infrastructure for the service logic
The service class in Infrastructure will map from one to the other as it needs it.
애플리케이션의 모델, 서비스 클래스와 함께 
하나의 인프라 계층 private 모델, 그리고 인프라계층의 서비스가 그들을 매핑..


