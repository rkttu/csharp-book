# 부록 B. .NET CLI 명령어

.NET CLI(Command Line Interface)는 .NET 애플리케이션을 개발, 빌드, 실행 및 배포하는 데 필요한 크로스 플랫폼 도구입니다. 명령줄에서 간단한 명령어로 프로젝트의 전체 생명 주기를 관리할 수 있으며, IDE 없이도 효율적인 개발이 가능합니다. 이 부록에서는 .NET 개발에서 가장 자주 사용되는 CLI 명령어들을 카테고리별로 정리하여 제공합니다.

---

## B.1 프로젝트 관리

.NET CLI를 사용하면 프로젝트의 생성, 구조 확인, 솔루션 관리 등을 명령줄에서 빠르게 수행할 수 있습니다.

### 프로젝트 생성

새로운 .NET 프로젝트를 생성하는 기본 명령어입니다.

```bash
# 콘솔 애플리케이션 생성
dotnet new console -n MyApp

# 클래스 라이브러리 생성
dotnet new classlib -n MyLibrary

# 웹 API 프로젝트 생성
dotnet new webapi -n MyApi

# ASP.NET Core MVC 프로젝트 생성
dotnet new mvc -n MyWebApp
```

**주요 옵션:**
- `-n` 또는 `--name`: 프로젝트 이름 지정
- `-o` 또는 `--output`: 프로젝트를 생성할 디렉터리 경로 지정
- `-f` 또는 `--framework`: 대상 프레임워크 지정 (예: net8.0, net10.0)

### 사용 가능한 템플릿 확인

```bash
# 설치된 모든 프로젝트 템플릿 목록 보기
dotnet new list

# 특정 템플릿 검색
dotnet new list web
```

### 솔루션 관리

여러 프로젝트를 하나의 솔루션으로 묶어 관리할 수 있습니다.

```bash
# 새 솔루션 파일 생성
dotnet new sln -n MySolution

# 솔루션에 프로젝트 추가
dotnet sln add MyApp/MyApp.csproj

# 솔루션에서 프로젝트 제거
dotnet sln remove MyApp/MyApp.csproj

# 솔루션의 모든 프로젝트 나열
dotnet sln list
```

### 프로젝트 참조 관리

프로젝트 간 의존성을 관리합니다.

```bash
# 다른 프로젝트 참조 추가
dotnet add reference ../MyLibrary/MyLibrary.csproj

# 프로젝트 참조 목록 확인
dotnet list reference

# 프로젝트 참조 제거
dotnet remove reference ../MyLibrary/MyLibrary.csproj
```

---

## B.2 빌드와 실행

프로젝트를 컴파일하고 실행하는 명령어들입니다.

### 빌드

프로젝트를 컴파일하여 실행 파일을 생성합니다.

```bash
# 현재 프로젝트 빌드
dotnet build

# 특정 구성으로 빌드 (Debug 또는 Release)
dotnet build --configuration Release
dotnet build -c Release

# 빌드 출력 디렉터리 지정
dotnet build -o ./output
```

**주요 옵션:**
- `-c` 또는 `--configuration`: 빌드 구성 (Debug, Release)
- `-o` 또는 `--output`: 출력 디렉터리 지정
- `--no-restore`: 빌드 전 패키지 복원 생략
- `-v` 또는 `--verbosity`: 출력 수준 지정 (quiet, minimal, normal, detailed, diagnostic)

### 실행

애플리케이션을 빌드하고 실행합니다.

```bash
# 프로젝트 실행 (자동으로 빌드 포함)
dotnet run

# 명령줄 인수와 함께 실행
dotnet run -- arg1 arg2

# 특정 프로젝트 파일 지정하여 실행
dotnet run --project ./MyApp/MyApp.csproj
```

**주요 옵션:**
- `--project`: 실행할 프로젝트 파일 경로
- `--no-build`: 빌드 없이 이전 빌드 결과 실행
- `-c` 또는 `--configuration`: 실행할 구성 지정

### 정리

빌드 산출물을 삭제합니다.

```bash
# 빌드 출력 파일 정리
dotnet clean

# 특정 구성의 출력만 정리
dotnet clean --configuration Release
```

### 게시

애플리케이션을 배포 가능한 형태로 출력합니다.

```bash
# 기본 게시 (프레임워크 의존)
dotnet publish

# Release 구성으로 게시
dotnet publish -c Release

# 특정 런타임용 자체 포함 게시
dotnet publish -c Release -r win-x64 --self-contained

# 단일 파일로 게시
dotnet publish -c Release -r win-x64 --self-contained -p:PublishSingleFile=true
```

**주요 옵션:**
- `-r` 또는 `--runtime`: 대상 런타임 식별자 (win-x64, linux-x64, osx-arm64 등)
- `--self-contained`: 자체 포함 배포 (true) 또는 프레임워크 의존 배포 (false)
- `-o` 또는 `--output`: 게시 출력 디렉터리

---

## B.3 패키지 관리

NuGet 패키지를 관리하는 명령어들입니다.

### 패키지 추가

프로젝트에 NuGet 패키지를 추가합니다.

```bash
# 패키지 추가 (최신 버전)
dotnet add package Newtonsoft.Json

# 특정 버전 지정
dotnet add package Newtonsoft.Json --version 13.0.3

# 프리릴리스 버전 포함하여 최신 버전 추가
dotnet add package Newtonsoft.Json --prerelease
```

