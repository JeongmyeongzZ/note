# 모델링

- 사용자 업무를 표현하는 수단이며, 개발자와 사용자 사이의 의사소통을 위해 사용
- 현실세계를 규칙적으로 나타낼 수 있어 공통적인 이해가 가능하게 함


## 추상화(abstraction) : 자세함을 무시하며 필요한 부분만을 추출하는 활동

> 필요한 부분만을 표현하고 불필요한 부분을 제거하여 간결하고 이해하기 쉽게 만드는 작업



## 분류화(classification) : 관계있는 개체들에 대한 그룹화

> 시스템에 요구되는 데이터의 특성을 규명할 경우에는 각각의 객체들을 기술하는 것보다 묶어진 엔티티 타입을 기술하는 것이 편리하여 비슷한 엔티티를 묶는 작업



## 일반화(generalization) : 재사용을 위한 의미적 종속관계를 말함

> 연관성이 있는 2개 이상의 객체 집합을 묶어 좀 더 상위의 객체 집합을 형성하는 것, 객체 사이에 유사성이 존재할 때 이 유사성을 모아 하나의 새로운 객체 타입을 정의 내리는 것

일반적으로 알고 있는 상속


---


# 객체 모델링

오브젝트를 모델링하고, 오브젝트 간의 관계를 표시하며, 오브젝트가 수행하는 일과 제공하는 서비스에 대해 설명할 수 있습니다.

클래스 다이어그램은 오브젝트 모델링 프로세스의 근간이 되며 시스템의 정적 구조를 모델링합니다



## Dependency (의존)

클래스 다이어그램에서 일반적으로 가장 많이 사용하는 관계. 클래스가 다른 클래스를 참조하는 것을 의미합니다.



## Association & Direct Association (연관)

클래스 다이어그램에서의 Association은 다른 객체의 참조를 가지는 필드를 의미합니다. 

실선으로 표현되며 방향성이 존재하는 경우에는 화살표를 넣어 표시합니다. 그리고 둘의 연관관계가 어떻게 되는지 숫자로 표시할 수 있습니다.

연관관계의 숫자 표현은 아래와 같이 합니다.

1 - 1개의 표현
* - 0 ~ n 개의 표현
n...m : n 부터 m까지 연관관계를 맺음
아래 예제는 양방향 연관관계를 가지며 1(Board) : n(Comment)의 관계를 표시한 것입니다.



## Aggregation (집합)

클래스 다이어그램에서 Aggregation과 Composition은 Association의 특수한 관계입니다. Aggregation은 Association의 집합관계를 나타내는 것으로 Collection이나 Array를 이용하는 관계입니다. 하지만 이 관계는 일반적인 Association으로도 충분히 나타낼 수 있는 관계입니다. 우리가 위에서 Association의 예제가 바로 1 : N 연관관계를 나타낸 것입니다. Aggregation과 Association의 모호성은 지속적으로 논란이 되고 있지만 결론이 나지 않았다고 합니다. 논란

Aggregation은 실선에 빈 다이아몬드로 나타냅니다.



## Composition (합성)

Composigiton은 클래스 연관관계에서 강한 결합의 관계를 의미합니다. 즉, 참조하는 클래스의 라이프 사이클이 종속적이라는 말입니다. 다이어그램으로는 실선에 채워져있는 다이아몬드로 표시합니다.




_출처_
- https://mk28.tistory.com/173
- https://www.nextree.co.kr/p6753/
