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
- MSA 구성시 필요한 Netflix와는 다르게 플랫폼 영역에서 동작하기 때문에 개발언어와 무관하게 사용이 가능하다.
- 쿠버네티스, 클라우드 기반에 적합하다.

## istio의 필요성
- gateway 및 proxy 대체 가능하다.
- 장애대응 및 트래픽제어를 정말 효율적으로 관리할 수 있음
- 유레카랑 비교해서 비즈니스 로직에 집중할 수 있는 환경을 만들어준다는 것이 큰 장점인 것 같다.

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

## 이러한 서비스메쉬를 적용하는 방법
1) REST API 호출 방식으로 서비스를 결합시키는 스프링 클라우드(Spring Cloud) 기반 넷플릭스 OSS
2) 쿠버네티스(Kubernetes) 기반 클라우드 네이티브(Cloud Native) 서비스
3) 사이드카 프록시(Sidecar Proxy) 패턴의 쿠버네티스 기반 이스티오(Istio) 솔루션
- 스프링 클라우드 유레카를 기반으로 구성할 수도있고 Istio 솔루션을 적용해 볼수도 있는것 같다.
<br/><br/>
- 출처: https://www.samsungsds.com/kr/insights/1239180_4627.html

### 스프링 클라우드 기반 유레카
- 유레카는 단위 서비스들에 대하여 동적으로 서비스 레지스트리 및 서비스 디스커버리를 수행(EurekaClient - EurekaServer 관계)하고 서비스간 통신의 부하 분산 기능을 제공한다.
    <br/>
    ![image](https://user-images.githubusercontent.com/38865267/155261666-994ad9c4-da23-4b74-9990-4e8f457ba3dc.png)

- 애플리케이션이 시작될 때, 유레카 서버의 레지스트리에 상태 정보를 등록한다.
- 각 서비스는 설정된 간격(기본 30초)마다 하트비트 방식으로 상태 정보를 유레카 서버로 전송해 Health check를 한다.
- 하트비트가 수신되지 않는다면 레지스트리에서 해당 서비스가 제거된다.
- 유레카에서 부하의 분산은 Client-side 방식이며 리본(Ribbon)을 사용한다.

### 사이드카 프록시 패턴으로 서비스 메쉬 구현
- 애플리케이션 서비스 앞단에 프록시를 배치하는 디자인
- 애플리케이션을 수정하지 않고도 서비스간 통신에 해당하는 추가 기능을 수행할 수 있다.
    <br/>
    ![image](https://user-images.githubusercontent.com/38865267/155261989-c984d89a-7210-43f8-88b6-7e4ef4b6aa08.png)

- 위 그림은 대표적인 사이드카방식의 오픈소스 구현체이며 쿠버네티스 기반의 솔루션인 이스티오(Istio)
- Data Plain에서 Envoy가 사이드카로 배포되어 서비스의 인/아웃 통신 트래픽을 제어한다.
- 서비스 디스커버리 수행 방식은 Control Plain의 Pilot이 서비스 레지스트리 정보를 가지고 있고 Envoy가 해당 정보를 참고해 다른 서비스의 엔드포인트로 접근하게 된다.
- Mixer는 Pilot을 이용하여 서비스 버전별 트래픽 양을 분산 혹은 콘텐츠에 따라 트래픽을 분할 전송한다. 또 서비스를 디스커버리하고 로드밸런싱한다.(유레카에 대입해보면 둘이 합쳐서 EurekaServer의 역할, Pliot 자체는 config서버의 역할을 하는듯한 느낌이다.)
- Mixer는 접근 정책에 대한 통제와 서비스 모니터링을 위한 Metric 지표를 수집한다. 또한, 서비스 상태를 주기적으로 체크하여 비정상적인 인스턴스는 자동제거한다.(유레카의 Health check와 흡사한듯)
- 여기서 Jaeger를 이용하면 분산 트랜잭션 구간별로 응답 시간을 모니터링 할 수 있다는데, MSA간 트랜잭션 부분도 추가 공부가 필요할 듯 하다.
- Citadel은 인증, 인가와 같은 보안을 담당하며 TLS 인증서를 관리한다.
- 사이드카 프록시 패턴은 각 서비스별로 프록시가 필요하기에 서비스 인스턴스가 2배로 소요되지만 프록시에 공통기능을 별도로 구현해서 서비스 별 비즈니스 로직 구현에 집중할 수 있는 이점을 가진다.