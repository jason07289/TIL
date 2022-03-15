# Prometheus
- Apache2 라이센스로 공개된 오픈소스
 ![image](https://user-images.githubusercontent.com/38865267/157573517-353488db-2b47-4f3f-ba12-e1f38afd9ec7.png)

## 프로메테우스의 특성
- 대부분의 모니터링 도구는 Push 방식이었다.
  - 각 서버에 클라이언트를 설치하고 이 클라이언트가 메트릭 데이터를 수집해서 서버로 보내고, 서버가 모니터링 상태를 보여주는 방식
- 프로메테우스는 Pull 방식이다.
  - 서버가 각 클라이언트를 알 필요가 없다.
  - 서버에 클라이언트가 떠 있으면 서버가 주기적으로 클라이언트에 접속해서 데이터를 가져오는 방식이다.

## 프로메테우스 구성요소
- Exporter
  - 모니터링 대상의 Metric 데이터를 수집하고 프로메테우스가 접속했을 때 정보를 알려주는 역할
  - Exporter 실행 시 데이터를 수집하는 동시에 HTTP 엔드포인트를 열어서 프로메테우스 서버가 데이터를 수집해 갈 수 있도록 한다. -> PULL 방식
  - Exporter의 엔드포인트로 접속해 웹 브라우저로 매트릭을 볼 수 있다.
  - node exporter: 서버의 CPU, Memory 등을 수집
  - nginx exporter: nginx의 데이터를 수집

- Prometheus Server
  - prometheus.yml이라는 설정 파일로 프로메테우스 서버를 띄운다.
  - 매트릭 수집, 매트릭 규칙을 설정할 수 있다.
  - 메트릭을 수집할 엔드포인트를 설정할 수 있다.

- Grafana
  - Prometheus Server에 Grafana를 연동해서 대시보드 형태로 시각화가 가능하다.
  - PromSQL을 통해 프로메테우스 서버의 메트릭을 조회할 수 있다.
  - AWS-cloud Watch 등 다른 시스템도 연동이 가능하다.

- Alertmanager
  - 매트릭에 대해 특정한 기준을 정해두고 이를 Alertmanager에서 관리하며 개발자에게 알림을 주게 하는 시스템. 별도의 대시보드가 존재하며, yml 파일로 관리가 가능하다.
  - Grouping: 여러가지 문제에 대해 하나의 알림으로 보낼 수 있다.
  - Inhibition: 알림이 너무 많이 갈 경우 조건을 주어 추가 알림이 가지 않도록 설정 가능.
  - Silences: matcher로 조건을 주어서 일정 시간 동안 알림을 멈출 수 있음

### Actuator (jmx-exporter) 
- JVM에서 발생하는 메트릭을 수집하는 Exporter
- 외장 jmx-exporter와 java agent를 사용하는 두 가지 방법이 있음
- Actuator는 Spring boot에서 maven 또는 gradle로 dependency 추가가 가능하다.
  ```xml
        <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-prometheus</artifactId>
        </dependency>
    </dependencies>
  ```
- jmx-exporter는 기본적으로 tomcat의 8999 port를 이용해서 metric을 export시킨다.
- jmx-exporter를 사용하면 기본적으로 jvm안에서 발생하는 metric을 수집힌다.
  - 대표적으로 heap 메모리 사용률, gc 수행 시간 및 실행 수, thread 수와 같은 metric
- 부가적으로 Spring-boot의 thrid-party 라이브러리에 관해서도 수집이 가능하다.
  - ex) kafka, hystrix, hicaricp, rabbitmq, cache


- jmx-exporter 관련 참고 글: https://ooeunz.tistory.com/145
  