# AWS Seminar

On prem 보다 managed 가 (ElastiCache)
- 서비스의 메모리를 더 많이 활용 가능
- Multi AZ 는 AZ 간 통신비용이 더 많이 과금 될 수 있음
- IAM 연동, 로그인 지원등 더 뛰어난 보안 신뢰성
- 샤딩 및 레플리케이션 기능 확장
- 보장되는 뛰어난 성능 

---

MemoryDB for Redis <-> ElastiCache 와 다른점?
- Data Durability (내구성) 보장
    - Elasticache 는 뒷 단에 별도의 Rdb, s3, dynamo, document 같은 내구성이 보장되는 별도의 저장소를 쓰는 편
    - 저널로그(트랜잭션로그)를 가장 네트웍이 가까운 노드에 저장하고 완료하면 Ack 하는 방식. (완료 되었다는 commit 을 전달)
        - 비동기로 레플리카에 동기화 하기 때문에 eventually consistency
        - Primary 는 strongly consistency
- Wirte query 가 더 느림 (6배정도 더 느리지만, RDB 라고 생각할 땐 나쁘지 않은 속도) (read 는 동일함)
- Primary node failover 시 privmary 가 새로 구성될 때 시간이 소요됨 (대신 durability 가 보장된다)
    - elastiCache 는 복제지연시간이 짧으면 그냥 미반영된 트랜잭션 반영 안하고 바로 primary 선출되지만, memory 는 다 하고나서 제일 빨리 된 애가 primary 로 선출 됨 
- 말그대로 DB 처럼 사용 가능. 비용이 비싸지만 (30프로정도) 얘 하나로만 된다면 비용효율적일수도

---

ElastiCache failover
- Replica 중 복제 지연이 가장 적은애가 primary 가 됨. (그 지연시간 만큼 데이터가 누락됨)

---

Redis single thread
- Lock 관련 고려가 필요 없어짐 
- Enhanced I/O mode 는 pool,  tcp 관련 task 는 관련 처리는 별도 thread 에서 하면서 I/O 는 싱글스레드로 지원하도록 기존 기능을 훼손하지 않음
- 대신 시간 복잡도를 중요하게 생각해야함 (대량 장애 유발 가능)
- Unlink, backup 같은 기능은 별도 스레드를 사용

---

Re-sharding (scale in- out) 을 할 때 균일하게 슬롯을 분포하려고 할 때 어쩔 수 없이 읽기/쓰기/삭제 식의 작업이 생김.
Zero-downtime 이긴 하지만 cloudwatch 등을 통해 모니터링이 필요함


- Cluster 모드를 사용하지 않으면 Replication 만 지원되지만, 사용하면 Data partitioning 도 지원됨 (물론, Redis Cluster 자체가 데이터 분산을 위한 방법이긴 하지만.)
- Shard는 단순히 관련된 Node의 집합이다. 그렇다고 무한정의 Node를 가지고 있는 것은 아니고 최대 6개 까지 가질 수 있다.
    - 최대 6개는 Primary Node 1개와 Replica Node 최대 5개를 의미한다.
    - 그리고 Cluster 환경에서는 최대 15개까지 사용가능하다. (그렇기 때문에 Cluster 환경에서 전체 Node의 개수는 최대 90개가 될 수 있다.)
- Node group (shard) 는 각각의 hash slot 을 가지고 있다.

- 스케일 아웃 활동은 ElastiCache for Redis 클러스터에 있는 샤드/복제본 수를 늘립니다.


Binary safe
- 개행문자 같은 것들도 영향이 없다

Lazy loading
- 기본적인 캐싱패턴 있으면 뱉고 없으면 다른 저장소에서 읽어서 캐싱하고 등

Write through
- 저장소에 write 하고 lambda 등에 trigger 를 하면 그 뒤에 cache 를 update 하거나 하는 방식

