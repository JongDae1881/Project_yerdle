# Java

> [!info] 관련 개념
> - [[02 · JDK|JDK]] — Java 소스를 컴파일하고 실행하는 도구
> - [[03 · 라이브러리와 프레임워크|라이브러리와 프레임워크]] — Java 표준 라이브러리를 직접 호출하는 예시

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 자바는 언어, 스프링은 자바의 프레임워크, 스프링부트는 스프링을 더 편리하게 만든 것, Maven은 자바 빌드 도구(패키지 관리자 역할도 함) 이라고 대충 아는데 정확히 어떻게 되나요?

Java는 소스 코드를 작성하는 프로그래밍 언어다. 꼬들의 게임방 이름이나 게임방 목록을 표현할 때 Java 문법으로 클래스, 변수, 메서드를 작성한다. `.java`는 그 소스를 저장하는 파일의 확장자다.

Java 개발 환경에는 자주 쓰는 기능을 모은 표준 라이브러리도 있다. 원본 대화의 예처럼 `ArrayList`를 사용하면 목록에 원소를 추가하는 기능을 호출할 수 있다.

```java
List<String> names = new ArrayList<>();
names.add("철수");
names.add("영희");
```

`List`와 `ArrayList`는 `java.util`에 있는 표준 라이브러리 타입이다. 게임방 목록에 어떤 값을 추가할지는 이 코드를 작성하는 개발자가 결정한다. 외부 프레임워크인 [[01 · Spring Framework|Spring Framework]]는 같은 Java 프로젝트에서 객체 관리와 웹 요청 처리 같은 기능을 제공한다. [Oracle Java 기초 문서](https://dev.java/learn/)

## 복습 질문

- `ArrayList`를 사용하는 소스 코드에서 Java 문법을 쓰는 부분과 표준 라이브러리 기능을 쓰는 부분을 각각 찾아볼 수 있을까?
