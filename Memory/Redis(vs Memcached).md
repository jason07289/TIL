# Redis(Remote Dictionary Storage, 레디스) VS Memcached(맴캐시드)
- 오픈소스이며, 인메모리 데이터 저장소이다.
- 맴캐쉬드는 명료하고 단숨함을 위하여 개발.
- 레디스는 다양한 용도에 효과적으로 사용할 수 있도록 많은 특징을 가짐.

## 공통점
- 1ms 이하의 응답대기시간
  - 데이터를 메모리에 저장하기 때문에 디스크 기반의 데이터베이스보다 빠르게 데이터를 읽어온다.
- 개발의 용이성
  - 문법저긍로 사용하기 쉽고, 개발코드 양 또한 적다.
- 데이터 파티셔닝
  - 데이터를 여러 노드에 분산하여 저장시킬 수 있다. 데이터 양에 대한 수요가 증가할 때 더 많은 데이터를 효과적으로 처리하기 위해 스케일아웃이 가능하다.
- 다양한 프로그래밍언어 지원

## Memached만의 특징
- 멀티스레드를 지원하므로 멀티프로세스 코어를 사용할 수 있다.
- 스케일업을 통해 더욱 많은 작업처리가 가능하다.

## Redis만의 특징
1. 더 다양한 데이터 구조
   ![image](https://user-images.githubusercontent.com/38865267/155070565-a6820dd4-e353-4429-bda9-5421114798c1.png)

   - 문자열 뿐만아니라 List, Set, Sorted Set, Hash, Bit 배열, hyperloglogs (매우 적은 메모리로 집합의 개수를 추정할 수 있는 방법)을 지원
2. Snapshots
   - 레디스는 특정시점에 데이터를 디스크에 저장하여 파일 보관이 가능
   - 이는 장애복구 시에도 활용이 된다.
3. 복제
   - Master-Slave 구조로, 여러개의 복제본을 만들 수 있다.
   - 데이터 읽기가 확장가능하며 높은 가용성을 제공한다.(클러스터 형태로 구성이 가능해서 오랜시간동안 고장나지 않음)
4. 트랜잭션 특징 지원
5. pub/sub messaging
   - publish, subscribe 방식의 메시지 패턴 검색이 가능하다. 때문에, 높은 성능을 요구하는 채팅, 실시간 스트리밍, SNS 피드 그리고 서버상호통신에 사용이 가능하다.
6. 루아 스크립트 지원
   - 매우 경량화된 절차스크립트 언어인 루아 지원, 프로그램을 명료하게하고 성능을 높일 수 있다.
7. 위치기반의 데이터 타입 지원
   - 실시간 위치기반 데이터를 지원한다.
   - 두 위치의 거리를 찾거나, 사이에 있는 요소찾기 등의 작업을 수행할 수 있다.

## Redis 영속성
- Redis는 지속성을 보장하기 위해 데이터를 디스크에 저장이 가능하다. 만약 서버가 내려가도 데이터를 읽어서 메모리에 로딩할 수 있다.
- 두 가지 방식
  - RDB(Snapshotting) 방식
    - 순간적으로 메모리에 있는 내용 전체를 DISK에 옮겨 담는 방식
  - AOF (Append On File) 방식
    - Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태
## Redis 운영 주의사항
- 메모리 관리
  - 큰 메모리를 사용하는 하나의 instance 보다는 작은 메모리를 사용하는 여러개의 instance가 안전하다.
    ![image](https://user-images.githubusercontent.com/38865267/155071524-fe5b616c-0ad5-48ef-91b7-504d308857f7.png)
  - Ziplist를 잘 활용해야함...
- O(N) 관련 명령어는 주의할 것
  - 싱글쓰레드기 때문에 많은데이터에 대해서 느린 응답을 받을 수 있음. 한번에 하나의 명령어만 수행하기 때문.
  - scan 명령을 사용해 수행시간이 긴 명령어를 수행시간이 짧은 여러개의 명령어로 바꿀 수 있다.
  - 큰 Collection을 작은 여러개의 Collection으로 나눠서 저장하도록 한다.
- Redis Cluster?
  - 장점: 
    - 자체적인 primary, Seconfary Failover
    - Slot 단위의 데이터 관리
  - 단점:
    - 그만큼 메모리 사용량 증가
    - Migration 문제는 관리자가 직접 결정
    - Library 구현이 필요.. 난이도 증가
- 모니터링 요소
  - Redis Info를 통한 정보
    - RSS
    - Used Memory
    - Connection 수
    - 초당 처리 요청 수
  - System
    - CPU
    - Disk
    - Network rx/tx
  - CPU가 100%일 경우
    - 처리량이 매우 많은 경우
      - CPU 성능을 높인다.(서버 이전..)
      - 혹은 스케일아웃?
    - O(N) 계열의 명령이 많은 경우
      - Monitor 명령을 통해 특정 패턴을 파악해볼 필요가 있다.
      - 괜히 Monitor를 잘못쓸 경우 부하가 더 가므로 주의
## 정리
- Redis는 많은 기능이 있는 방면, 싱글쓰레드다.
- 결국엔 속도 차이가 나는건데 100만건의 데이터를 기준으로 명령어를 실행할때 Mamcached는 1ms, Redis는 1초로 엄청난 차이가 있다.
- Redis는 특정 간격마다 RDB 작업을 할 경우 또 시간이 오래 걸리므로 주의해야한다.
- AWS ElasticCache에서는 Memcached용과 Redis용이 존재하는데 잘고려해서 사용해야할 듯...
- 메모리가 날라가도 원본으로 복구가 가능한 경우면 Memcached를 사용하는 것이 좋다. 만약, 복구가 안되어 서비스 장애로 이어지는 상황이면 Redis를 사용하는 것이 좋다.
- Memcahed는 통신 속도 향상을 위한 것이 주목적