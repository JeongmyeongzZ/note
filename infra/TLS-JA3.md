## TLS (Transport Layer Security):

TLS는 SSL의 후속 프로토콜로, 더 강력한 보안 기능을 제공합니다.
TLS는 데이터의 기밀성과 무결성을 보호하기 위해 암호화를 사용하며, 상호 인증을 통해 통신 상대방의 신원을 확인합니다.
현재 대부분의 웹 브라우저와 웹 서버에서 TLS가 사용되고 있습니다.

## SSL (Secure Sockets Layer):

SSL은 처음에 개발된 보안 프로토콜로, 데이터 전송 중에 암호화와 인증을 제공합니다.
하지만 취약점이 발견되어 TLS로 대체되었습니다.

## HTTPS (Hypertext Transfer Protocol Secure):

HTTPS는 HTTP의 보안 버전으로, 안전한 통신을 위해 TLS 또는 SSL을 사용합니다.
HTTPS를 사용하면 데이터가 암호화되어 전송되므로 중간에서 데이터를 가로채더라도 해독이 어려워집니다.

## JA3 Fingerprint:

JA3 fingerprint는 TLS 클라이언트의 특성을 나타내는 문자열입니다.
TLS 클라이언트는 여러 속성을 가지고 있으며, 이 속성들은 JA3 fingerprint에 반영됩니다.
JA3는 클라이언트의 TLS 버전, 지원하는 암호화 알고리즘, 확장 등을 포함한 다양한 정보를 담고 있습니다.

<br>

JA3 fingerprint는 TLS 클라이언트의 특징을 나타내기 때문에, TLS 통신에서 JA3 fingerprint를 사용하면 클라이언트를 식별하고 추적하는 데 도움이 됩니다.
보안 전문가들은 JA3 fingerprint를 사용하여 특정한 악성 트래픽이나 비정상적인 행위를 탐지하고 분석하는 데 활용합니다.
TLS에서 JA3 fingerprint를 사용하면, 클라이언트의 특징을 기반으로 보다 정확한 보안 판단과 모니터링이 가능해집니다.

TLS 연결은 TLS Handshake로 알려진 일련의 순서를 사용하여 초기화됩니다. 사용자가 TLS를 사용하는 웹 사이트를 돌아다니면 사용자 장치와 웹 서버 간에 TLS Handshake가 시작됩니다.

TLS Handshake 동안 사용자 장치와 웹 서버는 다음과 같은 일을 수행합니다.

- 사용할 TLS 버전(TLS 1.0, 1.2, 1.3 등)을 지정합니다
- 사용할 암호 제품군을 결정합니다
- 서버의 TLS 인증서를 사용하여 서버의 신원을 인증합니다.
- Handshake가 완료된 후 키 간의 메시지를 암호화하기 위한 세션 키를 생성합니다.

<br>
<br>

_https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/_
_https://moonsiri.tistory.com/180_
