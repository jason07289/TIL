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