# Homebrew의 패키지 설치 방식 비교

> [!info] 관련 개념
> - [[01 · Homebrew Formula|Homebrew Formula]] — 공통으로 사용하는 패키지 정의
> - [[02 · Homebrew Bottle|Homebrew Bottle]] — 사전 빌드 결과물 설치
> - [[03 · 소스 빌드|소스 빌드]] — 내 Mac에서 빌드
> - [[03 · Homebrew와 Xcode Command Line Tools|Homebrew와 Xcode Command Line Tools]] — 개발 도구 요구 사항

같은 `wget`을 설치하더라도 [[02 · Homebrew Bottle|Bottle]]을 쓰면 이미 빌드된 파일을 내려받고, [[03 · 소스 빌드|소스 빌드]]를 요청하면 내 Mac에서 빌드한다. 두 경로 모두 [[01 · Homebrew Formula|wget Formula]]를 사용한다.

| 비교 항목 | Bottle 설치 | 소스 빌드 |
| --- | --- | --- |
| 명령 예시 | `brew install wget` | `brew install --build-from-source wget` |
| `wget`에 대해 받는 파일 | 호환되는 사전 빌드 패키지 | 소스 코드 |
| 빌드가 일어나는 곳 | 패키지 배포 전에 Homebrew의 빌드 환경에서 | 설치 중에 내 Mac에서 |
| 내 Mac에서 필요한 도구 | `wget`을 컴파일하는 단계가 생략된다. 플랫폼별 요구 사항은 [[03 · Homebrew와 Xcode Command Line Tools\|Homebrew와 Xcode Command Line Tools]] 참조 | CLT 또는 Xcode와 Formula에 필요한 빌드 의존성 |
| 설치 시간 | 보통 더 짧다. | 컴파일과 링킹 때문에 보통 더 오래 걸린다. |

`brew install wget`은 호환되는 Bottle을 사용할 수 있을 때 자동으로 선택한다. 호환되는 Bottle을 사용할 수 없는 환경에서는 소스 빌드가 시도될 수 있다. `--build-from-source`를 명시하면 `wget` 자체의 소스 빌드를 요청하며, 의존성은 Bottle을 사용할 수 있다. [Homebrew Bottle 문서](https://docs.brew.sh/Bottles) · [Homebrew 명령 설명서](https://docs.brew.sh/Manpage)

Homebrew 없이도 배포자가 제공하는 바이너리를 설치하거나 프로젝트의 빌드 절차를 따라 소스에서 빌드할 수 있다. [[01 · Homebrew|Homebrew]]를 사용하면 다운로드, 의존성 설치, 업데이트와 삭제를 같은 도구로 관리할 수 있다.

[[02 · 맥북 개발환경 시작|맥북 개발환경 시작]]
