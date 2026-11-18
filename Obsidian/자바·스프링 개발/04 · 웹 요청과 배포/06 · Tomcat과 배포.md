# Tomcat과 배포

> [!info] 관련 개념
> - [[02 · 웹 서버와 WAS|웹 서버와 WAS]] — HTTP 서버와 애플리케이션 실행 환경의 역할
> - [[04 · Spring Boot의 pom.xml|Spring Boot의 pom.xml]] — Maven 패키징과 실행 가능한 JAR
> - [[07 · Spring Boot 자동 설정|Spring Boot 자동 설정]] — 내장 서버 구성이 적용되는 조건

[원본 대화 · 톰캣 서버 사용 여부](chatgpt-conversation://6afda0d7-7cd8-83e8-8daa-0e5eeac4f68a) · [원본 대화 · 서블릿 작동 원리](chatgpt-conversation://6afd932f-a1f8-83ec-8197-28acc533fcba)

> 톰캣 "서버"라고 하는데, 만약 제가 만든 프로그램을 localhost가 아닌 외부의 다른 컴퓨터와 인터넷으로 통신할 때도 톰캣 서버를 쓰나요? 개발 컴퓨터가 아닌 다른 서버 컴퓨터에 배포할 때도 톰캣 서버를 쓰나요?

## 누가 어느 주소로 접속할까?

Tomcat을 쓰는 Java 웹 앱은 개발 컴퓨터에서 실행할 수도 있고, 별도 서버 컴퓨터에서 실행할 수도 있다. **서버 컴퓨터**는 앱이 실행되는 물리 또는 가상 머신이고, **서버 프로그램**인 Tomcat은 그 머신에서 HTTP 요청을 받아 웹 앱 코드를 실행한다. 브라우저는 Tomcat으로 직접 연결하거나, 요청을 전달해 주는 앞단 서버에 연결한다. [Tomcat HTTP 커넥터](https://tomcat.apache.org/tomcat-11.0-doc/config/http.html), [Apache HTTP Server의 리버스 프록시](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html)

| 접속 상황 | 브라우저 주소 예시 | 요청이 도착하는 경로 |
| --- | --- | --- |
| 개발 PC에서 직접 확인 | `http://localhost:8080` | 개발 PC 자신의 8080번 포트에서 대기하는 서버로 간다. |
| 같은 네트워크의 다른 PC에서 확인 | `http://192.168.0.10:8080` | 해당 IP를 가진 개발 PC로 간다. 서버가 그 네트워크 인터페이스에서 받고 방화벽 등에서 허용해야 한다. |
| 인터넷에서 배포한 웹 앱 사용 | `https://api.example.com` | 도메인이 가리키는 서버나 앞단 프록시에 접속한다. 앞단 프록시가 있으면 그곳에서 Tomcat으로 전달할 수 있다. |

친구가 자기 PC에서 `localhost`를 열면 친구 PC로 접속한다. 독립 실행형 Spring Boot 웹 앱의 기본 HTTP 포트는 8080이며, `server.port`로 바꿀 수 있다. [RFC 6761의 localhost 정의](https://www.rfc-editor.org/rfc/rfc6761.html#section-6.3), [Spring Boot 내장 웹 서버 설정](https://docs.spring.io/spring-boot/how-to/webserver.html)

## 내장 Tomcat과 외장 Tomcat은 누가 시작할까?

| 배포 예시 | 배포할 산출물 | 실행의 출발점 |
| --- | --- | --- |
| 내장 Tomcat을 포함한 Spring Boot 웹 앱 | 실행 가능한 JAR | 서버에서 `java -jar app.jar`로 앱을 시작한다. Spring Boot가 같은 JVM에서 내장 Tomcat을 시작한다. |
| 외장 Tomcat에 올리는 서블릿 웹 앱 | WAR | 서버에 설치한 Tomcat이 WAR를 배포받아 앱을 로딩하고 실행한다. Eclipse의 `Run on Server`는 실습에서 이 과정을 대신 수행할 수 있다. |

대화에 나온 Boot POM의 플러그인 구성에서는 Maven의 `package` 단계에서 `repackage`가 실행되어 실행 가능한 JAR가 만들어진다. `3logindao`처럼 WAR로 구성된 앱은 호환되는 Servlet 규격의 외장 컨테이너에 배포할 수 있다. Spring Boot의 서블릿 웹 앱도 WAR로 배포할 수 있으며, `SpringBootServletInitializer`로 앱의 초기화를 연결하고 내장 컨테이너 의존성의 범위를 `provided`로 지정한다. [Spring Boot 실행 가능한 아카이브](https://docs.spring.io/spring-boot/maven-plugin/packaging.html), [Spring Boot의 외장 컨테이너 배포](https://docs.spring.io/spring-boot/how-to/deployment/traditional-deployment.html)

내장 Tomcat을 포함한 JAR를 개발 PC에서 실행해도, 같은 JAR를 배포 서버에서 실행해도 Spring Boot가 Tomcat을 시작한다. Docker 이미지에 JAR를 넣는 방식도 가능하다. 이 경우 앱과 Tomcat은 함께 컨테이너 안에서 실행되며, 외부에서 접속하려면 포트 게시나 프록시 연결 등 접속 경로가 필요하다. [Docker의 포트 게시](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/)

> [!note] 추가 설명
> 우리 앱이 다른 서비스에 HTTP 요청을 먼저 보내는 상황에서는 앱이 클라이언트 역할을 한다. 예를 들어 Java의 `HttpClient`로 외부 API를 요청할 수 있다. Tomcat으로 들어오는 요청을 처리하는 중에 이 클라이언트 호출을 할 수도 있다. [Java HttpClient API](https://docs.oracle.com/en/java/javase/21/docs/api/java.net.http/java/net/http/HttpClient.html)

## 복습 질문

- 친구가 자기 컴퓨터에서 `http://localhost:8080`을 입력하면 어느 컴퓨터를 찾을까?
- 개발 PC에서 실행하던 Boot JAR를 서버에 옮겼을 때, 어떤 프로그램이 Tomcat을 시작할까?
- WAR를 외장 Tomcat에 배포하면 앱을 로딩하는 주체가 어떻게 달라질까?
- 인터넷 사용자가 우리 앱을 부르는 요청과 우리 앱이 외부 API를 부르는 요청에서 각각 어느 쪽이 클라이언트일까?
