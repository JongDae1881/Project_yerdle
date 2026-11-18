# IoC·DI와 Spring 컨테이너

[원본 대화 · 기본 Application 코드 설명 1탄](chatgpt-conversation://6afb11c3-6230-83ee-beaf-e75331460f92)

> 스프링 프레임워크의 핵심 설계 철학인 제어의 역전과 의존성 주입을 설명하고 이에 의거해서 Spring 컨테이너가 무엇이고 왜 필요한지 설명해주세요.

## RoomService가 방 코드 생성기를 직접 만들면 어떻게 될까?

Yerdle에서 `RoomService`는 방을 만들 때 `RoomCodeGenerator`에 방 코드를 요청한다. 이때 서비스가 사용하는 생성기가 **의존성**이다. 서비스 안에 `new RoomCodeGenerator()`를 두면 서비스가 방 만들기와 생성기 선택을 함께 맡는다. 생성기 구현을 바꾸려면 서비스를 수정해야 하고, 테스트에서 고정된 코드를 내놓는 생성기를 넣을 자리도 필요해진다.

## 생성자 주입으로 누가 객체를 연결할까?

서비스가 생성자에서 필요한 객체를 받으면, 호출하는 쪽이 생성기를 골라 전달한다. 아래 예제에서는 일반 Java 코드인 `main()`이 그 일을 맡는다. 나중에 테스트에서는 `generate()`를 재정의한 생성기를 만들어 같은 생성자에 넘길 수 있다.

```java
class RoomExample {
    public static void main(String[] args) {
        RoomCodeGenerator generator = new RoomCodeGenerator();
        RoomService service = new RoomService(generator);
        System.out.println(service.createRoomCode());
    }
}

class RoomCodeGenerator {
    String generate() {
        return "AB12CD";
    }
}

class RoomService {
    private final RoomCodeGenerator generator;

    RoomService(RoomCodeGenerator generator) {
        this.generator = generator;
    }

    String createRoomCode() {
        return generator.generate();
    }
}
```

| 개념 | 예제에서 확인할 부분 |
| --- | --- |
| IoC(Inversion of Control, 제어의 역전) | 생성기를 선택하고 서비스에 연결하는 제어가 `RoomService` 밖으로 옮겨간다. 수동 예제에서는 `main()`이 맡는다. |
| DI(Dependency Injection, 의존성 주입) | `RoomService`가 필요한 생성기를 생성자 인자로 받는다. 생성자 주입은 객체 구성에서 IoC를 구현하는 방법이다. |

생성자 매개변수를 보면 서비스에 꼭 필요한 객체를 알 수 있다. 호출자가 인자를 공급하는 이 방식은 순수 Java에서도 쓸 수 있고, Spring을 쓰면 컨테이너가 호출자 역할을 맡을 수 있다. [Spring의 의존성 주입](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html)

## Spring 컨테이너에 맡기면 무엇이 달라질까?

앱에 `RoomController`가 추가되고 컨트롤러가 `RoomService`를 필요로 하면, 앱을 시작하는 쪽에서 생성기, 서비스, 컨트롤러를 차례로 만들고 연결해야 한다. 객체가 많아져도 각 클래스는 필요한 의존성을 생성자에 표현하고, 연결은 Spring의 IoC 컨테이너에 맡길 수 있다. Spring 애플리케이션은 보통 `ApplicationContext`를 통해 이 컨테이너를 사용한다. [Spring IoC 컨테이너와 Bean](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html)

1. 컨테이너는 설정에서 관리할 객체의 정의를 읽는다. 여기에는 어떤 타입의 객체를 어떻게 만들지에 대한 정보가 들어 있다.
2. `RoomService`를 만들 때 생성자에 필요한 `RoomCodeGenerator`를 찾고, 그에 해당하는 관리 객체를 전달한다. `RoomController`가 `RoomService`를 필요로 하면 같은 방식으로 연결한다.
3. 컨테이너가 관리하는 객체를 **Spring Bean**이라고 한다. 컨테이너는 Bean에 지정된 초기화와 종료 작업도 생명주기에 맞춰 호출할 수 있다. [Spring Bean 생명주기](https://docs.spring.io/spring-framework/reference/core/beans/factory-nature.html)

이 예제의 생성기와 서비스를 실제로 등록하는 방법은 [[05 · Spring Bean과 자바 설정|Spring Bean과 자바 설정]]에 있다. `SpringApplication`이 이 컨테이너를 시작하는 과정은 [[02 · 스프링 부트 웹 앱 학습 노트#main에서 왜 자기 클래스 정보를 넘길까|스프링 부트 웹 앱 학습 노트]]에서 이어서 볼 수 있다.

## 복습 질문

- `RoomService` 안에서 생성기를 만들 때와 생성자로 받을 때, 생성기를 바꾸는 코드는 각각 어디에 있을까?
- 순수 Java 예제에서는 어느 코드가 의존성을 주입할까? Spring 앱에서는 무엇이 그 역할을 맡을까?
- `RoomController`가 목록을 조회하기 전, Spring 컨테이너는 컨트롤러와 서비스 사이에 어떤 연결을 준비할까?
