# Homebrew와 Xcode Command Line Tools

> [!info] 관련 개념
> - [[01 · Homebrew|Homebrew]] — 설치를 처리하는 패키지 관리자
> - [[02 · Xcode Command Line Tools|Xcode Command Line Tools]] — 빌드 도구와 SDK
> - [[02 · Homebrew Bottle|Homebrew Bottle]] — 사전 빌드 패키지 설치
> - [[03 · 소스 빌드|소스 빌드]] — 내 Mac에서 빌드

[[01 · Homebrew|Homebrew]]는 패키지 설치를 처리하고, [[02 · Xcode Command Line Tools|Xcode Command Line Tools]](CLT)는 macOS용 프로그램을 빌드할 때 필요한 개발 도구를 제공한다. Homebrew는 필요할 때 이 도구를 사용한다. CLT 업데이트는 Apple의 소프트웨어 업데이트에서 관리하고, Homebrew로 설치한 `wget` 같은 패키지는 `brew upgrade`로 관리한다. [Apple Command Line Tools 문서](https://developer.apple.com/documentation/xcode/installing-the-command-line-tools/)

| Homebrew에서 하는 작업 | Apple 개발 도구 요구 사항 |
| --- | --- |
| Apple Silicon Mac에서 호환되는 [[02 · Homebrew Bottle\|Homebrew Bottle]] 설치 | Homebrew 공식 문서에 따르면 CLT 없이 설치할 수 있다. |
| Intel Mac에서 호환되는 Bottle 설치 | Homebrew 공식 문서에 따르면 Apple 개발 도구가 필요하다. |
| macOS에서 Formula [[03 · 소스 빌드\|소스 빌드]] | CLT 또는 Xcode가 필요하다. 개별 프로그램이 추가로 요구하는 빌드 의존성이 있을 수 있다. |

이 표의 Bottle 행은 호환되는 패키지를 설치하는 경우를 다룬다. Homebrew가 안내하는 지원 macOS와 기기 조건은 설치 문서에서 확인할 수 있다. [Homebrew 설치 문서](https://docs.brew.sh/Installation.html) · [Homebrew 지원 등급](https://docs.brew.sh/Support-Tiers)

## Git, Clang, Make를 따로 설치하면 충분할까?

Git은 코드를 내려받고 이력을 관리하는 데 쓰이고, Clang과 Make는 빌드 과정에 쓰인다. macOS용 프로그램을 빌드하려면 프로젝트에 따라 링커, macOS SDK, 외부 라이브러리, 적절한 도구 경로도 필요하다. 세 명령이 실행되더라도 Homebrew의 macOS 소스 빌드 요구 사항을 충족했는지는 별도로 확인해야 한다.
