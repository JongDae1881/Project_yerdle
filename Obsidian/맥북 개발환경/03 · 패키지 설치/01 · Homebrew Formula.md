# Homebrew Formula

> [!info] 관련 개념
> - [[01 · Homebrew|Homebrew]] — 정의를 읽고 설치를 수행
> - [[02 · Homebrew Bottle|Homebrew Bottle]] — 정의에 따라 미리 빌드한 패키지
> - [[04 · Homebrew의 패키지 설치 방식 비교|Homebrew의 패키지 설치 방식 비교]] — Formula를 사용하는 두 설치 경로

Formula는 [[01 · Homebrew|Homebrew]]가 패키지를 어떻게 가져오고 빌드하고 설치하는지 정의한 Ruby 코드다. 소스 URL과 체크섬, 의존성, 빌드·설치 절차, 설치 후 테스트 등을 담을 수 있다. [Homebrew Formula Cookbook](https://docs.brew.sh/Formula-Cookbook)

예를 들어 `brew install wget`을 실행하면 Homebrew는 `wget` Formula를 참조한다. 함께 설치할 의존성이 있는지, 현재 환경에서 쓸 수 있는 [[02 · Homebrew Bottle|Bottle]]이 있는지 등을 확인한다. 소스에서 빌드할 때는 Formula에 정의된 빌드·설치 절차를 따른다.

`brew info wget`으로 Formula 정보를 확인할 수 있다. `wget`의 실제 정의는 Homebrew의 패키지 페이지에서 연결된 Formula 소스로 확인할 수 있다. [Homebrew의 wget 패키지 페이지](https://formulae.brew.sh/formula/wget)

[[04 · Homebrew의 패키지 설치 방식 비교|Homebrew의 패키지 설치 방식 비교]]
