# Spring Boot의 pom.xml

> [!info] 관련 개념
> - [[01 · Maven|Maven]] — 의존성 해결과 `compile`, `test`, `package`의 관계
> - [[03 · Wrapper|Wrapper]] — 프로젝트에 지정된 Maven 버전으로 명령을 실행하는 방법
> - [[02 · 스프링 부트 웹 앱 학습 노트|스프링 부트 웹 앱 학습 노트]] — 이 설정에서 시작해 `main()`을 실행하고 HTTP 요청을 받기까지의 흐름

[원본 대화 · 기본 pom.xml 설명](chatgpt-conversation://6afb0b5b-cd28-83ec-b3d0-6403c2c82aa4)

> 먼저 pom.xml에 대해 알아봅시다. 아무 dependency 추가 안한 pom.xml인데, 이게 기본인가요? 각각의 속성에 대해 알려주세요.

`pom.xml`은 Maven이 프로젝트를 식별하고, 필요한 라이브러리를 구하고, 빌드하는 데 사용하는 XML 파일이다. POM은 Project Object Model의 약자다. 아래 설명에서 쓰는 Boot `4.1.2`와 Java `21`은 대화에 나온 Yerdle 예제의 설정값이다.

## Spring Initializr가 생성한 기본 POM

대화의 예제는 Spring Initializr에서 추가 의존성을 선택하지 않았을 때 생성된 Spring Boot용 구성이다. 기본 실행에 필요한 `spring-boot-starter`와 테스트용 `spring-boot-starter-test`가 이미 선언되어 있다. Maven 프로젝트 자체를 정의하는 최소 POM은 다음과 같다. Spring Boot 의존성과 빌드 플러그인은 목적에 따라 여기에 더한다. [Maven POM 기본 요소](https://maven.apache.org/pom.html#The_Basics), [Spring Boot 스타터](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.yerdle</groupId>
    <artifactId>yerdle-backend</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</project>
```

## XML 선언과 프로젝트 좌표

| 예제의 항목 | Maven과 도구에서 맡는 일 |
| --- | --- |
| `<?xml version="1.0" encoding="UTF-8"?>` | 문서의 XML 규격 버전과 문자 인코딩을 선언한다. |
| `<project>` | Maven 프로젝트 정보를 담는 최상위 요소다. |
| `xmlns="http://maven.apache.org/POM/4.0.0"` | 접두사가 없는 태그를 Maven POM의 네임스페이스에 속하게 한다. 네임스페이스로 서로 다른 XML 규격의 같은 이름을 구분할 수 있다. |
| `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"` | `xsi` 접두사를 XML Schema 관련 속성에 사용할 수 있게 선언한다. |
| `xsi:schemaLocation` | Maven POM 네임스페이스와 대응하는 XSD 주소를 알려 준다. IDE 등이 태그 구조를 검사하거나 자동완성할 때 참고할 수 있다. |
| `<modelVersion>4.0.0</modelVersion>` | 이 파일이 따르는 POM 모델 버전이다. |
| `<groupId>com.yerdle</groupId>` | 프로젝트가 속한 그룹 식별자다. 역순 도메인 표기를 쓰는 관례가 있다. |
| `<artifactId>yerdle-backend</artifactId>` | 그룹 안에서 빌드 산출물을 식별하는 이름이다. |
| `<version>0.0.1-SNAPSHOT</version>` | Yerdle 백엔드의 개발 버전이다. `SNAPSHOT`은 같은 버전 문자열로 여러 번 빌드하거나 배포해도 결과가 달라질 수 있음을 나타낸다. |
| `<packaging>` | 빌드 산출물의 유형을 지정한다. 예제에서는 생략되어 기본값인 `jar`가 적용된다. |

`groupId`, `artifactId`, `version`을 묶어 **Maven 좌표**라고 부른다. 예제 프로젝트의 좌표는 `com.yerdle:yerdle-backend:0.0.1-SNAPSHOT`이다. 기본 산출물 이름은 `yerdle-backend-0.0.1-SNAPSHOT.jar`가 된다. [Maven 좌표와 패키징](https://maven.apache.org/pom.html#The_Basics), [Maven SNAPSHOT 버전](https://maven.apache.org/guides/getting-started/index.html#what-is-a-snapshot-version)

## 부모 POM과 Java 버전

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>4.1.2</version>
    <relativePath/>
</parent>
```

`parent` 안의 좌표는 상속받을 POM을 가리킨다. `4.1.2`는 이 예제에서 지정한 Spring Boot 부모 POM의 버전이다. `<relativePath/>`를 비워 두면 기본 경로인 `../pom.xml`을 후보에서 빼고 좌표로 부모 POM을 찾는다. 일반적인 단독 프로젝트라면 Maven은 로컬 저장소에 있는 사본을 활용하고, 필요하면 원격 저장소에서 내려받는다. [Maven POM 상속](https://maven.apache.org/pom.html#Inheritance)

| 부모에게서 얻는 설정 | 예제에 적용되는 방식 |
| --- | --- |
| `dependencyManagement` | `spring-boot-starter`처럼 관리 대상인 라이브러리를 `dependencies`에 선언하면 Maven이 상속된 버전을 적용한다. 실제 라이브러리는 선언된 의존성과 그 전이 의존성을 따라 들어온다. |
| `pluginManagement` | 해당 빌드 플러그인이 사용될 때 적용할 버전과 실행 설정을 제공한다. 예제에서는 `build/plugins`에 등록된 Spring Boot Maven 플러그인에 부모의 `repackage` 구성이 적용된다. |
| 컴파일 기본 설정 | `properties`의 `<java.version>21</java.version>`이 컴파일러의 릴리스 값을 정하는 데 쓰인다. 실제 컴파일에는 Java 21을 대상으로 컴파일할 수 있는 JDK가 필요하다. |

`properties`는 POM 안에서 재사용할 설정값을 정의한다. 예제의 `${java.version}`은 `21`로 평가된다. 현재 Maven을 실행 중인 Java는 `mvn -version`으로 확인할 수 있다. [Maven POM 속성](https://maven.apache.org/pom.html#Properties), [Spring Boot 부모 POM 설정](https://docs.spring.io/spring-boot/maven-plugin/using.html)

## 비어 있는 프로젝트 정보

| 요소 | 값이 있을 때 나타내는 정보 |
| --- | --- |
| `<name/>`, `<description/>`, `<url/>` | 읽기 쉬운 프로젝트 이름, 설명, 프로젝트 홈페이지 |
| `<licenses><license/></licenses>` | 프로젝트의 라이선스 목록 |
| `<developers><developer/></developers>` | 프로젝트 개발자 목록 |
| `<scm>`의 `connection`, `developerConnection` | Maven SCM 도구에서 사용할 소스 저장소의 읽기용 연결 정보와 개발자의 쓰기용 연결 정보 |
| `<scm>`의 `tag`, `url` | 해당 버전에 대응하는 버전 관리 태그와 브라우저에서 열 수 있는 소스 저장소 주소 |

이 항목들은 문서화, 배포, 릴리스 작업에서 활용하는 선택 항목이다. 표의 `<name/>`처럼 값이 없는 XML 요소도 작성할 수 있다. [Maven 프로젝트 메타데이터](https://maven.apache.org/pom.html#More_Project_Information), [Maven SCM 설정](https://maven.apache.org/pom.html#SCM)

## 스타터와 의존성 범위

예제의 `<dependencies>`에는 `org.springframework.boot` 그룹의 두 라이브러리가 있다. 스타터는 용도에 맞게 여러 의존성을 묶어 놓은 진입점이다. 예제의 `spring-boot-starter`가 `spring-boot-autoconfigure`를 참조하고, `spring-boot-autoconfigure`가 다시 `spring-boot`를 참조한다. 이렇게 직접 선언한 라이브러리가 필요로 해 함께 들어오는 라이브러리를 **전이 의존성**이라고 한다. 그래서 `Application.java`에서 `spring-boot`의 `SpringApplication`과 `spring-boot-autoconfigure`의 `@SpringBootApplication`을 사용할 수 있다. [기본 스타터 구성](https://raw.githubusercontent.com/spring-projects/spring-boot/v4.1.2/starter/spring-boot-starter/build.gradle), [자동 설정 모듈 구성](https://raw.githubusercontent.com/spring-projects/spring-boot/v4.1.2/core/spring-boot-autoconfigure/build.gradle), [Maven 전이 의존성](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#transitive-dependencies)

| 선언한 의존성 | `scope` | 가져오는 기능 |
| --- | --- | --- |
| `spring-boot-starter` | 생략되어 `compile` | Spring Boot의 기본 실행, 자동 설정 지원, 로깅, YAML 지원 |
| `spring-boot-starter-test` | `test` | JUnit Jupiter, Hamcrest, Mockito 등 테스트 도구 |

`scope`는 해당 라이브러리를 사용할 수 있는 클래스패스 범위에 영향을 준다. `compile`은 기본값으로 컴파일과 실행에 사용할 수 있고, `runtime`은 실행과 테스트에 사용할 수 있다. `test`는 테스트 컴파일과 실행에서 사용한다. `provided`는 컴파일과 테스트에 사용하며, 일반적인 실행 시에는 JDK나 컨테이너가 제공할 것으로 간주한다. [Maven 의존성 범위](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#dependency-scope)

> [!note] 추가 설명
> 기본 스타터 두 개로 구성된 예제는 일반 Spring Boot 앱으로 시작한다. Spring Boot 4의 서블릿 기반 HTTP 앱을 만들 때는 Spring MVC와 내장 Tomcat을 제공하는 `spring-boot-starter-webmvc`를 사용할 수 있다. [Spring Boot 스타터 목록](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)

## 빌드 플러그인과 확인 명령

`dependencies`는 애플리케이션과 테스트에서 쓸 라이브러리를 선언한다. `build/plugins`는 Maven이 수행할 작업을 설정한다. 예제의 `spring-boot-maven-plugin`에는 앱을 실행하는 `run`과, Maven이 만든 JAR를 `java -jar`로 실행할 수 있도록 필요한 의존성과 실행 정보를 담아 다시 포장하는 `repackage`가 있다. `package`는 Maven 빌드 단계이며, 이 예제에서는 부모 POM에서 상속한 실행 구성으로 `repackage`까지 이어진다. [Maven 기본 빌드 생명주기](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html), [Spring Boot Maven 실행](https://docs.spring.io/spring-boot/maven-plugin/run.html), [실행 가능한 JAR 패키징](https://docs.spring.io/spring-boot/maven-plugin/packaging.html)

| 프로젝트 루트에서 실행할 명령 | 확인할 수 있는 내용 |
| --- | --- |
| `mvn -version` | Maven 버전, Maven을 실행 중인 Java 버전과 위치 |
| `mvn help:effective-pom` | 상속, 기본값, 활성 프로필을 반영한 최종 POM |
| `mvn dependency:tree` | 직접 선언한 의존성과 전이 의존성의 경로 및 버전 |
| `mvn spring-boot:run` | 컴파일한 앱을 로컬에서 시작 |
| `mvn package` | 컴파일·테스트·패키징을 실행해 `target/` 아래에 산출물 생성 |
| `java -jar target/yerdle-backend-0.0.1-SNAPSHOT.jar` | 예제의 실행 가능한 JAR를 만든 뒤 해당 파일로 앱 시작 |

Wrapper가 포함된 프로젝트라면 앞의 `mvn` 자리에 macOS/Linux에서는 `./mvnw`, Windows PowerShell에서는 `.\mvnw.cmd`를 써서 같은 작업을 할 수 있다. [Apache Maven Wrapper](https://maven.apache.org/tools/wrapper/), [최종 POM 출력](https://maven.apache.org/plugins/maven-help-plugin/effective-pom-mojo.html), [의존성 트리 출력](https://maven.apache.org/plugins/maven-dependency-plugin/tree-mojo.html)

## 복습 질문

- `4.0.0`, `4.1.2`, `0.0.1-SNAPSHOT`은 각각 무엇의 버전인가?
- 스타터 선언에 `<version>`을 생략했는데 Maven은 어떻게 버전을 결정할까?
- POM에 `spring-boot-starter`를 선언하면 `SpringApplication`이 들어 있는 JAR는 어떤 의존성 경로로 따라올까?
- `mvn spring-boot:run`과 `mvn package`를 실행한 뒤 각각 무엇을 확인할 수 있을까?
