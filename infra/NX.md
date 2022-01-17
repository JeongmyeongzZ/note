monorepo란, 다양한 모듈을 여러개의 repository로 관리하지 않고, 하나의 repository에서 패키지 등으로 분기하여 관리하는 것을 의미합니다.

예를 들면, Node.js를 활용한 디바이스별로 서버를 분리한다거나, 템플릿을 분리하는 용도 등이 포함 될 수 있습니다. 때로는, 서비스별로 monorepo를 구성할 수 있을 것 같습니다.

react application은 물론이며, nextjs, gatsby 프로젝트도 지원하며, cypress등을 이용한 e2e, Jest 및 testing-library를 이용한 테스팅 및, storybook까지 지원하니 typescript 기반으로 React 프로젝트를 진행한다면 대부분의 인프라를 CLI로 설정하여, 사용할 수 있게 됩니다. 


마이크로 프론트엔드 또한 MSA 환경에서 발전하면서, 이를 통합하는 환경, 또는 방법이 필요해졌다.

로직 및 컴포넌트 재사용, 개발 환경 설정, 스키마 동기화, 지속적 통합 등과 같은 문제를 해결해야 하는 문제가 항상 생긴다. 프론트 특성상 일관 된 UX, UI 그리고 컴포넌트 기반의 개발 때문에 더 힘들 것 같다.

마이크로 프론트엔드 아키텍쳐와 코드베이스 분리를 하게 되면, 단순히 코드를 모아 두고 공유하는 것만으로는 충분하지 않다. 

모노레포 안에서도 각 라이브러리의 도메인을 설정하고, 라이브러리의 종류에 따라 의존 관계를 제한하고, 라이브러리 사이의 의존성을 확인하는 등 어려운 해결해야하는 문제들이 많다.

Nx라는 친구는이 문제들을 해결하는데 도움을준다.

> [monorepo ?](https://class101.dev/ko/blog/2019/07/12/tony/)


_참고_
- _https://medium.com/@trustyoo86/nx%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-react-monorepo-%EB%A7%8C%EB%93%A4%EA%B8%B0-workspace-%EB%A7%8C%EB%93%A4%EA%B8%B0-a53cd5374bcd_
- _https://mobicon.tistory.com/572_
- _https://metleeha.tistory.com/entry/BFFBackend-for-Frontend-%EB%9E%80_
- _https://injungchung.com/enterprise-frontend-application-architecture_
