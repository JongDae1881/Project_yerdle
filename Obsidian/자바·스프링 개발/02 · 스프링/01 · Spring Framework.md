# Spring Framework

> [!info] 관련 개념
> - [[02 · Spring Boot|Spring Boot]] — Spring 앱을 구성하고 실행하기 위한 기본 설정
> - [[03 · 라이브러리와 프레임워크#모듈의 뜻과 역할|라이브러리·프레임워크·모듈 비교]] — Spring을 구성하는 모듈의 의미
> - [[04 · IoC·DI와 Spring 컨테이너|IoC·DI와 Spring 컨테이너]] — 객체의 생성과 연결을 컨테이너에 맡기는 이유
> - [[04 · MVC|MVC]] — 웹 요청 처리에서 모델, 뷰, 컨트롤러의 책임

[원본 대화 · Git 확인 및 Maven 설치](chatgpt-conversation://6af7f38b-b0c0-83ee-b0ca-a57f4131d900)

> 스프링이 있는데 왜 스프링부트를 쓰는 건가요? 차이가 뭔가요?

Spring Framework는 Java 애플리케이션의 객체 관리, 웹 요청 처리, 트랜잭션 관리에 필요한 기능을 제공하는 프레임워크다. 이 노트에서 “Spring”은 Spring Framework를 가리킨다. [Spring Framework 개요](https://docs.spring.io/spring-framework/reference/overview.html)

| 기능 | 꼬들 프로젝트에서 할 수 있는 일 |
| --- | --- |
| IoC 컨테이너와 의존성 주입(DI) | Spring에 등록된 `RoomController`가 `RoomService`를 사용하도록 객체를 생성하고 연결한다. |
| Spring MVC | `GET /rooms`가 들어오면 그 요청에 매핑된 컨트롤러 메서드를 호출한다. |
| 트랜잭션 관리 | 게임방 생성과 방장 등록 같은 데이터베이스 작업을 한 트랜잭션으로 묶을 수 있게 지원한다. |

예시 컨트롤러에서는 `@RestController`가 웹 요청을 처리할 클래스를 나타내고, `@GetMapping("/rooms")`이 URL과 HTTP 메서드를 지정한다.

```java
@RestController
public class RoomController {
    @GetMapping("/rooms")
    public String getRooms() {
        return "게임방 목록";
    }
}
```

내장 서버가 요청을 받은 뒤 Spring MVC에 전달하면, Spring MVC는 요청 정보로 `getRooms()`를 찾고 호출한다. `@RestController`를 사용했으므로 이 예시의 반환 문자열은 응답 본문에 쓰인다. [Spring MVC 요청 매핑](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html), [Spring MVC 응답 본문](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/responsebody.html)

## Spring Framework의 모듈

Spring Framework는 관련 기능을 여러 모듈로 나누어 배포한다. 꼬들 앱에서 Spring이 객체를 관리하는 일과 웹 요청을 처리하는 일은 서로 다른 모듈의 기능을 사용한다.

| 모듈 | 맡는 기능 | 꼬들 앱에서 쓰이는 예 |
| --- | --- | --- |
| `spring-context` | 앱이 사용하는 객체를 관리하는 `ApplicationContext`를 제공한다. | Spring에 등록된 `RoomController`와 `RoomService`를 관리하고 연결한다. |
| `spring-webmvc` | Spring MVC의 웹 요청 처리 기능을 제공한다. | `GET /rooms`에 매핑된 `getRooms()`를 찾아 호출한다. |

`spring-webmvc`의 컴파일된 코드 등은 `spring-webmvc-<버전>.jar` 파일에 담겨 배포된다. [[01 · Maven|Maven]]이나 [[02 · Gradle|Gradle]]은 필요한 모듈의 JAR를 프로젝트 의존성으로 가져온다. [Spring Framework 개요](https://docs.spring.io/spring-framework/reference/overview.html), [Spring IoC 컨테이너](https://docs.spring.io/spring-framework/reference/core/beans.html), [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html)

## 복습 질문

- `GET /rooms`가 들어왔을 때 어떤 코드가 처리할 메서드를 찾아 호출할까?
- `RoomController`가 필요로 하는 `RoomService` 객체를 연결하는 일은 Spring의 어떤 기능이 맡을까?
- 객체를 관리하는 `ApplicationContext`와 웹 요청을 처리하는 Spring MVC는 각각 어느 모듈에서 제공할까?
