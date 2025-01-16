## Infra

### **1. 가상화가 무엇이고, 이것이 가상머신과 어떠한 차이가 있는지 설명해 주세요.**
A. 가상화란 물리적인 하드웨어 자원을 소프트웨어적으로 분리 및 추상화하여,
하나의 하드웨어에서 여러 개의 운영 체제나 애플리케이션을 동시에 실행할 수 있게 하는 기술이다.
- 가상화(Virtualization)
  - 개념: 하드웨어 자원을 가상으로 나누어 여러 환경을 실행할 수 있게 하는 기술.
  - 방식: 하이퍼바이저(Hypervisor)를 사용하여 하드웨어를 추상화함.
  - 예: VMware, Hyper-V, KVM
- 가상머신(Virtual Machine)
  - 개념: 가상화 기술을 사용하여 운영 체제와 애플리케이션을 완전히 격리한 독립 환경.
  - 특징: 각 VM은 게스트 OS를 포함하며, 독립된 커널을 가짐.
  - 예: VirtualBox, VMware.

- **그렇다면 Docker는 둘 중 어디에 속하나요? 왜 사람들이 Docker를 많이 채택할까요?**
A.Docker는 컨테이너 가상화(Container Virtualization) 기술에 속한다.
경량화, 빠른 배포, 이식성, 마이크로서비스 아키텍처(MSA)에 적합 등의 이유로 Docker 를 많이 채택한다.
- VM과의 차이점
  - 가상머신(VM): 하이퍼바이저 기반, 게스트 OS 포함.
  - Docker(컨테이너): 호스트 OS의 커널 공유, 더 가볍고 빠름.

- **하나의 Host OS에서 돌아간다면 충분히 한 컨테이너가 다른 컨테이너에 간섭할 수 있는 위험이 있지 않을까요? 이를 어떻게 방어할 수 있을까요?**
A. 서로 간섭할 위험이 발생할 가능성이 있다. Docker 컨테이너는 커널(Kernel, 하드웨어-소프트웨어 간의 중재자 역할을 하는 운영 체제의 핵심 구성 요소)을 공유하기 때문에, 보안 문제가 발생할 수 있다.
- 방어 방법:
  - Namespace: 각 컨테이너에 고유한 프로세스 공간을 부여.
  - Cgroups(Control Groups): 리소스 사용량 제한 (CPU, 메모리).
  - AppArmor, SELinux: 추가적인 보안 레이어 적용.
  - User Namespace: 호스트 사용자와 컨테이너 사용자를 분리하여 권한 관리.

- **Docker 위에 Docker를 올릴 순 없을까요?**
A. 가능하다. 이를 Docker in Docker 라고 부른다. (DinD)
- 사용 사례: CI/CD 파이프라인(Jenkins 등)에서 사용.
- 주의점
  - 보안 위험: 호스트의 Docker 소켓이 노출될 수 있다.
  - 성능 오버헤드: 중첩된 가상화로 인해 성능 저하가 발생할 수 있다.

- **Docker Engine 은 무엇인가요?**
A. Docker Engine 은 Docker 의 핵심 구성 요소로, 컨테이너의 생성을 관리하는 런타임(runtime)이다.
- 구성 요소
  - Docker Daemon (데몬): 백그라운드에서 컨테이너 관리.
  - Docker CLI (커맨드라인 툴): 사용자가 Docker 를 조작할 수 있는 CLI 툴.
  - REST API: 프로그램 간 Docker 조작을 위한 API.
- 역할
  - 컨테이너 생성, 시작, 정지, 삭제 관리.
  - 이미지 빌드 및 저장소(Pull/Push) 기능.
  - 리소스 격리 및 관리 (Namespace, Cgroups).


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
A. Git 이란 분산 버전 관리 시스템(DVCS)으로, 소스 코드의 변경 이력을 효율적으로 관리하고
여러 개발자가 동시에 협업할 수 있게 도와주는 도구이다.
1. 버전 관리: 코드 변경 내역을 커밋(commit) 단위로 저장
2. 분산 저장소: 로컬과 원격 저장소를 각각 관리
3. 브랜칭 및 병합: 여러 브랜치를 생성하고 독립적으로 개발 후 병합 가능
4. 협업 기능: 원격 저장소(GitHub, GitLab)를 활용한 협업 지원

- **여러 브랜치를 합쳐야 할 때, 어떤 방법을 사용할 수 있는지 "모두" 설명해 주세요.**
1. Merge(머지): 두 브랜치를 그대로 유지하면서, 새로운 커밋을 생성해 브랜치를 병합.
  - 기능 개발 완료 후 메인 브랜치에 통합할 때 사용한다.
  - 이력을 보존할 수 있으나 커밋 이력이 복잡해질 수 있다.
    ```bash
    git checkout main
    git merge feature-branch
    ```
2. Rebase(리베이스): 브랜치를 병합할 때 새로운 커밋 히스토리를 만들고 기존 커밋을 재정렬.
  - 깨끗한 커밋 히스토리를 유지하고 싶을 때 사용한다.
  - 커밋 이력이 깔끔해지지만 충돌이 많을 경우 복잡해질 수 있다.
    ```bash
    git checkout feature-branch
    git rebase main
    ```
3. Squash Merge(스쿼시 머지): 여러 커밋을 하나의 커밋으로 합친 후 병합.
  - 브랜치 내 여러 커밋을 하나로 합쳐 기록을 간결하게 만들고 싶을 때 사용한다.
  - 커밋 히스토리가 단순해지지만 세부 이력이 사라질 수 있다.
    ```bash
    git checkout main
    git merge --squash feature-branch
    git commit -m "Merged feature-branch"
    ```
4. Cherry-Pick(체리픽): 특정 커밋만 선택적으로 다른 브랜치에 반영.
  - 특정 기능이나 버그 수정 커밋만 선택적으로 반영할 때 사용한다.
  - 특정 커밋만 선택할 수 있지만 커밋 순서가 바뀔 수 있다.
    ```bash
    git checkout main
    git cherry-pick <commit-hash>
    ```
5. Fast-Forward Merge(패스트 포워드 머지): 브랜치가 직선 형태일 때 별도 커밋 없이 브랜치를 앞으로 이동.
  - 병합 대상 브랜치가 최신상태일 때 사용한다.
  - 커밋 이력을 깔끔하게 관리할 수 있지만 브랜치 이력이 남지 않는다.
    ```bash
    git checkout main
    git merge feature-branch --ff
    ```
`정리`
- 기능 개발 완료 후 병합 → `Merge`
- 커밋 히스토리를 정리하고 싶을 때 → `Rebase`
- 여러 커밋을 하나로 합칠 때 → `Squash Merge`
- 특정 커밋만 반영하고 싶을 때 → `Cherry-Pick`
- 직선 병합이 가능할 때 → `Fast-Forward Merge`
