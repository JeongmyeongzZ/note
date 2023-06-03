# half-closed connection

TCP 연결 종료시 소켓의 완전종료가 일어나는데 (linux 의 close 나 window 의 closesocket), 완전 종료라는 것은 데이터의 전송 뿐 아니라 수신까지 모두 종료된 상황을 의미한다.

서버가 보낸 데이터를 완전히 수신하지 않았음에도 종료가 일어나게 된다면 모든 데이터를 수신하지 못하게 되는데, 이런 문제를 해결하기 위해 일부 종료 (half closed) 방식이 제공된다.

데이터를 송수신하는 두 호스트가 연결 된 경우를 stream 이 형성된 상태라고 말하는데, 이 스트림은 한 방향으로만 소통 가능하며, 송 수신이 모두 필요한 경우 두개의 스트림이 형성 된다.

일부 종료 (half closed) 는 이 중 하나만 종료하는 방식.

---

## NoHttpResponseException 가 자꾸 발생한다 (Apache HttpClient)

이 예외는 클라이언트가 서버로 요청을 보냈을 때, 서버가 응답을 주지 않거나 연결이 끊어진 경우에 발생합니다.

NoHttpResponseException 예외는 주로 Idle 상태의 연결이 서버로부터 연결이 닫혔음을 나타냅니다. 예를 들어, 클라이언트가 서버로 요청을 보내고 일정 시간 동안 서버로부터 응답이 없는 경우, HttpClient는 Idle 연결을 닫고 NoHttpResponseException을 발생시킵니다. 이는 클라이언트가 다음 요청을 위해 새로운 연결을 만들어야 함을 나타냅니다.


Handling way
- 예외를 재시도: NoHttpResponseException이 발생하면, 클라이언트는 요청을 다시 시도할 수 있습니다. 이를 통해 일시적인 네트워크 문제나 서버 문제를 극복할 수 있습니다. 하지만 너무 많은 재시도를 하는 경우에는 성능 문제가 발생할 수 있으므로 적절한 재시도 전략을 사용해야 합니다.
- 연결 풀 관리: NoHttpResponseException은 주로 Idle 상태의 연결이 닫혔을 때 발생합니다. 이를 방지하기 위해 HttpClient의 연결 풀 관리 설정을 확인해야 합니다. 연결 풀은 클라이언트와 서버 간의 연결을 관리하며, 일정 시간 동안 사용되지 않은 Idle 연결을 제거하고 새로운 연결을 만들어야 합니다. evictIdleConnections 설정을 사용하여 연결 풀 관리를 조정할 수 있습니다.
- 예외 처리: NoHttpResponseException이 발생했을 때, 적절한 예외 처리 로직을 구현해야 합니다. 이는 클라이언트 애플리케이션의 요구사항에 따라 달라질 수 있습니다. 예를 들어, 로깅하거나 예외를 상위로 전파하는 등의 방식으로 처리


evictIdleConnections는 HttpClient에서 제공하는 설정 중 하나로, 일정 시간 동안 사용되지 않은(Idle) 연결을 제거하는 기능을 제어합니다.
```java
CloseableHttpClient httpClient = HttpClientBuilder.create()
    .setConnectionTimeToLive(30, TimeUnit.SECONDS) // 연결 유지 시간 설정
    .evictIdleConnections(60, TimeUnit.SECONDS) // Idle 연결 제거 주기 설정
    .build();
```

위의 예시에서 setConnectionTimeToLive은 연결 유지 시간을 설정하는 부분입니다. 여기서는 30초로 설정되었습니다. 따라서 클라이언트가 서버와의 통신을 마치고 30초 동안 새로운 요청을 보내지 않는다면, 해당 연결은 Idle 상태가 됩니다.

evictIdleConnections 설정은 Idle 상태의 연결을 제거하는 주기를 설정하는 부분입니다. 위의 예시에서는 60초로 설정되었습니다. 즉, 60초 동안 사용되지 않은 Idle 상태의 연결은 자동으로 제거됩니다.

이렇게 evictIdleConnections 설정을 사용하면, HttpClient는 일정 시간 동안 사용되지 않은 Idle 연결을 주기적으로 검사하고 제거합니다. 이를 통해 리소스 낭비를 방지하고, 최적의 성능과 안정성을 유지할 수 있습니다.

---

결제사 (PG) 간 통신 오류를 해결하며..

half-closed connection 이슈인데요
http keep-alive로 인해 connection을 유지하면서 재사용을 하지만, 이 connection에 대한 idle timeout이 서로 달라서 발생하는 문제 입니다.
가령 server에 설정된 커넥션 유지시간이 짧아서 먼저 끊어버렸는데, client에서는 이를 감지하지 못하기 때문에 여전히 사용 가능한 connection이라 판단하게 되고, 이를 사용하다가 NoHttpResponseException예외가 발생하는 케이스 입니다.
사용 가능한 connection이 아니기 때문에 server로는 요청이 안갔을 것 같아요.
`해결 방법으로는 server에 설정된 값보다 더 작은 값으로 설정하는 방법이 있는데요`
사용하는 HttpClient의 종류에 따라 차이가 있겠지만, Apache HttpClient를 사용하는 경우에는 evictIdleConnections 값을 조정할 수 있습니다.


참고
- https://hc.apache.org/httpcomponents-client-4.5.x/current/httpclient/apidocs/org/apache/http/impl/client/HttpClientBuilder.html#evictIdleConnections(long,%20java.util.concurrent.TimeUnit)
