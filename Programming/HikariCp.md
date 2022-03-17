# HikariCp
- Database와의 커넥션 풀을 관리해준다.
- Pool에서 미리 커넥션을 만들어 놓고 동작.
    ![image](https://user-images.githubusercontent.com/38865267/158754704-c6426405-1b5a-49b2-bd9e-015484a198db.png)
  - HikariCp는 미리 정해놓은 만큼의 커넥션을 Pool에 담아 놓는다.
  - Thread에 커넥션 요청이 들어올 시 Hikari는 Pool 내에 있는 커넥션을 연결해 준다.
  - Thread 입장에선 즉시 쿼리를 날리는 것 처럼 느껴진다.
- Spring boot 2.0 부터 default JDBC 커넥션이다.

## Spring에서의 사용법
```java
    static{
        config.setJdbcUrl("jdbc_url");
        config.setUsername("database_username");
        config.setPassword("database_password");
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");
        ds = new HikariDataSource (config);
    }
```
- HikariConfig는 DataSource를 초기화 하는데 사용되는 구성 클래스이다. 4개의 필수 매개변수인 username, password, jdbcUrl, dataSourceClassName이 함께 제공된다.
- 기타 설정 옵셥
  - autoCommit
  - connectionTimeout
    - pool에서 Connection을 얻어오기까지 기다리는 최대시간
    - 허용가능한 시간을 초과하면 SQLException 발생.
    - default: 30,000 (30s)
  - idleTimeout
    - pool에서 일을 하지 않는 Connection을 유지하는 시간
    - 이 옵션은 minimumIdle이 maximumPoolSize보다 작게 설정되어있을 때만 설정.
    - default: 600,000(10min)
  - maxLifetime
    - connection pool에서 살아 있을 수 있는 connection의 최대 수명 시간
    - 사용중인 connection은 maxLifetime에 상관없이 제거되지 않음
    - 0으로 설정시 무한대의 life time이 적용된다.
    - default: 1,800,000(30min)
  - connectionTestQuery
    - 이 옵션은 JDBC4를 지원안하는 드라이버를 위한 옵션
    - JDBC4 드라이버를 지원 한다면 이 옵션을 설정하지 않는 것을 추천
    - connection pool에서 connection을 획득하기 전에 살아있는 connection인지 확인하기 위한 vaild query를 던진다.
    - JDBC4 드라이버를 지원하지 않는 환경에서 이 값을 설정하지 않으면 error레벨 로그 발생
    - default: none
  - mininumIdle
    - 아무 일을 하지 않아도 적어도 이 옵션에 설정된 size로 connection을 유지
    - default: 10
  - maximumPoolsize
    - pool에 유지시킬 수 있는 최대 커넥션
    - default: 10
  - readOnly
    - pool에서 connection을 획득할 때 read-only 모드로 가져옴
    - 몇몇의 database는 read-only 모드를 지원하지 않음
    - default: false
  - driverClassName
    - HikariCp는 jdbcUrl을 참조하여 자동으로 driver를 설정하려고 시도하지만, 몇몇의 오래된 driver들은 driverClassName을 명시해야 한다.
  - leakDetection Threashold
    - connection이 누수 메시지가 나오기 전에 Connection을 검사하여 pool에서 connection을 내보낼 수 있는 시간
    - 0으로 설정 시 leak detection을 이용하지 않는다.
    - 최소값: 2000ms
    - default: 0
