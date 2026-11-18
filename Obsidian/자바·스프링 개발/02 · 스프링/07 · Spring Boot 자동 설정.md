# Spring Boot 자동 설정

[원본 대화 · 기본 Application 코드 설명 1탄](chatgpt-conversation://6afb11c3-6230-83ee-beaf-e75331460f92) · [원본 대화 · 기본 Application 코드 설명 2탄](chatgpt-conversation://6afc5ad6-a500-83e8-b9c9-1bfdc982041f)

> 기본 Application.java 기본 코드에서 왜 굳이 ComponentScan을 하고, AutoConfiguration을 하는 거죠? 둘 다 패키지 내 다른 곳에서 해도 되는 거 아닌가요?

## 자동으로 무엇을 설정할까?

HTTP API를 제공하려면 내가 작성한 `RoomController`와 함께 요청을 받을 서버, 요청을 컨트롤러에 전달할 Spring MVC 구성 등이 필요하다. Boot는 이런 공통 구성을 재사용할 수 있도록 자동 설정 후보를 제공한다. `@EnableAutoConfiguration`을 활성화하면 Boot가 후보의 조건을 평가하고, 조건이 맞는 설정을 앱의 컨텍스트에 적용한다. `@SpringBootApplication`에도 이 기능이 포함되어 있다. [Spring Boot의 자동 설정](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)

| 조건의 예 | Boot가 확인하는 것 |
| --- | --- |
| `@ConditionalOnClass` | 필요한 타입이 실행 클래스패스에 있는가? |
| `@ConditionalOnMissingBean` | 기본값으로 제공하려는 Bean에 해당하는 등록이 아직 없는가? |
| `@ConditionalOnProperty` | 기능에 해당하는 설정값이 조건을 만족하는가? |
| `@ConditionalOnWebApplication` | 대상이 되는 종류의 웹 애플리케이션인가? |

어떤 조건을 사용하는지는 후보마다 다르다. 예를 들어 내장 데이터베이스용 `DataSource`를 제공하는 기본 구성이 기존 Bean의 유무를 확인한다면, 앱이 직접 `DataSource` Bean을 정의했을 때 그 기본 구성은 물러난다. [Spring Boot의 자동 설정 조건](https://docs.spring.io/spring-boot/reference/features/developing-auto-configuration.html), [사용자 설정으로 기본값 재정의](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)

## 웹 스타터를 추가하면 무엇이 달라질까?

대화의 Yerdle 예제는 Spring Boot `4.1.2`와 `spring-boot-starter`로 시작했다. 이 기본 구성으로 Spring 컨텍스트를 시작할 수 있다. `GET /rooms`를 서블릿 웹 앱으로 제공하는 경우, `spring-boot-starter-webmvc`를 추가한 뒤의 흐름은 다음과 같다. [Spring Boot 스타터 목록](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters), [Spring Boot의 내장 웹 서버](https://docs.spring.io/spring-boot/how-to/webserver.html)

| 시점 | 예제에서 일어나는 일 |
| --- | --- |
| Maven이 의존성을 가져올 때 | 웹 스타터가 가져오는 Spring MVC와 기본 내장 Tomcat 라이브러리가 실행 클래스패스에 들어간다. |
| Spring Boot가 시작할 때 | 서블릿 웹 앱에 해당하는 조건을 평가해 내장 서버와 `DispatcherServlet` 등 웹 구성을 적용한다. |
| 내 코드가 컨텍스트에 등록될 때 | [[06 · 컴포넌트 스캔\|컴포넌트 스캔]] 범위의 `RoomController`를 찾아 Bean으로 등록할 수 있다. |

의존성 선언, 자동 설정, 컴포넌트 스캔은 각각 라이브러리 확보, 기본 구성 선택, 내 컴포넌트의 발견을 맡는다. POM에서 스타터가 무엇을 가져오는지는 [[04 · Spring Boot의 pom.xml|Spring Boot의 pom.xml]]에서, 이 과정이 시작되는 시점은 [[08 · SpringApplication과 시작 클래스|SpringApplication과 시작 클래스]]에서 확인할 수 있다.

## 어디에서 활성화하고 적용 결과는 어떻게 볼까?

`@EnableAutoConfiguration`은 Spring이 읽는 주 설정 클래스에서 한 번 활성화하는 것이 일반적이다. `main()`이 든 클래스와 설정 클래스를 따로 만들었다면 주 설정 클래스에서 활성화할 수 있다. 이를 붙인 클래스의 패키지는 일부 기능의 기본 탐색 패키지로도 쓰인다. 예를 들어 JPA 관련 구성이 활성화되면 엔티티를 그 패키지 아래에서 찾을 수 있으므로 설정 클래스의 위치를 옮길 때 이 범위도 확인한다. [Spring Boot의 자동 설정 패키지](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html#using.auto-configuration.packages)

자동 설정이 왜 적용됐는지 보려면 앱 실행 인자로 `--debug`를 전달한다. 콘솔에 출력되는 조건 평가 보고서에서 적용된 후보와 조건을 충족하지 못한 후보를 볼 수 있다. [Spring Boot의 조건 평가 보고서](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html), [Maven 플러그인의 앱 실행 인자](https://docs.spring.io/spring-boot/maven-plugin/run.html)

```powershell
# 실행 가능한 JAR를 만든 경우
java -jar app.jar --debug

# Spring Boot Maven 플러그인과 Windows Wrapper를 사용하는 경우
.\mvnw.cmd spring-boot:run '-Dspring-boot.run.arguments=--debug'
```

## 복습 질문

- `@EnableAutoConfiguration`을 켜 둔 상태에서 기본 스타터만 사용하던 예제에 `spring-boot-starter-webmvc`를 추가하면 어떤 조건이 달라질까?
- `RoomController`를 찾는 일과 `DispatcherServlet`의 기본 구성을 준비하는 일은 각각 어떤 기능이 맡을까?
- 기본 설정이 기대한 대로 적용됐는지 확인하려면 어떤 인자를 주고 로그의 무엇을 살펴볼까?
