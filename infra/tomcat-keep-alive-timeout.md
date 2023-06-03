# keep alive ?

HTTP/1.1부터는  이미 연결되어 있는 TCP 연결을 재사용, 유지 (persistent connection) 하는 Keep-Alive 라는 기능을 Default 로 지원한다. Handshake 과정이 생략

사용 안하면 `Connection to {host} closed by foreign host.`

하면 `Keep-Alive: timeout=5, max=99` 이런식의 응답을 볼 수 있음

이미지와 같이 여러 요청을 계속해서 보내는 상황 (site locality) 에서 큰 성능 향상이 가능

- network connection
- network resource
- network latency

---

# tomcat keepalive timeout 

기본적으로 Tomcat 은 Keep-Alive 연결을 2분 동안 유지하도록 설정되어 있습니다.

property 에 `server.tomcat.connection-timeout=60000` 와 같이 간단하게도 선언이 가능 (boot 2.2)

502 Bad Gateway 가 서비스간에 종종 발생할 때가 있는데, 이 때 해당 설정과 관련이 있을 수 있다.

Keep-Alive는 클라이언트와 서버 간의 연결을 재사용하여 성능을 향상시키는 메커니즘입니다. 서비스 메시나 프록시 환경에서는 일반적으로 프록시 서버가 클라이언트 요청을 받아 백엔드 서비스로 전달합니다. 이 때, 프록시 서버와 백엔드 서비스 사이의 연결에 Keep-Alive가 사용될 수 있습니다.

502 Bad Gateway 오류는 주로 다음과 같은 상황에서 발생할 수 있습니다:

1. 프록시 서버 문제: 프록시 서버에서 발생하는 오류로 인해 백엔드 서비스로의 요청이 실패하는 경우입니다. 
   이는 프록시 서버가 백엔드 서비스와의 연결을 제대로 수립하지 못한 경우나, 유효한 응답을 받지 못한 경우에 발생할 수 있습니다.
2. Keep-Alive 설정: 프록시 서버와 백엔드 서비스 간의 Keep-Alive 설정이 일치하지 않는 경우, 502 Bad Gateway 오류가 발생할 수 있습니다. 
   예를 들어, 클라이언트와 프록시 서버 간의 Keep-Alive Timeout 값이 다르게 설정되어 있거나, 프록시 서버와 백엔드 서비스 간의 Keep-Alive Timeout 값이 다르게 설정되어 있는 경우에 문제가 발생할 수 있습니다.

gateway (proxy) 서비스보다 뒷단의 서비스의 keep alive timeout 설정을 조금 더 길게 가져간다거나 하면 해당 문제 해결에 도움이 될 수 있음.


너무 긴 timeout 시간이 가져올 수 있는 문제

- 확장성과 부하 분산: 서비스 메시 환경에서는 서비스 인스턴스의 수가 동적으로 확장 및 축소될 수 있습니다. 
Keep-Alive Timeout 값을 너무 크게 설정하면 연결 유지에 대한 자원 소모가 늘어나며, 확장성과 부하 분산에 영향을 줄 수 있습니다. 
따라서 서비스 메시 환경에서는 적절한 Keep-Alive Timeout 값을 선택해야 합니다.

- 자원 소모: Timeout 값이 너무 길면 서버 자원이 오래 사용될 수 있습니다. 예를 들어, 서버는 연결을 유지하기 위해 메모리, 스레드, 소켓 등을 사용합니다. 따라서 많은 연결이 동시에 유지되고 Timeout 값이 너무 길면 서버 자원이 고갈될 수 있습니다.

- Scalability 저하: Keep-Alive Timeout 값이 너무 크면 서버가 동시에 처리할 수 있는 연결 수가 제한됩니다. 연결이 유지되는 동안 해당 연결은 서버에 대한 리소스를 점유하게 되므로, 많은 연결이 유지되는 경우 새로운 연결을 처리하는 데 시간이 더 많이 소요되고 서버의 확장성이 저하될 수 있습니다.

- 장애 복구 시간: Timeout 값이 너무 길면 장애 발생 시 해당 연결의 복구 시간이 오래 걸릴 수 있습니다. 예를 들어, 네트워크 연결이 끊어진 경우 클라이언트와 서버 사이의 연결을 끊기까지 많은 시간이 소요될 수 있습니다. 이는 장애 복구 시간을 증가시키고 사용자 경험에 부정적인 영향을 미칠 수 있습니다.

- 리소스 낭비: Timeout 값이 너무 길면 사용자가 더 이상 해당 연결을 사용하지 않는 경우에도 연결이 계속 유지될 수 있습니다. 이는 리소스의 낭비로 이어지며, 서버의 성능과 확장성에 부정적인 영향을 미칠 수 있습니다.