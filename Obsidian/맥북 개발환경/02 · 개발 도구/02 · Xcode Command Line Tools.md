# Xcode Command Line Tools

> [!info] 관련 개념
> - [[01 · macOS|macOS]] — 개발 대상 운영체제
> - [[03 · 소스 빌드|소스 빌드]] — 컴파일러와 SDK의 사용 과정
> - [[03 · Homebrew와 Xcode Command Line Tools|Homebrew와 Xcode Command Line Tools]] — 패키지 관리자와의 관계

Xcode Command Line Tools(CLT)는 Apple이 별도로 설치할 수 있게 배포하는 명령줄 개발 도구 모음이다. [[01 · macOS|macOS]]에서 프로그램을 빌드할 때 필요한 도구 체인과 macOS SDK를 제공한다. 전체 Xcode를 설치한 경우에는 Xcode에 포함된 도구를 사용할 수 있다. [Apple Command Line Tools 문서](https://developer.apple.com/documentation/xcode/installing-the-command-line-tools/)

| 구성 요소 | 빌드에서 맡는 역할 |
| --- | --- |
| Clang | C 등으로 작성된 소스를 컴파일한다. |
| 링커 | 컴파일한 코드와 필요한 라이브러리를 연결해 실행 파일 등을 만든다. |
| `make` | Makefile에 정의된 빌드 단계와 의존 관계를 처리한다. |
| macOS SDK | macOS 기능을 사용하는 코드의 빌드에 필요한 헤더와 라이브러리 인터페이스 등을 제공한다. |

`xcode-select --install`은 별도 CLT 패키지 설치를 요청한다. `xcode-select -p`는 현재 선택된 개발 도구 디렉터리를 보여준다. CLT 패키지만 설치한 환경에서는 보통 `/Library/Developer/CommandLineTools`가 표시된다. CLT 업데이트는 macOS 소프트웨어 업데이트에서 관리한다. [Apple Command Line Tools 문서](https://developer.apple.com/documentation/xcode/installing-the-command-line-tools/)

[[03 · 소스 빌드|소스 빌드]] · [[03 · Homebrew와 Xcode Command Line Tools|Homebrew와 Xcode Command Line Tools]]
