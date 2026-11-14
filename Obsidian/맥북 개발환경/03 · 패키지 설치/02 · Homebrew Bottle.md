# Homebrew Bottle

> [!info] 관련 개념
> - [[01 · Homebrew Formula|Homebrew Formula]] — 패키지 빌드와 설치의 정의
> - [[03 · 소스 빌드|소스 빌드]] — 소스에서 결과물을 만드는 과정
> - [[04 · Homebrew의 패키지 설치 방식 비교|Homebrew의 패키지 설치 방식 비교]] — 다운로드와 빌드 위치 비교

Bottle은 [[01 · Homebrew Formula|Formula]]를 바탕으로 사전에 빌드해 압축한 Homebrew 패키지다. 내 시스템에 맞는 Bottle을 사용할 수 있으면 `brew install wget`은 그 패키지를 내려받아 설치한다. 이 경로에서는 내 Mac에서 `wget` 자체를 컴파일하는 단계가 생략된다. [Homebrew Bottle 문서](https://docs.brew.sh/Bottles)

**바이너리 배포본**은 이미 빌드된 프로그램을 배포하는 파일을 가리키는 일반 용어다. 배포자가 제공하는 Node.js 설치 파일도 바이너리 배포본의 예이며, **Bottle**은 Homebrew가 Formula의 빌드 결과를 배포할 때 쓰는 이름이다.

Bottle을 사용할 수 있는지는 운영체제와 아키텍처, 설치 경로 호환성 등에 따라 달라진다. 프로그램 실행에 필요한 라이브러리가 별도 의존성으로 함께 설치될 수 있다. [Homebrew Bottle 문서](https://docs.brew.sh/Bottles)

[[03 · 소스 빌드|소스 빌드]] · [[04 · Homebrew의 패키지 설치 방식 비교|Homebrew의 패키지 설치 방식 비교]]
