# JDK

> [!info] 관련 개념
> - [[01 · Java|Java]] — JDK로 컴파일할 소스를 작성하는 언어
> - [[01 · Maven|Maven]] — JDK를 사용해 컴파일을 포함한 빌드 작업을 자동화하는 도구

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 왜 자바를 깔았지만 Maven을 따로 깔아야 한다면서 스프링이나 스프링부트는 따로 안 까는지 모르겠습니다.

JDK(Java Development Kit)는 Java 프로그램을 개발하고 실행하는 데 필요한 컴파일러와 실행 환경 등을 제공한다. “Java를 설치했다”라는 말은 개발 환경을 준비하는 맥락에서 보통 JDK를 설치했다는 뜻이다.

| 구성 요소 | 역할 |
| --- | --- |
| `javac` | `.java` 소스를 JVM이 읽을 수 있는 `.class` 바이트코드로 컴파일한다. |
| `java` | JVM을 시작하고, 실행할 클래스나 실행 가능한 JAR를 지정한다. |
| JVM | Java 바이트코드를 실행하는 가상 머신이다. |

패키지 선언이 없고 `main` 메서드가 있는 `Hello.java`라면, 파일이 있는 디렉터리에서 아래 두 명령으로 단계를 확인할 수 있다.

```sh
javac Hello.java
java Hello
```

첫 명령에서는 `Hello.class`가 만들어진다. 두 번째 명령의 `Hello`는 실행할 클래스 이름이다. 설치 및 경로 설정을 확인하려면 `javac -version`과 `java -version`을 사용한다. [Oracle의 javac 문서](https://docs.oracle.com/en/java/javase/25/docs/specs/man/javac.html), [Oracle의 java 문서](https://docs.oracle.com/en/java/javase/25/docs/specs/man/java.html)

프로젝트에 소스 파일과 외부 의존성이 많아지면 Maven이나 [[02 · Gradle|Gradle]]로 컴파일 대상과 라이브러리 경로를 관리할 수 있다. 이때도 Java 소스를 컴파일하는 도구는 JDK가 제공한다.

## 복습 질문

- `javac Hello.java`를 실행한 뒤 어떤 파일이 생성될까? `java Hello`는 무엇을 시작할까?
