# Spring Bean과 자바 설정

[원본 대화 · 기본 Application 코드 설명 1탄](chatgpt-conversation://6afb11c3-6230-83ee-beaf-e75331460f92)

> 기초부터 공부하는 입장에서 스프링부트에서 @Configuration과 @Bean에 대해서부터 설명해주면 좋겠습니다.

## 어떤 객체를 Spring Bean이라고 부를까?

**Spring Bean**은 Spring 컨테이너가 관리하는 객체다. 예를 들어 `main()`에서 `new RoomCodeGenerator()`를 호출하고 코드에서 직접 쓰면 그 객체는 일반 Java 객체다. 동일한 `new RoomCodeGenerator()`를 Spring이 호출하는 `@Bean` 메서드 안에서 반환하면, 컨테이너는 그 객체를 `roomCodeGenerator`라는 이름으로 관리할 수 있다. **Bean 정의**는 객체를 만드는 방법과 이름 등을 기록한 정보이고, **Bean 인스턴스**는 그 정의를 바탕으로 만들어진 실제 객체다. [Spring IoC 컨테이너와 Bean](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html), [Spring의 Bean 정의와 범위](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html)

## @Configuration과 @Bean은 어디에 붙일까?

같은 패키지에 `RoomCodeGenerator`와 생성자로 이를 받는 `RoomService`가 있다고 하자. 두 객체의 Java 코드는 [[04 · IoC·DI와 Spring 컨테이너|IoC·DI와 Spring 컨테이너]]에서 볼 수 있다. 아래 설정은 두 클래스를 Spring이 관리하도록 등록한다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
class RoomBeanConfig {
    @Bean
    RoomCodeGenerator roomCodeGenerator() {
        return new RoomCodeGenerator();
    }

    @Bean
    RoomService roomService(RoomCodeGenerator generator) {
        return new RoomService(generator);
    }
}
```

| 코드 | 하는 일 |
| --- | --- |
| 클래스의 `@Configuration` | `RoomBeanConfig`가 Bean 정의를 제공하는 설정 클래스임을 표시한다. |
| 메서드의 `@Bean` | 메서드가 반환할 객체의 Bean 정의를 등록한다. 이름을 따로 지정하지 않으면 메서드 이름이 기본 Bean 이름이다. |
| `roomService(RoomCodeGenerator generator)` | Spring이 이 메서드를 호출할 때 필요한 `RoomCodeGenerator` Bean을 인자로 받는다. 서비스의 생성자에는 그 인자가 전달된다. |

Spring이 이 설정을 처리하려면 `RoomBeanConfig`가 컨텍스트에 포함되어야 한다. 예를 들어 앱의 기본 스캔 범위 안에 설정 클래스를 두거나, 앱의 설정 클래스에 `@Import(RoomBeanConfig.class)`를 선언할 수 있다. `@SpringBootApplication`은 `@SpringBootConfiguration`을 포함하므로 이 애너테이션이 붙은 클래스에도 `@Bean` 메서드를 선언할 수 있다. [Spring의 자바 기반 설정](https://docs.spring.io/spring-framework/reference/core/beans/java/basic-concepts.html), [Bean 메서드 인자와 이름](https://docs.spring.io/spring-framework/reference/core/beans/java/bean-annotation.html), [Spring의 `@Import`](https://docs.spring.io/spring-framework/reference/core/beans/java/composing-configuration-classes.html#beans-java-using-import), [Spring Boot의 앱 설정 클래스](https://docs.spring.io/spring-boot/reference/using/using-the-springbootapplication-annotation.html)

## Spring은 Bean을 언제 만들고 얼마나 공유할까?

1. 컨텍스트가 설정을 읽고 `roomCodeGenerator`와 `roomService`의 Bean 정의를 등록한다.
2. 컨테이너는 `roomService`를 만드는 데 필요한 생성기 Bean을 준비하고, `roomService(generator)`를 호출한다. 반환된 서비스를 관리하고 다른 Bean이 요청하면 주입할 수 있다.
3. 기본 범위인 `singleton`에서는 **같은 컨테이너에서 Bean 정의 하나당 인스턴스 하나**를 공유한다. 기본적으로 지연 초기화 설정이 없는 싱글턴 Bean은 컨텍스트 초기화 중에 생성된다. 초기화나 종료 콜백을 지정하면 Spring이 해당 시점에 호출한다. [Spring Bean 범위](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html), [지연 초기화](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-lazy-init.html), [Spring Bean 생명주기](https://docs.spring.io/spring-framework/reference/core/beans/factory-nature.html)

객체를 만드는 과정에 인자 설정이나 팩터리 호출이 필요할 때는 `@Bean` 메서드에 그 코드를 적을 수 있다. 소스에 표시를 붙일 수 있는 애플리케이션 클래스는 `@Service` 같은 애너테이션과 [[06 · 컴포넌트 스캔|컴포넌트 스캔]]으로도 등록할 수 있다. 이 두 방식이 Boot 앱 시작 흐름에서 어떻게 쓰이는지는 [[02 · 스프링 부트 웹 앱 학습 노트#Spring은 왜 객체를 관리할까|스프링 부트 웹 앱 학습 노트]]에서 확인할 수 있다. [Spring 클래스패스 스캔과 컴포넌트](https://docs.spring.io/spring-framework/reference/core/beans/classpath-scanning.html)

## 복습 질문

- `new RoomCodeGenerator()`로 만든 객체를 Spring Bean으로 보려면 어떤 조건이 필요할까?
- `@Configuration`은 클래스에, `@Bean`은 메서드에 붙인다. Spring은 각각에서 어떤 정보를 얻을까?
- `roomService(RoomCodeGenerator generator)`의 인자를 누가 준비할까? 같은 컨테이너에서 생성기 Bean을 두 곳에서 사용하면 기본 범위에서는 몇 개의 인스턴스를 쓸까?
