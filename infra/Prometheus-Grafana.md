# Prometheus

SoundCloud사에서 만든 오픈소스 시스템 모니터링 및 경고 툴킷 (2016년에는 쿠버네티스에 이어 두 번째로 CNCF(Cloud Native Computing Foundation) 산하 프로젝트 멤버로 들어가게 됐다.)
Grafana Labs에서 유지 보수를 메인으로 전담하고 있다.

## 특징
- 풀 방식의 메트릭 수집, 시계열 데이터 저장
- PromQL을 활용하여 저장된 시계열을 쿼리 및 집계
- 서비스 디스커버리
- 데이터 시각화


`Jobs/exporters` : 매트릭을 수집하는 프로세스

- HTTP 통신을 통해 매트릭 데이터를 가져갈 수 있게 `/metrics` 라는 HTTP 엔드포인트를 제공한다.
- Prometheus server가 이 exporter의 엔드포인트로 HTTP GET 요청을 날려 매트릭 정보를 수집(Pull)한다.
- 수집한 정보를 Prometheus가 제공하는 간단한 웹 뷰를 통해 조회할 수 있고 그 안에서 테이블 및 그래프 형태로 볼 수 있다.
  - 하지만 시각화 도구가 부족해서 이를 직접 사용하지는 않고 대게 Grafana 라는 Data Visualization tool을 이용하여 시각화하고 있다.


# Grafana

시계열 매트릭 데이터를 시각화 하는데 가장 최적화된 대시보드를 제공해주는 오픈소스 툴킷


시각화한 그래프에서 특정 수치 이상으로 값이 치솟을 때 (예를 들어 CPU 사용량 80% 이상) 알림을 전달받을 수 있는 기능도 제공

그리고 오픈소스인 만큼 커뮤니티도 많이 활성화 되어있는데, 일반 사용자들이 만들어놓은 대시보드를 import해서 사용할 수도 있고 import한 대시보드를 내 입맛에 맞게 커스터마이징 할 수도 있다.
또한 다양한 플러그인이 있어 Grafana 내부적으로 기능 확장도 쉽게 가능하다.

## vs Datadog

데이터독의 경우 데이터를 직접 저장하고 있는 것과 달리 그라파나는 외부 데이터 소스를 정의하고 해당 데이터 소스에 쿼리를 통해서 데이터를 동적으로 가지고 와서 시각화를 한다.

예를 들어 데이터독에서 AWS 서비스를 연동하면 클라우드 와치의 메트릭 데이터를 일정 주기로 가져와서 데이터독 데이터베이스에 저장해둡니다. 
이 때문에 데이터독을 사용하는 경우 클라우드와치의 실시간 데이터와 15분 정도의 약간의 시차가 발생합니다. 

반면에 그라파나는 AWS 클라우드 와치에 직접 쿼리해서 데이터를 가져오기 때문에 대시보드를 조회하는 동안 최신 데이터를 확인할 수 있습니다.
