# Terminal.app

> [!info] 관련 개념
> - [[01 · macOS|macOS]] — 앱이 실행되는 운영체제
> - [[03 · 셸과 zsh|셸과 zsh]] — 입력을 받아 명령을 실행

Terminal.app은 [[01 · macOS|macOS]]에서 명령줄 프로그램과 텍스트로 상호작용하는 앱이다. 보통 새 창이나 탭을 열면 셸이 시작된다. macOS의 기본 셸은 zsh이며, 새 창에서 시작할 셸은 터미널 설정에서 바꿀 수 있다. [Apple의 기본 셸 변경 안내](https://support.apple.com/guide/terminal/change-the-default-shell-trml113/mac)

`brew install wget`을 입력하고 Enter를 누르면 터미널이 입력을 셸 세션에 전달한다. [[03 · 셸과 zsh|셸]]은 명령을 해석해 `brew`를 실행하고, 터미널은 화면에 설치 진행 상황과 결과를 표시한다.

터미널 창에서 `pwd`를 실행하면 셸을 통해 현재 작업 디렉터리를 확인할 수 있다. 새 탭의 시작 위치는 터미널 설정과 세션에 따라 달라질 수 있다.

Windows Terminal에서 PowerShell 탭을 여는 모습을 떠올리면 관계를 이해하기 쉽다. Windows Terminal은 입력과 출력을 보여주는 앱이고, 그 탭 안에서 PowerShell이 명령을 해석한다. Mac에서는 Terminal.app과 zsh가 각각 이 역할을 맡는다. [Microsoft Windows Terminal 설명](https://learn.microsoft.com/en-us/windows/terminal/)

[[01 · macOS#네 개념이 함께 동작하는 방식|macOS#네 개념이 함께 동작하는 방식]]에서 운영체제부터 brew 실행까지의 흐름을 볼 수 있다.
