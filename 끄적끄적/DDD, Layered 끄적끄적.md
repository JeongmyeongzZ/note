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
