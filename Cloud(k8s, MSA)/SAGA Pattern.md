# SAGA Pattern
- MSA의 어려운점 중 하나인 트랜잭션문제가 있다.
- 기존 Monolithic 환경에서는 트랜잭션을 통해 데이터의 일관성을 유지했었다.
- 서비스, DB가 분산되면 이를 어떻게 처리해야할까?
- 대안: Two-phase commit
  - 문제점: 장애날 경우 서비스에 동시에 Rocking 걸림

## SAGA 패턴
- 서비스간 이벤트를 주고 받아 특정 서비스에서의 작업이 실패하면 이전까지의 작업이 완료된 서비스들에게 보상 이벤트를 소싱해서 분산환경에서의 원자성을 보장하는 패턴
- 로컬 트랜잭션, MSA 서비스 전체에 대한 트랜잭션 개념이 존재한다.
- 트랜잭션 실패 이벤트를 관련 서비스들에게 준다.
    ![image](https://user-images.githubusercontent.com/38865267/155629906-53b9b339-3a00-457f-8431-dd7a331be6ee.png)
- 트랜잭션의 관리주체가 DBMS가 아닌 Application에 존재한다.
- 장애로 인한 Rollback 처리(보상 트랜잭션)은 Application 부분에서 구현한다.
- 서비스마다 순차적으로 트랜잭션이 처리되며 마지막 트랜잭션이 끝났을 때 데이터가 완전히 영속되었다는 것을 확인하고 종료한다. -> 최종 일관성 달성 가능.

## Choreography SAGA 패턴
- 서비스에서 로컬 트랜잰션이 완료되면 완료 event를 만들고 다음 서비스로 전달하는 순차적인 트랜잭션 수행방식
- event는 보통 kafka와 같은 메시지 큐를 통해서 비동기식으로 전달
    ![image](https://user-images.githubusercontent.com/38865267/155631062-8799b884-c8b2-4423-a1ee-5669a9af413c.png)

- 만약 트랜잭션 실패시에는 실패한 서비스에서 보상 이벤트를 발행해서 Rollback
    ![image](https://user-images.githubusercontent.com/38865267/155631096-4cc69236-ae95-4f03-979f-d9f45b4fc14d.png)
- 구성하기 편하다는 장점이 있으나, <span style="color:red">운영자</span> 입장에서 트랜잭션의 현재 상태를 확인하기 어렵다.

## Orchestration based SAGA 패턴
- 위와 다르게 SAGA 인스턴스(Manger)가 별도로 존재한다.
- 트랜잭션에 관혀하는 모든 서비스들이 Manager에 의해 점진적으로 트랜잭션을 수행하며 결과를 Manager에게 전달한다.
- 비즈니스 로직상 마지막 트랜잭션이 끝나면 Manager를 종료해서 전체 트랜잭션 처리를 종료한다.
- 실패시 Manager 자체에서 보상 이벤트(트랜잭션)을 발동해서 일관성을 유지한다.
  ![image](https://user-images.githubusercontent.com/38865267/155636498-4ca3c759-6d9f-4f6a-b550-b2aa79cfdb3a.png)
- 장점:
  - 서비스간 복잡성이 줄어들어서 구현 및 테스트가 쉬워진다.
  - 트랜잭션의 현재 상태를 Manager가 알고 있어서 롤백을 하기 쉽다.(<span style="color:red">운영자</span> 입장에서 편할듯)
- 단점:
  - 관리를 해야하는 Orchestrator 서비스가 추가되어야하기 때문에 인프라 구현이 복잡해집니다. (Manager가 없는 Choreography SAGA 패턴과 상반되는 느낌)