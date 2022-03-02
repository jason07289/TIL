# Domain Driven Design
- DDD의 전략적 설계 방법론에서는 Domain Model과 Bounded Context, Ubiquitous Language 등을 정의한다.

## Domain이란?
- 비즈니스 Domain을 의미하며 유사한 업무의 집합이다.
- 애플리케이션은 비즈니스 Domain 별로 나누어 설계 및 개발 될 수 있다.

## DDD?
1. 비즈니스 Domain 별로 나누어 설계하는 방식.
   - 기존의 애플리케이션 설계(현업에서 IT로의 일방향 소통구조)에서 탈피하여 <span style="color:red">현업과 IT의 쌍방향 커뮤니케이션</span>을 매우 중요하게 생각합니다.

2. DDD의 핵심목표는 "Loosly coupling", "High Cohesion"
3. DDD는 Strategic Design(보통 개념을 설계)과 Tactical Design(프로그래밍을 하기 위한 구체적 설계)으로 나눌 수 있다.

## Strategic Design(전략적 설계)
1. 비즈니스의 상황(Context:대상사용자 등등)에 맞게 설계하자는 컨셉
2. 전략적 설계를 위해 비즈니스 도메인의 상황을 <span style="color:red">Event storming</span>으로 공유, 비즈니스 목절별로 서비스들을 그루핑한다.
    - Event storming: https://haandol.github.io/2020/12/10/demystifying-event-storming.html
3. Bounded Context & Domain Model
   - Bounded Context는 비즈니스 도메인의 사용자, 프로세스, 정책/규정 등을 고유한 비즈니스 목적별로 그룹핑한 것이다. 사용자, 프로세스, 정책/규정들을 그 비즈니스 도메인의 Context라고 말할 수 있으므로 Bounded Context는 Domain안의 서비스를 경계지은 Context의 집합이라고 할 수 있다.
   - Domain Model은 비즈니스 도메인의 서비스를 추상화한 설계도. 도메인을 sub Domain으로 분해한 것과 Bounded Context가 Domain Model이다.
4. Bounded Context & Micro Service: 1개의 Bounded Context는 최소한 1개 이상의 Micro Service로 구성된다.
5. Context Map: Bounded Context 간 관계를 도식화한 다이어그램.
6. Ubiquitous Language
   - 현업, 개발자, 디자이너 등 참여자들이 동일한 의미로 이해하는 언어.
   - 비즈니스 도메인에 따라 동음이의어가 많기 때문에 공통언어를 정의하고 사용해야 한다.
   - 하나의 도메인내에서는 어떤 단어나 문장이 동일한 의미로 소통된다.

## Event Sourcing
- 전통적인 데이터 저장 방식에서는 로직을 처리하고 그 결과를 DB에만 저장하는데에 반해, Event Sourcing에서는 데이터에 변경을 주는 모든 과정의 이벤트들을 순차적으로 저장한다.
- 이벤트는 어떠한 것이 일너난 사실이기에 수정되거나 삭제될 수 없고 오직 추가만 된다. -> 데이터 접근을 위한 경쟁이 발생하지 않음.
- 이벤트를 순서대로 재생하여 도메인 모델의 상태를 복원할 수 있다.
- 도메인 이벤트는 단순한 구조로 저장되기에 다양한 데이터베이스를 고려할 수 있고 분산 저장소에 적합하여 규모 확장도 쉽다.