**주요 옵션:**
- `--version`: 특정 버전 지정
- `--prerelease`: 프리릴리스 버전 허용
- `--source`: 패키지 소스 URL 지정

### 패키지 제거

```bash
# 프로젝트에서 패키지 제거
dotnet remove package Newtonsoft.Json
```

### 패키지 목록 확인

```bash
# 프로젝트의 모든 패키지 종속성 나열
dotnet list package

# 오래된 패키지 확인
dotnet list package --outdated

# 업데이트 가능한 패키지의 최신 버전 포함하여 표시
dotnet list package --outdated --include-prerelease
```

### 패키지 복원

프로젝트 파일에 정의된 모든 패키지를 다운로드합니다.

```bash
# 패키지 복원
dotnet restore

# 특정 소스에서만 복원
dotnet restore --source https://api.nuget.org/v3/index.json
```

**참고:** `dotnet build`, `dotnet run` 명령은 자동으로 패키지를 복원하므로, 명시적으로 `dotnet restore`를 실행할 필요는 없습니다. 단, CI/CD 파이프라인이나 캐시 최적화를 위해 별도로 실행하는 경우가 있습니다.

### 패키지 업데이트

```bash
# 특정 패키지를 최신 버전으로 업데이트
dotnet add package Newtonsoft.Json

# 모든 패키지 업데이트 (수동으로 각 패키지에 대해 실행 필요)
# .NET CLI는 모든 패키지를 한 번에 업데이트하는 명령어를 제공하지 않음
```

---

## B.4 테스트 실행

단위 테스트 및 통합 테스트를 실행하는 명령어들입니다.

### 테스트 프로젝트 생성

```bash
# xUnit 테스트 프로젝트 생성
dotnet new xunit -n MyApp.Tests

# NUnit 테스트 프로젝트 생성
dotnet new nunit -n MyApp.Tests

# MSTest 테스트 프로젝트 생성
dotnet new mstest -n MyApp.Tests
```

### 테스트 실행

```bash
# 모든 테스트 실행
dotnet test

# 빌드 없이 테스트 실행
dotnet test --no-build

# 상세한 출력으로 테스트 실행
dotnet test --verbosity detailed

# 특정 구성으로 테스트 실행
dotnet test --configuration Release
```

**주요 옵션:**
- `--no-build`: 테스트 실행 전 빌드 생략
- `--no-restore`: 패키지 복원 생략
- `-c` 또는 `--configuration`: 테스트할 구성 지정
- `--logger`: 테스트 결과 로거 지정 (예: `--logger "console;verbosity=detailed"`)
- `--filter`: 특정 테스트만 실행 (예: `--filter "FullyQualifiedName~MyNamespace"`)

### 테스트 필터링

특정 테스트만 실행하려면 필터를 사용합니다.

```bash
# 특정 네임스페이스의 테스트만 실행
dotnet test --filter "FullyQualifiedName~MyApp.Tests.UnitTests"

# 특정 테스트 메서드만 실행
dotnet test --filter "Name=CalculateSum_ShouldReturnCorrectValue"

# 카테고리별 테스트 실행
dotnet test --filter "Category=Integration"
```

### 코드 커버리지

테스트 코드 커버리지를 측정합니다.

```bash
# 코드 커버리지 수집
dotnet test --collect:"XPlat Code Coverage"

# 특정 형식으로 커버리지 리포트 생성 (coverlet 사용)
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover
```

**참고:** 코드 커버리지를 사용하려면 `coverlet.collector` 패키지가 테스트 프로젝트에 설치되어 있어야 합니다.

### 테스트 결과 출력

```bash
# 테스트 결과를 파일로 저장
dotnet test --logger "trx;LogFileName=test-results.trx"

# 여러 로거 동시 사용
dotnet test --logger "console;verbosity=detailed" --logger "trx"
```

---

## 추가 유용한 명령어

### 버전 확인

```bash
# .NET SDK 버전 확인
dotnet --version

# 설치된 모든 SDK와 런타임 확인
dotnet --info

# 설치된 SDK 목록만 확인
dotnet --list-sdks

# 설치된 런타임 목록만 확인
dotnet --list-runtimes
```

### 도움말

```bash
# 전체 명령어 도움말
dotnet --help

# 특정 명령어의 도움말
dotnet new --help
dotnet build --help
dotnet test --help
```

### 대화형 모드

```bash
# .NET Interactive (C# REPL) 시작
dotnet repl
```

**참고:** 대화형 모드를 사용하려면 별도로 `dotnet tool install -g Microsoft.dotnet-interactive` 명령으로 도구를 설치해야 합니다.

---

## 마치며

.NET CLI는 강력하고 유연한 도구로, IDE 없이도 완전한 .NET 개발이 가능하게 합니다. 이 부록에서 다룬 명령어들은 일상적인 개발 작업에서 가장 자주 사용되는 것들입니다. 

더 자세한 정보와 고급 옵션은 다음 공식 문서를 참조하세요:
- [.NET CLI 공식 문서](https://docs.microsoft.com/ko-kr/dotnet/core/tools/)
- [dotnet 명령어 참조](https://docs.microsoft.com/ko-kr/dotnet/core/tools/dotnet)

명령줄 환경에 익숙해지면 개발 생산성이 크게 향상되며, 자동화 스크립트 작성이나 CI/CD 파이프라인 구성 시에도 매우 유용합니다.
