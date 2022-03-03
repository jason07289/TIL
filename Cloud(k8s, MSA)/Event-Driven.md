# Event-Driven
- MSA가 적용된 시스템을 보완해주는 개념
- 

## EDA(Event-Driven Architecture)
- 분산된 시스템 간 이벤트를 생성, 발행(Publishing)하고 발행된 이벤트를 필요로하는 수신자에게 전송된다.
- 이벤트를 수신한 수신자가 이벤트를 처리하는 형태의 시스템 아키텍처
- Event Driven Pattern: 특정 행동이 자동으로 혹은 순서에 따라 발생하는 것이 아닌 어떤 일에 대한 반응으로 동작하는 디자인 패턴
  - 예시:
    - IO Event: 컴퓨터 회로를 구동시키기 위해 발생하는 일
    - IOT 기기 등의 센서로 부터 유입되는 데이터 스트리밍 기반의 동작
    - <strong><span style="color:blue">시스템 내,외부에서 발생한 주목할 만한 상태의 변화</span><strong>
- 주로 Event Driven 시스템은 Message Broker(Kafka, Rabbit MQ, Redis)와 결합하여 Message Driven 시스템으로 구성된다.

## EDA 구성요소
- Event Generator: 시스템 내,외부의 상태 변화를 감지하여 표준화된 형식의 이벤트를 생성
- Event Channel: 이벤트를 필요로 하는 시스템까지 발송
- Event Processing Engine: 수신한 이벤트를 식별, 적절한 처리를 함. 때에 따라, 이벤트 처리의 결과로 또 다른 이벤트를 발생시킬 수 있다.

### Event Processing Style
- Simple Event Processing
  - 각각의 이벤트가 직접적으로 수행해야할 action과 매핑되어 처리 된다.
  - 실시간으로 작업의 흐름을 처리할 때 사용되며, 이벤트 처리 시간과 비용의 손실이 적다.
- Event Stream Processing
  - 이벤트를 중요도에 따라 필터링하여 걸러진 이벤트만을 수신자에게 전송.
  - 실시간으로 정보의 흐름을 처리할 때 사용되며, 기업에 적용될 경우 신속한 의사 결정을 가능하게 한다.
- Complex Event Processing
  - 일상적인 이벤트의 패턴을 감지하여 더 복잡한 이벤트의 발생을 추론하는 것.

## EDA의 장단점
- 장점
  - Decoupling: 시스템 간의 느슨한 결합이 가능하므로 MSA 환경에서 시스템 간 의존성을 배제하는 것이 가능하다.
  - 다른 시스템의 정보를 알 필요가 없다.(미리 정의된 Event message를 가지고 상호 정보 교환)
  - Micro Service 단위로 시스템을 분리하기 쉽기 때문에 확장성, 탄력성을 고려하기 좋다.

- 단점
  - Broker Dependency: Event를 전송하기 위한 Message Broker에 대한 의존성이 커지기 때문에 Message Broker 장애 시, 전체 장애로 이에질 수 있다.
  - Transaction 단위가 격리되기 때문에 서비스 장애 발생시 retry/rollback을 고려해야 한다.
  - 디버깅이 어렵다.

## MSA에서의 Event

### Event
- MSA에서의 event는 시스템의 내,외부에서 발생한 주목할 만한 상태의 변화(a significant change in state)이다.
- 데이터의 생성, 변경, 삭제를 통해 발생하는 서비스의 의미있는 변화

### Event log를 보관
- 현재의 데이터는 상태 변경의 누적이다.
- 상태 변경은 이벤트를 의미하고 이를 누적하는 행위는 이벤트 로그를 보관하는 것이다.
- 보관된 이벤트는 데이터의 현재 상태를 구성하는 근간이다.
- 장애발생 혹은 특정상황에 보관된 이벤트를 바탕으로 복원이 가능하다.

### 비동기 통신
- amqp, mqtt, jms 등 메시징 프로토콜을 이용한 메세지 큐 방식이 자주 사용된다.
- 서비스에서 데이터의 생성,변경,삭제를 통해 이벤트가 발생하면 발행 서비스는 메세지 형태로 이벤트를 발행하고 해당 이벤트에 관심이 있는 서비스에서 구독을 수행한다.
- 메세지 큐를 사용함으로 requeue/dlq(dead letter queue) 등의 기능을 활용할 수 있다.

### 시스템 내 통합
- 이상적으로 구현된 MSA는 서비스 간 데이터 참조를 위한 내부 통신이 필요없지만, 현실적으로 서비스 간 내부 통신이 전혀 없는 시스템을 구현하기는 불가능하다.

### 트랜잭션 관리
- Micro Service 단위로 분리된 환경이기 때문에 각자 데이터베이스를 적용한 시스템에 대해 데이터 무결성을 보장할 수는 없지만 Event를 통해 최종적인 일관성을 유지 할 수는 있다.