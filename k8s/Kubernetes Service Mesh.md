# Service Mesh

## Service Mesh란?
- MSA에서 서비스간의 통신을 제어하고 관리할 수 있도록 하는 데에 특화된 인프라 계층
- 기존의 모놀리식에서의 호출방식은 직접 호출이라면 서비스 메시에서의 호출은 자체 인프라 계층의 Proxy를 통해 이뤄지게 된다.
- 개별 proxy는 서비스 내부가 아니라 각 서비스와 함께 실행되기에 sidecar 형식으로 올라온다.
- 이러한 sidecar들이 모여서 Mesh 네트워크를 형성한다.

## Service Mesh의 필요성
- 서비스 메시는 컨테이너 오케스트레이터 위에 별도의 계층으로 배포해 서비스 간 통신을 안전하고 빠르게 만드는 전용 인프라 계층이다.
- API 게이트웨이 같은 경우에는 외부의 트래픽을 받아들이고 내부적으로 배포하기 위해 설계되었다.(중앙 프록시를 통해 트래픽이 이동)
- 서비스 메시는 내부 트래픽을 관리하므로 API 게이트웨이와 서비스 메시를 적절히 섞어서 활용해야 한다.

## Service Mesh로 얻는 이점
- 서비스 간 안정적인 연동이 되므로 개발자들이 비즈니스에 집중 가능
- Jaeger를 통한 요청의 분산 추적은 서비스와 함께 가시적인 인프라 계층을 제공
- 서비스 메시는 장애가 발생한 서비스로부터 요청을 재 라우팅이 가능해 장애 복구 능력이 있음
- 성능 매트릭을 통해 런타임 환경에서 커뮤니케이션 최적화 방법을 찾을 수 있다.

## istio
- MSA의 분산 네트워크 환경 즉, k8s에서 각 app들의 네트워크 연결을 쉽게 설정할 수 있도록 지원하는 기술이며, 서비스에 대한 연결, 보안, 제어 및 모니터링이 가능
- 동적 서비스 발견, 로드 밸런싱, TLS 처리, HTTP/2 및 gRPC 프록싱, 서킷 브레이커, 헬스 체크, 백분율 기반 트래픽 분할을 통한 단계적 출시, 결함 주입 및 풍부한 메트릭 등의 Envoy 기능을 활용

## istio의 필요성
- gateway 및 proxy 대체 가능하다.
- 장애대응 및 트래픽제어를 정말 효율적으로 관리할 수 있음

## MSA환경에서 Service Mesh의 주요기능
- Configuration Management: 서비스의 재빌드·재부팅 없이 설정사항을 반영(Netflix Archaius, Kubernetes Configmap)
- Service Discovery: MSA 기반 서비스 배포 시 서비스 검색 및 등록(Netflix Eureka, Kubernetes Service, Istio)
- Load Balancing: 서비스 간 부하 분산(Netflix Ribbon, Kubernetes Service, Istio)
- API Gateway: 클라이언트 접근 요청을 일원화(Netflix Zuul, Kubernetes Ingress)
- Service Security: 마이크로서비스 보안을 위한 심층 방어메커니즘 적용(Spring Cloud Security)
- Centralized Logging: 서비스별 로그의 중앙집중화(ELK Stack)
- Centralized Monitoring: 서비스별 메트릭 정보의 중앙집중화(Netflix Spectator, Heapster)
- Distributed Tracing: 마이크로서비스 간의 호출 추적(Spring Cloud Sleuth, Zipkin)
- Resilience & Fault Tolerance: MSA 구조에서 하나의 실패한 서비스가 체인에 연결된 전체 서비스들에 파급 효과를 발생시키지 않도록 하기 위한 계단식 실패 방지 구조(Netflix Hystrix, Kubernetes Health check)
- Auto-Scaling & Self-Healing: 자동 스케일링, 복구 자동화를 통한 서비스 관리 효율화
- Build/Deploy Automation: 서비스별 빌드·배포 자동화(Netflix Spinnaker, Kubernetes Scheduler, Jenkins)
- Test Automation: 서비스에 대한 테스트 자동화
- Image Repository: 컨테이너 기반 서비스 배포를 위한 이미지 저장소

## 이 서비스메쉬를 적용하는 방법
1) REST API 호출 방식으로 서비스를 결합시키는 스프링 클라우드(Spring Cloud) 기반 넷플릭스 OSS
2) 쿠버네티스(Kubernetes) 기반 클라우드 네이티브(Cloud Native) 서비스
3) 사이드카 프록시(Sidecar Proxy) 패턴의 쿠버네티스 기반 이스티오(Istio) 솔루션
- 스프링 클라우드 유레카를 기반으로 구성할 수도있고 Istio 솔루션을 적용해 볼수도 있는것 같다.