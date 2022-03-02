# Axon
## AxonFramework
- Spring boot 환경에서 어플리케이션을 개발할 수 있도록 지원해준다.
- CQRS 구현을 위해 Command 부분을 위한 어플리케이션과 Query 부분을 위한 어플리케이션을 분리하는 것을 원칙으로 사용한다.
- command, Query 공통적으로 사용하는 Command와 Event 등을 위해 공통모듈 개발도 필요하다.

## Axon Server
- 마이크로 서비스간 메시지 Routing을 담당한다.
## 개발환경
- VS Code, Gradle Multi Project 권장
- Gradle Spring Boot를 위해 Extension Pack for Java, Spring Boot Extension Pack, Spring Boot Tools, Gradle for Java, Lombok Annotations Support for VS Code 등의 extension들을 설치한다.
- Axon Application 개발을 위해서는 Axon Server와의 연결이 필요하므로 로컬 환경에 Axon Server를 설치한다.
- Intellij IDE 사용가능.

## AxonFramework Architecture
![image](https://user-images.githubusercontent.com/38865267/156282478-892efc9c-50c3-4eaa-8d23-f4a0dc9af4ba.png)
    출처: https://docs.axoniq.io/reference-guide/architecture-overview
- CQRS 구조이다. 외부에서 Command가 들어오면, 해당 명령에 대한 이력을 EventStore에 저장하고 동시에 Event를 발생시킨다.
- 발생된 Event는 Event Handler를 통해여 Read model에 반영되고, 사용자 입장에서는 Read Model에 대한 Query를 통하여 데이터를 읽는 구조이다.

## Messages
- Axon의 핵심 개념 중 하나는 메시징, Axon APPlication과 Axon Server간의 모든 통신은 메시지 객체를 사용하여 이루어진다.
- 모든 메시지에는 Payload, MetaData 및 고유 식별자가 포함된다. Payload는 메시지의 본체이다. MetaData는 메시지가 전송되는 context로 http 메시지의 header와 비슷하다고 생각하면 된다.
- Axon에서는 Command, Event, Query 이렇게 세가지의 메시지가 존재한다.

