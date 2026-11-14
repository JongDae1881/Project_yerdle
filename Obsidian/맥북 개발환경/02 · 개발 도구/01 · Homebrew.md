# Homebrew

> [!info] 관련 개념
> - [[02 · Terminal.app|Terminal.app]] — 명령을 입력하는 앱
> - [[03 · 셸과 zsh|셸과 zsh]] — brew 실행
> - [[01 · Homebrew Formula|Homebrew Formula]] — 패키지 정의
> - [[03 · Homebrew와 Xcode Command Line Tools|Homebrew와 Xcode Command Line Tools]] — 빌드 도구가 필요한 조건

Homebrew는 macOS와 Linux에서 사용할 수 있는 독립 오픈소스 패키지 관리자다. [[02 · Terminal.app|Terminal.app]]을 열고 [[03 · 셸과 zsh|셸]]에서 `brew` 명령으로 사용한다. Homebrew가 관리하는 패키지의 다운로드, 의존성 설치, 업데이트, 삭제를 처리한다. [Homebrew 공식 홈페이지](https://brew.sh/)

| 작업 | 명령 예시 |
| --- | --- |
| 설치 | `brew install wget` |
| 설치된 Formula 목록 확인 | `brew list --formula` |
| 설치된 패키지 업데이트 | `brew upgrade wget` |
| 제거 | `brew uninstall wget` |

Debian 계열의 `apt`도 패키지를 설치하고 업데이트하는 도구다. `apt`는 Debian 계열 배포판의 시스템 패키지 관리 체계에 통합되어 있고, macOS에서는 Homebrew를 추가로 설치해 사용한다.

`wget` 같은 패키지의 정의는 [[01 · Homebrew Formula|Homebrew Formula]]에서 다룬다. Apple의 개발 도구와 관리 범위가 어떻게 맞물리는지는 [[03 · Homebrew와 Xcode Command Line Tools|Homebrew와 Xcode Command Line Tools]]에서 볼 수 있다.

[Homebrew 명령 설명서](https://docs.brew.sh/Manpage) · [Debian 패키지 관리 도구](https://www.debian.org/doc/manuals/debian-faq/pkgtools.en.html)

## brew로 설치한 다음에는 어떻게 실행할까?

```sh
brew install wget
wget --version
```

첫 번째 명령에서는 셸이 brew를 실행하고, Homebrew가 wget을 설치한다. 설치와 경로 설정이 끝난 뒤 두 번째 명령을 실행하면 셸이 wget을 찾아 직접 실행한다. wget은 자신의 버전 정보를 출력한다. Homebrew는 설치와 관리를 맡으며, 설치된 프로그램의 평소 실행은 셸에서 해당 명령을 호출해 수행한다.

[[01 · macOS#Windows 환경과 비교|macOS#Windows 환경과 비교]]에서는 Windows Terminal, PowerShell, winget의 대응 관계를 함께 볼 수 있다.
