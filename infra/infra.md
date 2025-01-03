## Infra

### **1. 가상화가 무엇이고, 이것이 가상머신과 어떠한 차이가 있는지 설명해 주세요.**

- **그렇다면 Docker는 둘 중 어디에 속하나요? 왜 사람들이 Docker를 많이 채택할까요?**
- **하나의 Host OS에서 돌아간다면 충분히 한 컨테이너가 다른 컨테이너에 간섭할 수 있는 위험이 있지 않을까요? 이를 어떻게 방어할 수 있을까요?**
- **Docker 위에 Docker를 올릴 순 없을까요?**
- **Docker Engine 은 무엇인가요?**

### **2. CI/CD 를 사용해 본 경험이 있나요? 있다면 간단하게 설명해 주세요.**
- CI (Continuous Integration, 지속적 통합): 개발자가 작성한 코드를 중앙 저장소에 자주 병합하며, 자동화된 빌드와 테스트를 통해 코드의 품질을 지속적으로 검증하는 프로세스.
  - 주요 목표: 코드 충돌 방지, 문제 조기 발견, 개발 속도 향상
- CD (Continuous Delivery/Deployment, 지속적 배포/전달)
  - Continuous Delivery: 코드가 빌드 및 테스트를 거쳐 프로덕션에 배포할 준비가 된 상태를 자동화
  - Continuous Deployment: 배포 과정까지 완전히 자동화하여 코드 변경이 즉시 프로덕션에 반영

A. 저는 CI/CD 환경을 GitHub Actions, Docker, AWS ECR를 활용해 구축한 경험이 있습니다.
프로젝트는 MSA(Microservices Architecture) 구조로 구성되어 있었으며, 각 서비스는 API Gateway 를 통해 연결하였으며,
컨테이너 기반의 Kubernetes(Pod)를 사용해 배포했습니다.
  - GitHub Actions
    - push 또는 pull request 이벤트 발생 시 GitHub Actions 가 트리거되도록 설정
    - 각 서비스별로 빌드, 테스트, Docker 이미지 생성, ECR 에 푸시하는 워크플로우 작성
  - Docker 및 AWS ECR 을 활용한 이미지 관리
    - Docker 로 각 서비스 별 이미지 생성 후, AWS ECR 에 저장
  - API Gateway 및 MSA 연결
    - API Gateway를 통해 각 서비스(Pod) 간의 통신을 설정
    - 서비스별로 분리된 컨테이너 이미지를 Kubernetes 환경에서 실행하고, Gateway를 통해 엔드포인트를 통합 관리

GitHub Actions와 Docker의 자동화를 통해 수작업 배포 과정을 제거하여 효율성을 향상시켰으며,
빌드와 테스트를 자동화하여 코드 품질을 유지하며 안정성을 확보하였습니다.
또, MSA 구조와 Kubernetes 를 활용해 개별 서비스의 독립 배포 및 확장성을 확보해 코드의 유연성을 강화하였습니다.

### **3. Web Server, Web Application Server 의 차이는 무엇인가요?**
1. Web Server (웹 서버)
- 정적 컨텐츠 제공: HTML, CSS, JavaScript, 이미지, 동영상 등 정적 리소스를 클라이언트(브라우저)에 제공.
- 주요 역할
  - HTTP 요청 수락 및 정적 파일 제공.
  - 리버스 프록시 역할 (요청을 다른 서버로 전달).
  - ex) Nginx, Apache HTTP Server.
2. Web Application Server (WAS, 웹 애플리케이션 서버)
- 동적 컨텐츠 처리: 데이터베이스 연동, 비즈니스 로직 수행, API 호출 등 동적 웹 애플리케이션 처리.
- 주요 역할
  - 클라이언트 요청을 받아 비즈니스 로직 수행.
  - 데이터베이스 연동 및 API 호출.
  - 서버 사이드 렌더링(SSR) 가능.
  - ex) Tomcat, Jetty, WildFly.

A. 웹 서버는 정적 리소스 제공에 특화되어 있고, WAS 는 동적 데이터와 비즈니스 로직처리에 특화되어 있다.


### 4. Git에 대해 설명해 주세요.

- **여러 브랜치를 합쳐야 할 때, 어떤 방법을 사용할 수 있는지 "모두" 설명해 주세요.**
- **여러 브랜치를 합쳐야 할 때, 어떤 방법을 사용할 수 있는지 "모두" 설명해 주세요.**