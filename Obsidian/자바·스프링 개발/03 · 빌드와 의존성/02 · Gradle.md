# Gradle

> [!info] 관련 개념
> - [[01 · Maven|Maven]] — 같은 프로젝트 역할을 맡을 수 있는 빌드 도구
> - [[03 · Wrapper|Wrapper]] — 프로젝트가 지정한 Gradle 버전으로 실행하는 방법

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 스프링이나 스프링부트를 사용하려면 Maven이나 Gradle이 필수인가요?

Gradle은 의존성 관리와 컴파일, 테스트, 패키징 등의 작업을 자동화하는 빌드 도구다. Spring Boot 프로젝트를 만들 때 Maven과 Gradle 중 하나를 선택하는 경우가 많다. Gradle 설정을 쓰는 웹 프로젝트도 필요한 Spring과 Boot 모듈을 프로젝트 의존성으로 선언하고 Java 코드를 컴파일한다. [Spring Boot 빌드 도구](https://docs.spring.io/spring-boot/reference/using/build-systems.html)

| 찾고 싶은 것 | Maven 프로젝트 | Gradle 프로젝트 |
| --- | --- | --- |
| 주요 빌드 설정 | `pom.xml` | `build.gradle` 또는 `build.gradle.kts` |
| 프로젝트에서 제공하는 실행 스크립트 | `./mvnw` | `./gradlew` |
| Spring Boot 앱 로컬 실행 | `./mvnw spring-boot:run` | `./gradlew bootRun` |

위 명령은 macOS·Linux에서 Wrapper가 포함되고 Spring Boot 플러그인이 설정된 프로젝트를 가정한다. Gradle의 `bootRun`은 Spring Boot Gradle 플러그인이 제공하는 작업이다. 예를 들어 Maven 예제에서 실행한 것과 같은 `GET /rooms` 웹 앱을 Gradle 프로젝트에서도 띄울 수 있다. [Spring Boot Gradle 실행 작업](https://docs.spring.io/spring-boot/gradle-plugin/running.html)

Spring Boot는 의존성 관리를 지원하는 Maven과 Gradle을 권장하며, 다른 빌드 도구도 사용할 수 있다. 필요한 JAR를 직접 관리한다면 하위 의존성과 버전을 맞추고 컴파일·실행 시 클래스패스를 지정하는 작업을 직접 맡게 된다. [Spring Boot 빌드 도구](https://docs.spring.io/spring-boot/reference/using/build-systems.html)

## 복습 질문

- `build.gradle.kts`와 `gradlew`가 함께 있는 Spring Boot 프로젝트에서는 어떤 명령으로 앱을 띄울 수 있을까?
