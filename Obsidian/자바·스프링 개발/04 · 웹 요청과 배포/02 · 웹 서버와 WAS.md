# 웹 서버와 WAS

> [!info] 관련 개념
> - [[03 · 서블릿과 서블릿 컨테이너|서블릿과 서블릿 컨테이너]] — Tomcat이 Java 웹 코드를 실행하는 방식
> - [[06 · Tomcat과 배포|Tomcat과 배포]] — 서버 프로그램이 실행되는 장소와 배포 방식
> - [[02 · 스프링 부트 웹 앱 학습 노트|스프링 부트 웹 앱 학습 노트]] — 서버가 시작되기까지의 흐름

[원본 대화 · 서블릿 개념 설명](chatgpt-conversation://6afc6906-4790-83e8-8a8b-db65a3710e37)

> 웹서버와 WAS는 뭔가요? 톰캣은 웹서버인가요, WAS인가요?

## HTTP 요청을 받는 일과 애플리케이션을 실행하는 일

웹 앱을 방문하면 브라우저는 이미지 파일과 게임방 목록을 각각 요청할 수 있다. `/images/logo.png`는 이미 저장된 파일을 읽어 보낼 수 있다. `GET /rooms`는 현재 게임방 목록을 조회하는 애플리케이션 코드가 실행돼야 한다. 두 요청 모두 HTTP로 들어오고 브라우저는 HTTP 응답을 받는다.

| 용어 | 가리키는 역할 | 예시 |
| --- | --- | --- |
| 웹 서버 | HTTP 연결을 받고 응답을 전송한다. 파일을 읽어 직접 응답하거나 뒷단 서버로 요청을 전달할 수 있다. | Apache HTTP Server가 이미지 파일을 보내거나 API 요청을 Tomcat으로 전달한다. |
| WAS(Web Application Server) | 배포된 웹 애플리케이션을 실행하고 요청이 앱의 처리 코드에 도달할 환경을 제공한다. | Tomcat이 서블릿을 관리하고 요청에 맞는 서블릿을 호출한다. |

정적 파일과 동적 응답은 역할을 이해하는 예시다. 예를 들어 Tomcat의 기본 서블릿은 정적 파일을 제공하고, Apache HTTP Server도 모듈을 통해 동적 콘텐츠를 처리할 수 있다. 실제 구조를 읽을 때는 **어떤 프로그램이 HTTP 요청을 직접 받는지**, **어떤 프로그램에서 앱 코드가 실행되는지**를 각각 확인한다. [Tomcat의 기본 서블릿](https://tomcat.apache.org/tomcat-11.0-doc/default-servlet.html), [Apache HTTP Server의 리버스 프록시](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html)

## Tomcat은 두 역할을 어떻게 수행할까?

Tomcat의 HTTP 커넥터는 포트에서 HTTP 연결을 받아들인다. 이어서 서블릿 컨테이너가 웹 애플리케이션과 URL에 맞는 서블릿을 선택해 처리한다. Spring MVC 웹 앱에서 `GET /rooms`를 받으면 Tomcat이 `DispatcherServlet`을 호출하고, Spring MVC가 요청과 맞는 컨트롤러 메서드를 찾는다. 이렇게 HTTP 서버 기능과 Java 웹 앱의 실행 환경을 한 프로그램이 제공한다. Java 웹 개발에서 Tomcat을 WAS라고 부르는 이유다. [Tomcat HTTP 커넥터](https://tomcat.apache.org/tomcat-11.0-doc/config/http.html), [Tomcat이 구현하는 명세](https://tomcat.apache.org/whichversion.html), [Spring MVC의 DispatcherServlet](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-servlet.html)

| 배치 예시 | `GET /rooms`가 지나가는 곳 |
| --- | --- |
| Tomcat이 HTTP 요청을 직접 받는 구성 | 브라우저 → Tomcat → `DispatcherServlet` → `RoomController` |
| 앞단 웹 서버를 두는 구성 | 브라우저 → 앞단 웹 서버 → Tomcat → `DispatcherServlet` → `RoomController` |

두 번째 구성에서 앞단 웹 서버는 브라우저 대신 뒷단에 요청을 전달하고 받은 응답을 돌려준다. 이 기능을 **리버스 프록시**라고 부른다. 브라우저가 앞단으로 보낸 요청도 결국 Tomcat 안의 서블릿에 도달해 앱 코드로 이어질 수 있다. [Apache HTTP Server의 리버스 프록시](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html)

## 복습 질문

- Tomcat이 정적 이미지와 `GET /rooms` 요청을 모두 처리할 수 있는 이유는 무엇일까?
- 앞단 웹 서버를 두면 어느 프로그램이 브라우저의 HTTP 연결을 받고, 어느 프로그램이 서블릿을 실행할까?
- Tomcat의 HTTP 커넥터와 서블릿 컨테이너가 각각 맡는 일은 무엇일까?
