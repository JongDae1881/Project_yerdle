# PATH

> [!info] 관련 개념
> - [[03 · 셸과 zsh|셸과 zsh]] — 검색 경로를 사용하는 셸
> - [[01 · Homebrew|Homebrew]] — brew 실행 파일의 경로 설정

`PATH`는 [[03 · 셸과 zsh|셸]]이 외부 명령을 이름으로 실행할 때 검색하는 디렉터리 목록이다. macOS에서는 디렉터리를 콜론(`:`)으로 구분하며, 셸은 앞에 있는 디렉터리부터 확인한다. 실행 파일의 전체 경로를 입력하면 해당 경로로 직접 실행할 수 있다. [Zsh 매개변수 문서](https://zsh.sourceforge.io/Doc/Release/Parameters.html)

```sh
printf '%s\n' "$PATH"
command -v brew
```

첫 번째 명령은 현재 검색 경로를 출력하고, 두 번째 명령은 현재 셸에서 `brew`가 어떻게 해석되는지 확인한다. Apple Silicon에서 기본 경로에 [[01 · Homebrew|Homebrew]]를 설치했다면 일반적으로 `/opt/homebrew/bin/brew`가 표시된다. [Homebrew 설치 문서](https://docs.brew.sh/Installation.html)

설치한 뒤에도 `command not found: brew`가 나오면 실행 파일이 있는 디렉터리가 현재 셸의 `PATH`에 포함되어 있는지 확인한다. 빠져 있다면 Homebrew 설치 종료 시 표시된 `brew shellenv` 설정 안내를 해당 셸에 적용한다. [Homebrew 명령 설명서](https://docs.brew.sh/Manpage)
