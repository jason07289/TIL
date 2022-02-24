# HPA (Horizontal Pod Autoscaling)
- HorizontalPodAutoscaler는 워크로드 리소스(deployment, statefulset)를 자동 업데이트하며, 워크로드의 크기를 수요에 맞게 자동으로 스케일링하는 것이 목표
- 데몬셋과 같은 크기 조절이 불가능한 오브젝트에는 적용이 불가능하다.
- 쿠버네티스 API 자원 및 컨트롤러 형태로 구현... 컨트롤 플레인내에서 실행되는 HPA 컨트롤러는 평균 CPU 사용률, 평균 메모리 사용률, 또는 다른 커스텀 메트릭 등의 관측된 메트릭을 목표에 맞추기 위해 목표물의 적정 크기를 주기적으로 조정한다.(yml파일에 최소개수, cpu나 mem 임계치를 설정했었는데 이에 따라 실제로 늘어나는 것을 확인했다.)

## HorizontalPodAutoscaler 작동원리
- HPA는 간헐적으로 실행된다. 실행주기는 kube-controller-manager의 --horizontal-pod-autoscaler-sync-period 파라미터에 의해 설정된다
- 각 주기마다, Controller Manager는 각 HPA에 지정된 메트릭에 대해 리소스 사용률을 질의한다.
- Controller Manager는 리소스 메트릭 API(파드 단위 리소스 메트릭 용) 또는 사용자 지정 메트릭 API(다른 모든 메트릭 용)에서 메트릭을 가져온다.
- 메트릭 수집 후 알고리즘(원하는 레플리카 수 = ceil[현재 레플리카 수 * ( 현재 메트릭 값 / 원하는 메트릭 값 )])에 따라서 오토스케일링이 일어난다.