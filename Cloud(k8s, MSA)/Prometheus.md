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
  - node exporter: 서버의 CPU, Memory 등을 수집
  - nginx exporter: nginx의 데이터를 수집
- Prometheus Server
  - prometheus.yml이라는 설정 파일로 프로메테우스 서버를 띄운다.
  - 매트릭 수집, 매트릭 규칙을 설정할 수 있다.
  - 메트릭을 수집할 엔드포인트를 설정할 수 있다.

- Grafana
- Alertmanager