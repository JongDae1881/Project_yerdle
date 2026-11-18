# SpringApplication과 시작 클래스

[원본 대화 · 기본 Application 코드 설명 2탄](chatgpt-conversation://6afc5ad6-a500-83e8-b9c9-1bfdc982041f) · [원본 대화 · 기본 Application 코드 설명 1탄](chatgpt-conversation://6afb11c3-6230-83ee-beaf-e75331460f92)

> 아니 근데 뭔가 Application 클래스 내의 main 함수의 스프링 애플리케이션 실행을 위해 Application 클래스를 넘긴다는 게 뭔가 이상해 보여요

```java
package com.yerdle.backend;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class YerdleBackendApplication {
    public static void main(String[] args) {
        SpringApplication.run(YerdleBackendApplication.class, args);
    }
}
```

## main과 .class가 맡는 역할

JVM이 `YerdleBackendApplication.main()`을 호출하면 본문에 있는 호출식이 `SpringApplication.run()`을 실행한다. `YerdleBackendApplication.class`는 인자의 값을 구하는 식이다. 이 식을 평가하면 그 클래스를 나타내는 `Class<YerdleBackendApplication>` 객체가 나오고, Spring은 이 값을 구성 정보를 읽기 시작할 설정 소스로 사용한다. 기본 코드는 한 클래스에 Java 실행 진입점과 Spring 설정 소스를 함께 둔다. [Java 언어 명세의 클래스 리터럴](https://docs.oracle.com/javase/specs/jls/se24/html/jls-15.html#jls-15.8.2), [SpringApplication의 run API](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/SpringApplication.html)

| `@SpringBootApplication`에 포함된 애너테이션 | 맡는 일 |
| --- | --- |
| `@SpringBootConfiguration` | 이 클래스를 Boot의 주 설정으로 표시한다. Java 기반 설정에 쓰이는 `@Configuration`을 포함한다. |
| `@ComponentScan` | 이 패키지와 그 하위 패키지의 컴포넌트를 찾는 [[06 · 컴포넌트 스캔\|컴포넌트 스캔]]을 켠다. |
| `@EnableAutoConfiguration` | 조건에 맞는 기본 구성을 고르는 [[07 · Spring Boot 자동 설정\|Spring Boot 자동 설정]]을 켠다. |

애너테이션은 Spring이 설정으로 읽을 정보를 표시한다. `SpringApplication.run()`은 그 정보를 사용해 컨텍스트를 시작한다. [SpringBootApplication의 구성](https://docs.spring.io/spring-boot/reference/using/using-the-springbootapplication-annotation.html)

## 진입점과 설정을 분리한 예

`YerdleBackendApplication.java`에 아래처럼 두 클래스를 두면 JVM은 여전히 `YerdleBackendApplication.main()`부터 실행한다. `run()`에 넘겨준 `AppConfig.class`는 Spring이 설정을 읽을 대상이다. 두 클래스가 같은 패키지에 있어서 `AppConfig`의 기본 컴포넌트 스캔 범위는 앞 예시와 같다. [SpringApplication API](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/SpringApplication.html), [Spring Boot 패키지 구조](https://docs.spring.io/spring-boot/reference/using/structuring-your-code.html)

```java
package com.yerdle.backend;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
class AppConfig {
}

public class YerdleBackendApplication {
    public static void main(String[] args) {
        SpringApplication.run(AppConfig.class, args);
    }
}
```

## run이 시작하는 것과 돌려주는 것

`SpringApplication`은 명령행 인자와 설정을 읽고 앱 유형에 맞는 `ApplicationContext`를 준비한다. 컨텍스트를 초기화하면서 설정 클래스, 컴포넌트 스캔, 자동 설정을 처리하고 Bean을 생성해 연결한다. 완료하면 호출자에게 `ConfigurableApplicationContext`를 반환하므로 코드에서 등록된 Bean을 조회할 수도 있다. `args`는 JVM이 `main()`에 넘긴 인자 배열이다. 예를 들어 `java -jar app.jar --server.port=8081`로 실행한 내장 서버 앱에서 Boot는 `--server.port=8081`을 설정으로 읽는다. [SpringApplication API](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/SpringApplication.html), [Spring Boot 내장 서버 포트](https://docs.spring.io/spring-boot/how-to/webserver.html)

내장 Tomcat으로 구성한 서블릿 웹 앱에서는 시작 과정에서 HTTP 서버도 뜬다. 대화의 기본 `spring-boot-starter` 예제는 HTTP 서버 없이 컨텍스트를 시작할 수 있다. `main()`이 끝났고 JVM을 계속 실행할 작업도 없다면 초기화를 마치고 정상 종료될 수 있다. 시작부터 HTTP 요청까지 이어지는 예는 [[02 · 스프링 부트 웹 앱 학습 노트|스프링 부트 웹 앱 학습 노트]]에서 볼 수 있다. [SpringApplication의 웹 앱 유형](https://docs.spring.io/spring-boot/reference/features/spring-application.html), [Spring Boot의 내장 웹 서버](https://docs.spring.io/spring-boot/how-to/webserver.html)

## 복습 질문

- `YerdleBackendApplication.class`를 평가한 결과는 무엇이고 Boot는 거기서 무엇을 읽을까?
- `main()`의 위치와 `run()`에 넘기는 설정 클래스를 분리하면 누가 각각을 사용할까?
- `--server.port=8081`은 어떤 경로로 Boot 설정에 들어가고, 웹 서버가 없이 시작한 앱은 언제 정상 종료될 수 있을까?
