# Helm

## Helm?
- k8s 패키지를 관리하기 위한 툴, 쿠버네티스 패키지 배포를 가능하게 한다.
- node.js에서 npm과 비슷한 느낌이다.
- chart라고 부르는 package format을 사용한다.
- chart는 쿠버네티스 리소스를 describe하는 파일들의 집합이다.

## Helm의 목적
- 스크래치(scratch) 부터 새로운 차트 생성
- 차트 아카이브(tgz) 파일로 차트 패키징
- 차트가 저장되는 곳인 차트 리포지터리와 상호작용
- k8s 클러스터에 차트 인스톨 및 언인스톨
- Helm으로 설치된 차트들의 릴리스 주기 관리

## Helm의 중요 개념 3가지
- 차트는 쿠버네티스 애플리케이션의 인스턴스를 생성하는 데에 필요한 정보의 꾸러미이다.
- 설정은 릴리스 가능한 객체를 생성하기 위해 패키징된 차트로 병합될 수 있는 설정 정보를 가진다.
- 릴리스는 차트의 구동중인 인스턴스이며, 특정 설정이 결합되어 있다.

## Helm의 구성요소
- 헬름 클라이언트: 엔드유저용 명령줄 클라이언트
- 헬름 라이브러리: 헬름 동작 수행에 대한 로직을 제공, k8s API 서버에 대한 인터페이스 역할을 한다.

## Helm 차트(chart)란 무엇인가
- k8s 리소스와 관련된 셋을 설명하는 파일의 모음이다.
- 하나의 차트는 memcahed 파드를 배포하는 것처럼 단순한 형태나 HTTP 서버, DB, 캐시 등으로 구성된 완전한 웹앱 같이 복잡한 형태로 사용이 가능하다.
- 특정 디렉터리 구조를 가진 파일들로 생성된다.
- 
## Chart.yaml 파일 예시 (필드 내용들만 알고 가자)

```yaml
    apiVersion: 차트 API 버전 (필수)
    name: 차트명 (필수)
    version: SemVer 2 버전 (필수)
    kubeVersion: 호환되는 쿠버네티스 버전의 SemVer 범위 (선택)
    description: 이 프로젝트에 대한 간략한 설명 (선택)
    type: 차트 타입 (선택)
    keywords:
    - 이 프로젝트에 대한 키워드 리스트 (선택)
    home: 프로젝트 홈페이지의 URL (선택)
    sources:
    - 이 프로젝트의 소스코드 URL 리스트 (선택)
    dependencies: # 차트 필요조건들의 리스트 (optional)
    - name: 차트명 (nginx)
        version: 차트의 버전 ("1.2.3")
        repository: 저장소 URL ("https://example.com/charts") 또는 ("@repo-name")
        condition: (선택) 차트들의 활성/비활성을 결정하는 boolean 값을 만드는 yaml 경로 (예시: subchart1.enabled)
        tags: # (선택)
        - 활성화 / 비활성을 함께하기 위해 차트들을 그룹화 할 수 있는 태그들
        enabled: (선택) 차트가 로드될수 있는지 결정하는 boolean
        import-values: # (선택)
        - ImportValues 는 가져올 상위 키에 대한 소스 값의 맵핑을 보유한다. 각 항목은 문자열이거나 하위 / 상위 하위 목록 항목 쌍일 수 있다.
        alias: (선택) 차트에 대한 별명으로 사용된다. 같은 차트를 여러번 추가해야할때 유용하다.
    maintainers: # (선택)
    - name: maintainer들의 이름 (각 maintainer마다 필수)
        email: maintainer들의 email (각 maintainer마다 선택)
        url: maintainer에 대한 URL (각 maintainer마다 선택)
    icon: 아이콘으로 사용될 SVG나 PNG 이미지 URL (선택)
    appVersion: 이 앱의 버전 (선택). SemVer인 필요는 없다.
    deprecated: 차트의 deprecated 여부 (선택, boolean)
    annotations:
    example: 키로 매핑된 주석들의 리스트 (선택).
```