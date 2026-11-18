# Maven

> [!info] 관련 개념
> - [[02 · JDK|JDK]] — Maven이 Java 소스를 컴파일할 때 사용하는 도구 모음
> - [[02 · Spring Boot|Spring Boot]] — Maven 설정으로 스타터와 플러그인을 사용하는 프로젝트
> - [[03 · Wrapper|Wrapper]] — 프로젝트에 지정된 Maven 버전으로 명령을 실행하는 방법

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 왜 자바를 깔았지만 Maven을 따로 깔아야 한다면서 스프링이나 스프링부트는 따로 안 까는지 모르겠습니다.

Maven은 프로젝트의 의존성을 관리하고 빌드 단계를 실행하는 도구다. `pom.xml`에 어떤 외부 코드가 필요한지 선언하고 플러그인과 패키징 등 빌드 설정을 적는다.

## 웹 스타터를 추가하면 들어오는 것

아래는 원본 대화의 Spring Boot 웹 스타터 선언이다. 해당 스타터를 사용하는 Spring Boot 프로젝트에서 Parent POM 또는 BOM으로 버전 관리를 설정한 상황을 가정한다. BOM은 어떤 의존성 버전을 사용할지 모아 놓은 설정이다.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Maven은 스타터와 스타터가 참조하는 하위 의존성까지 확인한다. 로컬 저장소에 필요한 파일이 없으면 Maven Central 같은 원격 저장소에서 해당 파일을 받아 저장한다. 프로젝트는 이 방식으로 Spring MVC나 내장 서버 관련 JAR를 컴파일과 실행에 사용할 수 있다. [Spring Boot 의존성 관리와 스타터](https://docs.spring.io/spring-boot/reference/using/build-systems.html)

> [!note] 추가 설명
> Spring Boot 4에서 새 MVC 프로젝트를 구성할 때는 공식 문서가 기존 `spring-boot-starter-web`의 후속 스타터로 `spring-boot-starter-webmvc`를 안내한다. 프로젝트의 Boot 버전에 맞는 이름을 사용한다. [Spring Boot 스타터 목록](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)

## 빌드와 로컬 실행

| 명령 | 일반적인 동작 |
| --- | --- |
| `mvn compile` | JDK의 컴파일러를 사용해 프로젝트 소스를 컴파일한다. |
| `mvn test` | 필요한 컴파일을 거친 뒤 단위 테스트를 실행한다. |
| `mvn package` | 앞선 단계들을 수행하고, 프로젝트 설정에 따라 JAR 등의 파일을 만든다. |
| `mvn spring-boot:run` | Spring Boot Maven 플러그인이 앱을 로컬에서 실행한다. |

실행 가능한 Boot JAR를 만들려면 앱의 빌드 설정에 Spring Boot Maven 플러그인의 패키징 설정이 필요하다. 로컬에서 앱을 띄울 때는 `spring-boot:run`을 사용할 수 있다. [Maven 빌드 생명주기](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html), [Spring Boot Maven 플러그인](https://docs.spring.io/spring-boot/maven-plugin/)

macOS에서 시스템의 `mvn`을 사용하려면 [[01 · Homebrew|Homebrew]]로 `brew install maven`을 실행하고 `mvn -version`으로 확인할 수 있다. 프로젝트에 Maven Wrapper가 포함돼 있으면 `./mvnw`를 사용해 지정된 Maven 버전으로 실행할 수 있다. [Homebrew Maven Formula](https://formulae.brew.sh/formula/maven), [Maven Wrapper](https://maven.apache.org/tools/wrapper/)

## 복습 질문

- 웹 스타터 한 줄을 선언했는데 여러 JAR가 프로젝트에 들어오는 이유는 무엇일까?
- `mvn package`와 `mvn spring-boot:run`을 각각 어떤 목적으로 쓸까?
