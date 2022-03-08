# Spring / Spring boot version 별 특징

## Spring 3.X
- java5(제네릭, 가변 매개변수 등)을 사용하여 개정되었다.
  - BeanFactory 등 핵심 API 업데이트
- @Async를 통해 비동기 메서드 호출 지원
- jar파일의 모듈화(spring-core, spring-web)
- Annotation 및 DI 지원
- H2와 같은 Embeded DB 지원

## Spring 4.X (Spring boot 1.5)
- java 8 기능 지원
- Starter 팩의 등장
- Groovy를 통한 Bean 설정이 가능 (gradle)
- Core Container 들의 기능 지원 확대
  - Bean 관리 용이(@Order, @Lazy)
- @RestController 등 Web 개발 도구의 지원이 강화
- @Conditional을 사용한 유연한 조건부
- @Autowired 지원

## Spring 5.X (Spring boot 2.0.0 ~ )
- 제네릭, 람다 등을 통해 가독성이 향상되었다.
- JDK 9와 호환
- Spring WebFlux 추가
  - 복수개의 서비스로 이루어진 분산 시스템 제공
  - 서비스간 호출이 많은 MSA에 적합핟.
  - 비동기와 Non 블로킹 이벤트 루프 모델 사용가능
  - Reactive Programming
  - Tomcat 말고 Netty 가능
- kotlin 지원
- junit 5 지원
- HTTP/2 지원
- Data Support
  - 커넥션풀 변경
    - 커넥션풀이 톰캣에서 HikariCP로 변경되었다.
  - 몇몇 Reactive Data 자동설정을 지원
    - 몽고 DB, Redis 등등
- 새로운 TEST 어노테이션
  - @WebFluxTest
  - @DataRedisTest
- Quartz(오픈소스 스케쥴링 라이브러리) 스케쥴러 전용 starter 지원

## Spring 6.X (Spring boot 3.0.0~, 21년도 8월, 현재 로드맵만 나온상황?)
- JDK 17+
- jakarta EE 9 APIs migrate(호환성을 위해 Tomcat 10/Jetty 11 필요)
- jakarta.persistence(최대 절전 모드 ORM 6?).
- XML 구성 형식은 이제 과거형일 것이다.
- RPC 지원기간이 지난다.
- 일부 Java EE API 기한이 지난다.
- 클라우드의 길을 완전하게 열것이다...