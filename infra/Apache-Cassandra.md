# Cassandra의 특징

- high availability에 최적화된 대표적인 분산형 Data storage
- Consistent hashing을 이용한 Ring 구조와 Gossip protocol을 구현
  - 노드 장비들의 추가, 제거 등이 자유로움
  - 데이터센터까지 고려 할 수 있는 데이터 복제 정책을 사용하여 안정성 측면에서 많은 장점
  - Sharding 을 고려해야 할 필요도 없고 Master-Slave 와 같은 정책이 없이도 장애에 대응할 수 있으며, 필요에 따라 장비들을 늘리고 줄이는 데 큰 비용이 들지 않음
- Join, Transaction을 지원하지 않음
- Index 등의 검색을 위한 기능도 매우 단출합니다
- RDBMS와 같은 Paging을 구현하는 것이 힘듦
- Keyspace(RDBMS의 DB와 같은)나 Table 등을 과도하게 생성할 경우 Memory Overflow가 발생할 수 있음
- 계속해서 필드의 추가 변경이 이루어지는 경우 유용함. (Agile development methodology 에 유용함)
- CQL(Cassnadra Query Language) - SQL과 비슷한 쿼리 인터페이스


## vs MongoDB
Mongo는 Write시에, Memory에 먼저 Write후에, 1분 단위로 Flushing하는 Write Back 방식을 쓴다. 
즉 메모리에만 쓰면 돼서 Write가 무지 빠르다. 반대로 Read시에는 파일의 Index를 메모리에 로딩해놓고 찾는다(memory mapped file). 

이러니 성능이 좋을 수 밖에, 단 Flushing전에 Fail이 되면 데이타 유실에 의해서 Consistency 가 깨지는 문제가 발생하고, Configuration 구조상 메모리 사용량이 많으며, 확장성에 제약이 있다.
특히 Write 구조에서는 비동기 식으로 Write를 하기 때문에 Disk 성능에 덜 Sensitive하다. 즉 이 말은 DiskIO성능이 상대적으로 낮은 클라우드에서 더 잘돌아간다는 이야기.

이에 비해서 Cassandra(같은 Dynamo 계열인 Riak도 마찬가지) 는 Write Back이나 Memory Mapped file을 사용하지 않기 때문에 MongoDB에 비해서 성능이 낮을 수 밖에 없다.
더군다나, Write나 Read시 1개 이상의 Node에서 값을 읽거나(R Value) 쓰기 때문에(W Value) Consistency가 mongoDB에 비해서 나으며, 당연히 확장성도 더 높다.

즉 확장성+일관성 vs 성능간의 Trade Off 구조다.
 

출처: https://bcho.tistory.com/624 [조대협의 블로그]
