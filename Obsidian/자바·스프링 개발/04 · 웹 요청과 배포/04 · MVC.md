# MVC

[원본 대화 · 서블릿 작동 원리](chatgpt-conversation://6afd932f-a1f8-83ec-8197-28acc533fcba) · [원본 대화 · 서블릿 개념 설명](chatgpt-conversation://6afc6906-4790-83e8-8a8b-db65a3710e37)

> 그럼 여기서 서블릿은 뭐고, 어떻게 작동하는 거야?

이 질문에 나온 `3logindao` 실습에서 `MainServlet`은 상품을 조회하고, `list.jsp`는 조회 결과로 화면을 만든다. 이 코드에 **MVC(Model–View–Controller)**를 대입하면 HTTP 요청 처리, 업무 데이터와 규칙, 화면 표현을 누가 맡는지 알 수 있다.

## Model, View, Controller는 무엇을 맡을까?

| 구성 요소 | 역할 | `3logindao` 상품 목록에서 |
| --- | --- | --- |
| Model | 애플리케이션이 다루는 데이터와 업무 규칙을 표현한다. | `Product`와 상품 목록, 목록을 구하는 서비스와 데이터 접근 코드가 이쪽에 속한다. |
| View | 모델에서 전달받은 데이터를 화면이나 문서 형태로 표현한다. | `list.jsp`가 `prods`를 꺼내 상품 목록 HTML을 만든다. |
| Controller | 요청을 해석하고 필요한 업무 기능을 호출한 뒤, 결과를 어떤 응답에 사용할지 결정한다. | `MainServlet`의 `list()`가 상품 서비스를 호출하고 목록을 `prods`라는 요청 속성에 담아 `list.jsp`로 전달한다. |

상품 목록을 보여 주는 코드를 바꿀 때 열 이름과 배치의 변경은 JSP에서, 어떤 상품을 조회할지에 관한 공통 업무 규칙은 서비스에서 다룬다. 어떤 요청이 상품 조회를 시작하고 어떤 화면을 띄울지는 컨트롤러의 책임이다. 이렇게 나누면 화면이나 HTTP 경로가 바뀌어도 조회 기능을 다시 사용할 수 있다.

## 상품 목록 데이터는 어떻게 화면까지 갈까?

로컬 `C:\SSAFY\workspace\05back\3logindao`의 `MainServlet.java`, `ProductServiceImpl.java`, `list.jsp`에서 아래 흐름을 확인할 수 있다. 서블릿과 HTTP 메서드가 선택되는 과정은 [[01 · MainServlet과 DispatcherServlet|MainServlet과 DispatcherServlet]]에서 자세히 다룬다.

1. `MainServlet.list()`가 `ps.selectAll()`을 호출한다. 상품 서비스는 DAO에 조회를 위임해 `List<Product>`를 얻는다.
2. 컨트롤러가 `request.setAttribute("prods", prods)`로 목록을 현재 요청에 담고 `/list.jsp`로 포워드한다. 서버 내부에서 같은 요청을 다음 처리 대상으로 전달하는 방식이다. [Jakarta Servlet 규격의 포워딩](https://jakarta.ee/specifications/servlet/6.1/jakarta-servlet-spec-6.1.html#the-forward-method)
3. JSP가 `request.getAttribute("prods")`로 목록을 읽고 상품별 HTML을 만든다.

여기서 `prods`는 뷰에 전달하는 **모델 데이터**다. Spring MVC에서 컨트롤러가 받는 `org.springframework.ui.Model`은 이처럼 뷰에 넘길 이름과 값을 담는 객체다. 서비스가 맡는 업무 규칙과 데이터 조회는 MVC에서 말하는 더 넓은 모델 쪽의 책임이다. [Spring MVC 반환값과 모델](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/return-types.html)

## Spring MVC에서 HTML과 JSON은 어떻게 만들어질까?

상품 목록을 Spring MVC로 구현한다고 가정하면 컨트롤러가 상품 서비스를 호출하는 구조는 그대로 적용할 수 있다. 조회 결과로 화면을 만들어 보낼 때와 데이터만 보낼 때의 흐름은 다음과 같다. [Spring MVC 뷰 해석](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-servlet/viewresolver.html), [Spring MVC 응답 본문](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/responsebody.html)

| 응답 | 컨트롤러가 하는 일 | Spring MVC가 하는 일 |
| --- | --- | --- |
| 서버에서 만든 HTML | `@Controller` 메서드에서 상품 목록을 `Model`에 담고 `"list"` 같은 뷰 이름을 반환한다. | 뷰 이름에 연결된 JSP나 템플릿을 찾아 모델을 전달하고 HTML 응답을 만든다. |
| JSON API | `@RestController` 메서드에서 상품 목록 또는 응답용 객체를 반환한다. | JSON 응답으로 처리하는 경우 메시지 변환기가 반환 객체를 JSON으로 바꿔 HTTP 응답 본문에 쓴다. 브라우저 화면이 있다면 화면 구성은 프런트엔드가 맡는다. |

HTTP 요청이 여러 컨트롤러 중 어디로 갈지 정하는 일은 [[05 · 프론트 컨트롤러 패턴|프론트 컨트롤러 패턴]]과 이어진다. Spring MVC에서는 `DispatcherServlet`이 그 공통 진입점을 맡고, 애플리케이션이 작성한 컨트롤러가 상품 조회처럼 개별 요청에 필요한 작업을 연결한다.

## 복습 질문

- 상품 목록 표의 열을 추가하는 작업과 판매 중인 상품을 골라 조회하는 공통 규칙은 각각 어디에서 다룰까?
- `prods`는 어디에서 만들어져 어떤 이름으로 전달되며, JSP는 어디에서 꺼낼까?
- 같은 상품 조회 기능을 HTML 화면과 JSON API에 제공하면 서비스와 응답 처리의 책임은 어떻게 나뉠까?
