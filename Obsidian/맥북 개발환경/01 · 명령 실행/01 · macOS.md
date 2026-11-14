# macOS

> [!info] 관련 개념
> - [[02 · Terminal.app|Terminal.app]] — 운영체제에서 실행하는 터미널 앱
> - [[03 · 셸과 zsh|셸과 zsh]] — 명령을 해석하는 프로그램
> - [[01 · Homebrew|Homebrew]] — 추가 개발 도구 설치
> - [[02 · Xcode Command Line Tools|Xcode Command Line Tools]] — Apple의 빌드 도구

macOS는 Mac의 운영체제다. 파일 시스템과 네트워크를 관리하고, 프로그램을 프로세스로 실행할 환경을 제공한다. 터미널에서 작업할 때도 이 기반 위에서 여러 프로그램이 각자 역할을 수행한다.

| 구성 요소 | 역할 |
| --- | --- |
| [[02 · Terminal.app\|Terminal.app]] | 텍스트를 입력하고 출력을 보는 Apple의 앱 |
| [[03 · 셸과 zsh\|셸과 zsh]] | 명령을 해석하는 셸. zsh는 macOS에 포함된 오픈소스 프로그램이다. |
| [[01 · Homebrew\|Homebrew]] | 셸에서 `brew`로 호출하는 패키지 관리 프로그램 |
| [[02 · Xcode Command Line Tools\|Xcode Command Line Tools]] | Apple이 배포하는 명령줄 개발 도구와 macOS SDK |

## 네 개념이 함께 동작하는 방식

**macOS가 실행 환경을 제공하고, 터미널이 입출력을 담당하며, 셸이 명령을 해석하고, brew가 패키지 관리 작업을 수행한다.** Terminal.app, zsh, brew는 모두 macOS에서 실행되는 프로그램이다.

```text
macOS가 제공하는 실행 환경
  Terminal.app  ── 입력 전달 ──>  zsh  ── 프로그램 실행 ──>  brew
  Terminal.app  <── 실행 결과 표시 ──────────────────────────┘
```

이 그림의 화살표는 명령 처리 흐름을 간략하게 나타낸다. 실제 입력과 출력은 운영체제가 제공하는 터미널 장치와 입출력 기능을 통해 전달된다.

Homebrew가 설치되어 있는 Mac에서 `brew install wget`을 입력했다고 생각해 보자.

1. **Terminal.app**이 키보드 입력을 받고, Enter를 누르면 셸이 명령을 읽을 수 있게 전달한다.
2. **zsh**가 `brew`를 실행할 명령으로 해석하고, [[04 · PATH|PATH]]에서 실행 파일을 찾는다.
3. **macOS**의 프로세스 실행 기능을 통해 `brew`가 실행된다. zsh는 `install`과 `wget`을 인자로 전달한다.
4. **Homebrew**가 인자를 해석해 wget의 패키지 정보와 의존성을 확인하고 설치한다. 다운로드와 파일 저장에는 macOS의 네트워크·파일 시스템 기능을 사용한다.
5. **Terminal.app**에 진행 상황과 결과가 표시된다. 명령이 종료되면 셸이 다음 입력을 받을 프롬프트를 표시한다.

## Windows 환경과 비교

| 역할 | macOS 환경의 예 | Windows 환경의 예 |
| --- | --- | --- |
| 프로그램을 실행하는 운영체제 | macOS | Windows |
| 명령 입력과 결과를 보여주는 터미널 앱 | Terminal.app | Windows Terminal |
| 명령을 해석하고 프로그램을 실행하는 셸 | zsh | PowerShell |
| 프로그램 설치·관리를 수행하는 도구 | Homebrew의 `brew` | Windows Package Manager의 `winget` |

Windows Terminal의 PowerShell 탭에서 `winget search Git`을 실행하면, PowerShell이 winget을 실행하고 winget이 패키지를 검색한다. Mac의 터미널에서 `brew search git`을 실행하는 흐름과 대응한다. 이 비교는 각 구성 요소의 역할을 기준으로 하며, 명령 문법과 설치 방식은 도구마다 다르다. [Microsoft Windows Terminal 설명](https://learn.microsoft.com/en-us/windows/terminal/) · [Microsoft WinGet 설명](https://learn.microsoft.com/en-us/windows/package-manager/winget/)

Windows Terminal에서는 PowerShell과 명령 프롬프트를 각각 탭으로 열 수 있다. 같은 터미널 앱에서도 어떤 셸을 실행하느냐에 따라 명령 해석 방식이 달라진다. Mac에서도 Terminal.app 설정에서 시작할 셸을 선택할 수 있다. [Apple의 기본 셸 변경 안내](https://support.apple.com/guide/terminal/change-the-default-shell-trml113/mac)

macOS와 Linux에서는 `ls`, `grep`, `ssh`처럼 공통으로 쓰이는 도구가 많다. 운영체제별 구현이나 도구 버전이 다르면 같은 이름의 명령도 옵션에 차이가 날 수 있다.

[Apple 터미널 사용 설명서](https://support.apple.com/guide/terminal/welcome/mac)
