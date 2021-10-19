# Dead Letter Queue 란?

메시지 큐에서 전달되지 못한 메시지를 관리하는 큐. retry를 계속해서 실패해서 메시지가 더 이상  유효하지 않게 되면 해당 메시지는 poison message로 취급한다
메시지를 어떠한 이유로 처리할 수 없는 경우엔 Dead Letter Queue(토픽)로 보낸다. 아래와 같은 이유로 메시지 처리가 실패할 수 있다
- 메시지를 deserialize할 수 없는 경우
- 데이터가 예상과 다른 경우 (예를 들어 값이 항상 양수인 필드에 음수가 들어간 경우)

 
## Logstash DLQ

Logstash 데이터 처리 과정에서 데이터의 mapping 에러와 기타 다른 이슈들이 발생하게 되면, 데이터 유실이 발생하게 됩니다. 
하지만 dead letter queue 기능을 사용하면 데이터 처리 과정에서 문제가 발생한 데이터를 별도 queue 과정을 적용 시킬 수 있습니다.

dead letter queue는 아직까지는 Elasticsearch output에서만 지원됩니다. 또한 응답 코드가 400 또는 404을 가지는 도큐먼트를 위해 사용 됩니다.

Dead Letter Queue에 써지는 각 데이터는 원본 데이터, 메타 데이터, 플러그인 정보를 포함하게 됩니다. 
메타 데이터에는 데이터가 처리되지 못한 이유가 포함되어 있으며, 플러그인 정보는 이벤트가 DLQ에 기입된 시간을 포함하게 됩니다.

dead letter queue의 이벤트를 처리하기 위해서는 dead_letter_queue input plugin을 사용해야하며, 해당 플러그인을 통해 DLQ의 이벤트를 읽을 수 있습니다.
