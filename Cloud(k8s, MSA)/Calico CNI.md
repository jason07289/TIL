# Calico Container Network Interface
- 컨테이너, 가상 머신 및 기본 호스트 기반 워크로드를 위한 오픈 소스 네트워킹 및 보안 솔루션
- K8s, OpenShift 및 베어메탈 서비스를 포함한 광범위한 플랫폼을 지원
- 클라우드 네이트브 확장성과 함께 빠른 성능을 제공하며 퍼블릭 클라우드나 온프레미스, 단일 노드 또는 수천개의 노드 클러스터에서 실행되는지의 여부에 관계없이 개발자와 클러스터 운영자에게 일관된 경험과 기능을 제공한다.

## 서드파티 CNI 플러그인을 쓰는 이유
- 기본제공되는 kubenet의 기능이 너무 부족, kubenet은 컨테이너에서 노드간 교차 네트워킹도 지원이 되지 않는다.

## CNI 플러그인의 네트워크 모델
- VXLAN(Virtual Extensible LAN)나 IP-in-IP 프로토콜을 사용하는 오버레이 네트워크 모델
- BGP(Border Gateway Protocal)을 사용하는 비-오버레이 네트워크 모델

### 오버레이 네트워크 모델
- 엔드포인트 간의 네트워크 구조를 추상화하여 네트워크 통신 경로를 단순화
- 3계층을 넘어서 구축된 네트워크 간에 있는 엔드포인트의 노드간 통신이 일어날 때 패킷을 한겹 캡슐화 하여 통신시켜서, 2계층(LAN)에서 통신이 일어나는 것처럼 통신할 수 있도록 하는 기술.