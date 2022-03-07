# JAVA 17 (LTS)
- 2021년도 9월 JAVA 17 LTS(Long Term Support)로 출시

## java 17 특징
- Context-Specific Deserialization Filters (컨텍스트 기반의 역직렬화 필터링)
- (Second Incubator) Vector API
- Deprecate the Security Manager (Security Manager Deprecate 지정)
- Remove the Experimental AOT, JIT Compiler
- Sealed Class
  - Sealed Class는 클래스와 인터페이스 모두 사용 가능하며 상속할 수 있는 범위를 제한하는 것.
  - 기존에는 접근제한자(public, private, final 등)를 이용하였지만 Sealed class는 명확하게 상속 범위를 개발자가 지정할 수 있다.
  - Preview(패턴 매칭 스위치)
    - 스위치 문 안에서 패턴을 사용할 수 있는 것
    - 기존 instanceof 구문에서 패턴 매칭기능의 연장선에 있다.
- 12~16에서 나온 많은 기능이 운영 환경 및 대규모 서비스에서 사용가능하게 됨.
  - Record 클래스 (자바16) : 데이터를 표현하기 위한 클래스를 작성시 불필요한 작업을 줄여준다.
  - 텍스트 블록 (자바15) : 여러줄에 걸쳐서 작성해야 하는 문자열을 정의할 때 매우 편리하다. SQL 문장이나 XML 문장등을 작성할 때 유용하다.
  - 스위치 표현 (자바14) : 스위치 구문(switch statements)이 아닌 스위치 표현(switch expressions)이다. 스위치 문장에서 -> 를 이용해서 경우의 수를 쉽고 다양하게 정의할 수 있다.
- 기본 레거시 랜덤(java.util.Random) 클래스를 확장, 리팩토링한 RandomGenerator 난수 생성 API가 추가