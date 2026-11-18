# Wrapper

> [!info] 관련 개념
> - [[01 · Maven|Maven]] — Maven Wrapper가 실행하는 빌드 도구
> - [[02 · Gradle|Gradle]] — Gradle Wrapper가 실행하는 빌드 도구
> - [[02 · JDK|JDK]] — Java 프로젝트를 빌드하고 실행하는 데 필요한 환경

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 왜 자바를 깔았지만 Maven을 따로 깔아야 한다면서 스프링이나 스프링부트는 따로 안 까는지 모르겠습니다.

Maven과 Gradle은 프로젝트 저장소에 Wrapper 스크립트를 둘 수 있다. Wrapper는 설정 파일에서 사용할 빌드 도구 배포판을 확인한다. 해당 버전이 준비되어 있으면 재사용하고, 없으면 받아서 실행한다. 팀원이 각자 같은 빌드 도구 버전을 설치하는 수고를 줄일 수 있다.

| 빌드 도구 | 환경에 설치한 명령 | macOS·Linux의 Wrapper | Windows PowerShell의 Wrapper | 버전 설정 파일 |
| --- | --- | --- | --- | --- |
| Maven | `mvn` | `./mvnw` | `.\mvnw.cmd` | `.mvn/wrapper/maven-wrapper.properties` |
| Gradle | `gradle` | `./gradlew` | `.\gradlew.bat` | `gradle/wrapper/gradle-wrapper.properties` |

Wrapper 파일과 Spring Boot Maven 플러그인이 있는 프로젝트에서 `./mvnw spring-boot:run`을 실행하면 Wrapper가 Maven을 준비한다. Maven은 뒤에 붙인 `spring-boot:run` 목표를 실행한다. 환경에 전역 `mvn` 명령이 없어도 Wrapper를 사용할 수 있으며, 빌드에 필요한 호환 Java 환경은 준비되어 있어야 한다. [Maven Wrapper](https://maven.apache.org/tools/wrapper/), [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html)

## 복습 질문

- 팀원 두 명이 전역 Maven 버전을 다르게 쓰더라도 `./mvnw`로 같은 Maven 버전을 쓸 수 있는 이유는 무엇일까?
- Wrapper가 준비하는 Maven 배포판과 `pom.xml`에 선언된 Spring 의존성은 각각 누가 가져올까?
