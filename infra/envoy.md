## envoy

> https://bcho.tistory.com/1295

php 같이 connection pool 이 지원되지 않는 스크립트 언어 기반 서비스에서
envoy 같은 L7 레이어의 포워드 프록시 서비스를 도입해서 connection pooling 을 구현할 수 있다.

Envoy는 프록시 레벨에서 서비스 디스커버리, 로드 밸런싱, 보안, 로깅 등 다양한 기능을 제공하며, 데이터베이스 연결 풀링은 주로 애플리케이션 레벨에서 수행되기 때문에 Envoy 자체에서 Redis 연결 풀링을 직접 구현하지는 않습니다.


