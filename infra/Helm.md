## helm

Kubernetes 패키지 관리를 도와줍니다. 흔히 패키지 관리를 도와주는 Node.js의 npm과 Python의 pip와 같은 역할이라고 보면 됩니다.

### Chart(차트)

헬름 패키지로, k8s cluster에서 애플리케이션이 기동되기 위해 필요한 모든 리소스들이 포함되어 있습니다.

### Repository(저장소)

차트 저장소로, 차트를 모아두고 공유하는 장소입니다.

### Release(릴리즈)

k8s cluster에서 구동되는 차트 인스턴스입니다. 일반적으로 동일한 Chart를 여러 번 설치할 수 있고 이는  새로운 Release로 관리되게 됩니다. Release될 때 패키지된 차트와 Config가 결합되어 정상 실행되게 됩니다.
 

> k8s cluster 내부에 Helm Chart를 원하는 Repository에서 검색 후 설치 ▶ 각 설치에 따른 새로운 Release 생성
