# Spring Boot

> [!info] 관련 개념
> - [[01 · Spring Framework|Spring Framework]] — 객체 관리와 웹 요청 처리를 제공하는 기반
> - [[01 · Maven|Maven]] — 웹 스타터의 의존성을 가져오고 Boot 플러그인으로 앱을 실행하는 도구
> - [[07 · Spring Boot 자동 설정|Spring Boot 자동 설정]] — 클래스패스와 조건에 따라 적용할 설정을 고르는 과정
> - [[08 · SpringApplication과 시작 클래스|SpringApplication과 시작 클래스]] — JVM의 `main()`에서 Spring 컨텍스트를 시작하는 과정

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 스프링이 있는데 왜 스프링부트를 쓰는 건가요? 차이가 뭔가요?

Spring Boot는 Spring Framework를 기반으로 앱을 구성하고 실행하는 일을 간소화하는 프로젝트다. 예를 들어 꼬들에서 `GET /rooms`를 제공하려면 Spring MVC와 서버 관련 의존성을 선택하고 어떤 방식으로 서버를 시작할지 정해야 한다. Spring Boot는 이 준비에 필요한 기본 구성을 제공한다.

| 기능 | 적용되는 시점과 효과 |
| --- | --- |
| 스타터(Starter) | 프로젝트 설정에 웹 스타터를 선언하면 빌드 도구가 Spring MVC 등 관련 의존성을 함께 가져온다. |
| 자동 설정(Auto-configuration) | 앱을 시작할 때 클래스패스와 사용자 설정 등을 보고 조건에 맞는 기본 구성을 적용한다. |
| 내장 웹 서버 | 일반적인 Spring MVC 앱에서는 내장 Tomcat 같은 서버를 앱과 함께 시작할 수 있다. |
| 빌드 플러그인 | Maven이나 [[02 · Gradle\|Gradle]]에서 앱 실행과 실행 가능한 JAR 패키징을 지원한다. |

자동 설정은 사용자가 작성한 구성을 반영한다. 예를 들어 대응하는 설정을 직접 제공하면 조건에 따라 Boot의 기본 설정이 적용되지 않을 수 있다. 웹 앱이 시작된 뒤 `GET /rooms`를 적절한 컨트롤러로 연결하는 일은 Spring Framework의 Spring MVC가 맡는다. [Spring Boot 자동 설정](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html), [Spring Boot 빌드 도구와 스타터](https://docs.spring.io/spring-boot/reference/using/build-systems.html)

## 복습 질문

- 웹 스타터를 선언하는 일과 자동 설정이 적용되는 일은 각각 언제 일어날까?
- Spring Boot로 웹 앱을 시작한 뒤, `GET /rooms`에 맞는 메서드를 찾아 호출하는 기능은 어디에 속할까?
