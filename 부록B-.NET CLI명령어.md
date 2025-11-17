# 부록 B. .NET CLI 명령어

명령줄 인터페이스(Command Line Interface, CLI)는 소프트웨어 개발의 역사만큼이나 오래된 도구입니다. 1970년대 Unix 시스템의 쉘(Shell)부터 시작하여, 오늘날에도 개발자들은 명령줄을 통해 효율적이고 자동화된 워크플로우를 구축합니다. .NET CLI는 이러한 전통을 계승하면서도 현대적인 개발 환경에 최적화된 강력한 도구 세트를 제공합니다.

**CLI의 중요성과 가치:**

그래픽 사용자 인터페이스(GUI) 기반의 통합 개발 환경(IDE)이 보편화된 오늘날, 왜 여전히 명령줄 도구가 중요할까요? 그 이유는 다음과 같습니다:

1. **재현 가능성(Reproducibility)**: GUI 기반 작업은 "이 버튼을 클릭하고, 저 메뉴를 선택하고..."와 같이 순서를 기억해야 하지만, CLI 명령어는 정확히 같은 방식으로 반복 실행할 수 있습니다.

2. **자동화(Automation)**: 스크립트를 통해 반복적인 작업을 자동화할 수 있습니다. CI/CD 파이프라인, 빌드 스크립트, 배포 자동화 등에 필수적입니다.

3. **효율성(Efficiency)**: 숙련된 개발자에게 CLI는 GUI보다 훨씬 빠른 작업 수행을 가능하게 합니다. 마우스를 움직이고 메뉴를 찾는 대신, 명령어 한 줄로 즉시 원하는 작업을 실행할 수 있습니다.

4. **원격 작업(Remote Operations)**: 서버나 클라우드 환경에서는 GUI를 사용할 수 없는 경우가 많습니다. SSH로 접속한 환경에서 CLI는 유일한 선택지입니다.

5. **학습 깊이(Deep Learning)**: CLI를 사용하면 내부 동작 원리를 더 명확히 이해하게 됩니다. IDE가 뒤에서 실행하는 명령어를 직접 보고 제어할 수 있습니다.

**.NET CLI의 역사와 발전:**

.NET CLI는 .NET Core 1.0(2016년)과 함께 처음 도입되었습니다. 이전의 .NET Framework는 주로 Visual Studio라는 강력한 IDE에 의존했지만, .NET Core는 크로스 플랫폼을 지향하며 탄생했기 때문에, Windows뿐만 아니라 macOS와 Linux에서도 동일하게 작동하는 명령줄 도구가 필수적이었습니다.

.NET CLI는 다음과 같은 설계 원칙을 따릅니다:

- **크로스 플랫폼**: Windows, macOS, Linux에서 동일한 명령어가 동일하게 작동
- **일관성**: 모든 명령어가 `dotnet` 접두사로 시작하는 통일된 구조
- **확장성**: 글로벌 도구와 로컬 도구를 통한 기능 확장 가능
- **투명성**: IDE가 실행하는 작업을 명령줄에서 직접 확인하고 제어

이 부록에서는 .NET 개발의 전체 생명 주기를 지원하는 핵심 CLI 명령어들을 체계적으로 살펴봅니다. 프로젝트 생성부터 빌드, 실행, 테스트, 그리고 배포에 이르기까지, 각 단계에서 필요한 명령어와 그 활용 방법을 깊이 있게 다룹니다.

---

## B.1 프로젝트 관리

소프트웨어 프로젝트의 시작은 올바른 구조를 갖춘 프로젝트를 생성하는 것입니다. .NET CLI는 다양한 프로젝트 템플릿을 제공하여, 콘솔 애플리케이션부터 웹 API, 클래스 라이브러리에 이르기까지 필요한 모든 유형의 프로젝트를 손쉽게 생성할 수 있습니다. 이 섹션에서는 프로젝트의 생성과 관리에 관련된 모든 명령어를 상세히 살펴봅니다.

### 프로젝트 생성의 핵심: dotnet new

`dotnet new` 명령어는 .NET 프로젝트 생성의 시작점입니다. 이 명령어는 템플릿(template) 기반으로 작동하며, Microsoft가 제공하는 공식 템플릿뿐만 아니라 커뮤니티에서 제작한 다양한 템플릿도 설치하여 사용할 수 있습니다.

**템플릿의 개념:**

템플릿은 프로젝트의 뼈대를 제공합니다. 각 템플릿에는 해당 프로젝트 유형에 필요한 기본 파일 구조, 프로젝트 설정(`.csproj` 파일), 그리고 간단한 시작 코드가 포함되어 있습니다. 예를 들어, 콘솔 애플리케이션 템플릿은 `Program.cs` 파일과 기본 `Main` 메서드를 포함하며, 웹 API 템플릿은 컨트롤러, 라우팅 설정, Swagger 통합 등을 포함합니다.

```bash
# 콘솔 애플리케이션 생성 - 가장 기본적인 프로젝트 유형
# .NET 10에서는 top-level statements 방식으로 간결한 코드 생성
dotnet new console -n MyApp

# 클래스 라이브러리 생성 - 재사용 가능한 코드 라이브러리
# 다른 프로젝트에서 참조하여 사용할 수 있는 DLL 생성
dotnet new classlib -n MyLibrary

# 웹 API 프로젝트 생성 - RESTful API 서버 구축
# ASP.NET Core의 최소 API 스타일로 생성되며, Swagger 통합 포함
dotnet new webapi -n MyApi

# ASP.NET Core MVC 프로젝트 생성 - 전통적인 Model-View-Controller 패턴
# 서버 사이드 렌더링을 사용하는 웹 애플리케이션
dotnet new mvc -n MyWebApp

# Blazor Server 프로젝트 - C#으로 대화형 웹 UI 구축
dotnet new blazorserver -n MyBlazorApp

# Worker Service - 백그라운드 작업을 위한 호스팅 서비스
# 윈도우 서비스, Linux 데몬 등으로 실행 가능
dotnet new worker -n MyBackgroundService
```

**명령어 구조의 이해:**

`dotnet new` 명령어의 기본 구조는 다음과 같습니다:

```
dotnet new <template> [options]
```

- `<template>`: 사용할 템플릿의 짧은 이름(short name)
- `[options]`: 프로젝트 생성을 제어하는 다양한 옵션

**주요 옵션 상세 설명:**

- `-n` 또는 `--name`: 생성할 프로젝트의 이름을 지정합니다. 이 이름은 프로젝트 파일(`.csproj`)과 생성되는 디렉터리의 이름이 됩니다. 네임스페이스에도 사용되므로 C# 식별자 규칙을 따라야 합니다(공백 불가, 숫자로 시작 불가 등).

  ```bash
  # 올바른 예
  dotnet new console -n CustomerManagement
  dotnet new console -n MyCompany.Services.Email
  
  # 잘못된 예 (공백 포함)
  # dotnet new console -n "My App"  # 오류 발생
  ```

- `-o` 또는 `--output`: 프로젝트를 생성할 디렉터리 경로를 지정합니다. 지정하지 않으면 현재 디렉터리에 생성됩니다. 이 옵션을 사용하면 프로젝트 이름과 다른 경로에 프로젝트를 생성할 수 있습니다.

  ```bash
  # src 폴더 아래에 프로젝트 생성
  dotnet new console -n MyApp -o ./src/MyApp
  
  # 절대 경로로 지정
  dotnet new console -n MyApp -o /home/user/projects/MyApp
  ```

- `-f` 또는 `--framework`: 대상 .NET 버전을 명시적으로 지정합니다. 지정하지 않으면 설치된 SDK의 기본 버전이 사용됩니다.

  ```bash
  # .NET 8 Long-Term Support 버전 사용
  dotnet new console -n MyApp -f net8.0
  
  # .NET 10 사용
  dotnet new console -n MyApp -f net10.0
  
  # 여러 프레임워크 타겟팅 (멀티타겟팅)
  # 프로젝트 파일을 수동으로 편집하여 설정
  ```

**실전 활용 예시:**

```bash
# 실무 프로젝트 구조: 솔루션과 여러 프로젝트 생성
# 1. 작업 디렉터리 생성 및 이동
mkdir CustomerManagementSystem
cd CustomerManagementSystem

# 2. 솔루션 파일 생성 (다음 섹션에서 상세 설명)
dotnet new sln -n CustomerManagement

# 3. API 프로젝트 생성
dotnet new webapi -n CustomerManagement.Api -o src/CustomerManagement.Api

# 4. 비즈니스 로직 라이브러리 생성
dotnet new classlib -n CustomerManagement.Core -o src/CustomerManagement.Core

# 5. 데이터 액세스 레이어 생성
dotnet new classlib -n CustomerManagement.Data -o src/CustomerManagement.Data

# 6. 단위 테스트 프로젝트 생성
dotnet new xunit -n CustomerManagement.Tests -o tests/CustomerManagement.Tests
```

### 템플릿 탐색과 관리

.NET은 수십 가지의 기본 템플릿을 제공하며, NuGet을 통해 추가 템플릿을 설치할 수도 있습니다. 사용 가능한 템플릿을 확인하고 관리하는 것은 효율적인 개발의 시작점입니다.

```bash
# 설치된 모든 프로젝트 템플릿 목록 보기
# 템플릿 이름, 짧은 이름, 언어, 태그 등이 표시됩니다
dotnet new list

# 또는 더 간단하게 (동일한 결과)
dotnet new --list

# 특정 키워드로 템플릿 검색
# 예: 웹 관련 템플릿만 표시
dotnet new list web

# 예: 테스트 관련 템플릿 검색
dotnet new list test
```

**템플릿 출력 예시 이해하기:**

`dotnet new list` 명령어는 다음과 같은 형식으로 정보를 표시합니다:

```
템플릿 이름                              짧은 이름        언어     태그
---------------------------------------- --------------- -------- -----------------------
Console Application                      console         [C#]     Common/Console
Class Library                            classlib        [C#]     Common/Library
ASP.NET Core Web API                     webapi          [C#]     Web/WebAPI
xUnit Test Project                       xunit           [C#]     Test/xUnit
```

- **템플릿 이름**: 템플릿의 전체 설명 이름
- **짧은 이름**: `dotnet new` 명령어에서 사용하는 식별자
- **언어**: 지원하는 프로그래밍 언어(C#, F#, VB 등)
- **태그**: 템플릿의 카테고리나 특성

**커스텀 템플릿 설치:**

NuGet을 통해 커뮤니티가 만든 템플릿이나 회사 내부 표준 템플릿을 설치할 수 있습니다.

```bash
# NuGet에서 템플릿 패키지 설치
# 예: Amazon Lambda 템플릿
dotnet new install Amazon.Lambda.Templates

# 특정 버전의 템플릿 설치
dotnet new install Amazon.Lambda.Templates::6.0.0

# 설치된 템플릿 패키지 제거
dotnet new uninstall Amazon.Lambda.Templates

# 모든 설치된 템플릿 패키지 확인
dotnet new uninstall
```

**실무 팁: 조직 표준 템플릿 활용**

많은 조직에서는 프로젝트 구조, 코딩 표준, 필수 패키지 등이 미리 구성된 조직 전용 템플릿을 만들어 사용합니다. 이를 통해:

1. **일관성**: 모든 프로젝트가 동일한 구조와 표준을 따릅니다
2. **생산성**: 반복적인 설정 작업을 제거합니다
3. **품질**: 검증된 설정과 best practices가 자동으로 적용됩니다

```bash
# 조직의 내부 NuGet 서버에서 템플릿 설치
dotnet new install MyCompany.Templates --nuget-source https://nuget.mycompany.com
```

### 솔루션 관리: 대규모 프로젝트의 구조화

실무 소프트웨어 개발에서는 하나의 애플리케이션이 여러 개의 프로젝트로 구성되는 것이 일반적입니다. 예를 들어, 웹 API, 비즈니스 로직 라이브러리, 데이터 액세스 레이어, 단위 테스트 프로젝트 등이 각각 독립적인 프로젝트로 존재합니다. 이러한 여러 프로젝트를 하나의 논리적 단위로 관리하기 위해 **솔루션(Solution)** 개념이 사용됩니다.

**솔루션 파일(.sln)의 역할:**

솔루션 파일은 다음과 같은 중요한 역할을 수행합니다:

1. **프로젝트 그룹화**: 관련된 여러 프로젝트를 하나의 단위로 묶습니다
2. **빌드 순서 관리**: 프로젝트 간 의존성을 파악하여 올바른 순서로 빌드합니다
3. **IDE 통합**: Visual Studio, VS Code, Rider 등의 IDE에서 전체 프로젝트 구조를 인식합니다
4. **CI/CD 통합**: 빌드 파이프라인에서 단일 명령으로 전체 솔루션을 빌드할 수 있습니다

```bash
# 새 솔루션 파일 생성
# 현재 디렉터리에 지정한 이름의 .sln 파일이 생성됩니다
dotnet new sln -n MySolution

# 솔루션에 단일 프로젝트 추가
dotnet sln add MyApp/MyApp.csproj

# 솔루션에 여러 프로젝트 한 번에 추가 (와일드카드 사용)
# src 폴더의 모든 csproj 파일을 솔루션에 추가
dotnet sln add src/**/*.csproj

# 특정 패턴의 프로젝트만 추가
dotnet sln add **/*Tests.csproj

# 솔루션에서 프로젝트 제거
dotnet sln remove MyApp/MyApp.csproj

# 솔루션의 모든 프로젝트 나열
# 프로젝트 경로가 상대 경로로 표시됩니다
dotnet sln list
```

**솔루션 기반 빌드의 이점:**

솔루션을 사용하면 다음과 같은 이점이 있습니다:

1. **일괄 빌드**: `dotnet build MySolution.sln` 명령 하나로 모든 프로젝트를 올바른 순서로 빌드
2. **의존성 관리**: 프로젝트 간 참조 관계를 자동으로 파악하여 변경된 프로젝트만 재빌드
3. **병렬 빌드**: 의존성이 없는 프로젝트들을 동시에 빌드하여 시간 절약

**실전 예제: 마이크로서비스 아키텍처 솔루션 구성**

```bash
# 1. 루트 솔루션 생성
dotnet new sln -n ECommercePlatform

# 2. API 게이트웨이 프로젝트
dotnet new webapi -n Gateway -o services/Gateway
dotnet sln add services/Gateway/Gateway.csproj

# 3. 각 마이크로서비스
dotnet new webapi -n ProductService -o services/ProductService
dotnet new webapi -n OrderService -o services/OrderService
dotnet new webapi -n PaymentService -o services/PaymentService

# 4. 공유 라이브러리
dotnet new classlib -n Common.Models -o shared/Common.Models
dotnet new classlib -n Common.Infrastructure -o shared/Common.Infrastructure

# 5. 모든 프로젝트를 솔루션에 추가
dotnet sln add services/**/*.csproj
dotnet sln add shared/**/*.csproj

# 6. 전체 구조 확인
dotnet sln list
```

### 프로젝트 참조 관리: 모듈화된 아키텍처 구축

현대적인 소프트웨어 개발에서는 단일 거대한 프로젝트보다, 관심사의 분리(Separation of Concerns) 원칙에 따라 여러 작은 프로젝트로 나누는 것이 권장됩니다. 프로젝트 참조(Project Reference)는 이러한 프로젝트 간의 의존 관계를 정의하는 핵심 메커니즘입니다.

**프로젝트 참조의 작동 원리:**

프로젝트 A가 프로젝트 B를 참조하면:
1. A를 빌드할 때 B가 먼저 자동으로 빌드됩니다
2. A는 B의 모든 public 타입과 멤버에 접근할 수 있습니다
3. A를 실행할 때 B의 DLL이 자동으로 출력 디렉터리에 복사됩니다

```bash
# 다른 프로젝트 참조 추가
# 상대 경로로 프로젝트 파일(.csproj) 지정
dotnet add reference ../MyLibrary/MyLibrary.csproj

# 여러 프로젝트를 한 번에 참조
dotnet add reference ../Core/Core.csproj ../Data/Data.csproj

# 프로젝트 참조 목록 확인
# 현재 프로젝트가 의존하는 모든 프로젝트 표시
dotnet list reference

# 프로젝트 참조 제거
dotnet remove reference ../MyLibrary/MyLibrary.csproj
```

**계층형 아키텍처 예제:**

전형적인 3-tier 아키텍처에서의 프로젝트 참조 구조를 살펴보겠습니다:

```bash
# 1. Presentation Layer (API)는 Business Layer를 참조
cd src/MyApp.Api
dotnet add reference ../MyApp.Business/MyApp.Business.csproj

# 2. Business Layer는 Data Layer를 참조
cd ../MyApp.Business
dotnet add reference ../MyApp.Data/MyApp.Data.csproj

# 3. 참조 구조 확인
# MyApp.Api → MyApp.Business → MyApp.Data
cd ../MyApp.Api
dotnet list reference
# 출력: ../MyApp.Business/MyApp.Business.csproj

cd ../MyApp.Business
dotnet list reference
# 출력: ../MyApp.Data/MyApp.Data.csproj
```

**순환 참조 방지:**

프로젝트 참조에서 가장 주의해야 할 점은 **순환 참조(Circular Reference)**입니다. 예를 들어, A가 B를 참조하고 B가 다시 A를 참조하면 컴파일 오류가 발생합니다.

```bash
# 잘못된 예: 순환 참조
cd ProjectA
dotnet add reference ../ProjectB/ProjectB.csproj

cd ../ProjectB
dotnet add reference ../ProjectA/ProjectA.csproj
# 오류: 순환 프로젝트 종속성이 검색되었습니다
```

이러한 경우 공통 코드를 별도의 라이브러리로 분리하거나, 인터페이스를 사용하여 의존성 역전 원칙(Dependency Inversion Principle)을 적용해야 합니다.

**실무 팁: 프로젝트 구조 설계 원칙**

1. **의존성 방향 일관성**: 항상 상위 레이어가 하위 레이어를 참조하도록 구성
2. **공유 코드 분리**: 여러 프로젝트에서 사용하는 코드는 별도 라이브러리로 추출
3. **테스트 프로젝트**: 테스트 프로젝트는 테스트 대상 프로젝트만 참조
4. **API 계약 분리**: 공개 API 인터페이스를 별도 프로젝트로 분리하여 유연성 확보

---

## B.2 빌드와 실행

소프트웨어 개발의 핵심 단계는 작성한 소스 코드를 컴파일하여 실행 가능한 형태로 변환하는 것입니다. .NET CLI는 이 과정을 간소화하면서도 세밀한 제어가 가능한 다양한 명령어를 제공합니다. 이 섹션에서는 개발부터 배포까지의 전체 빌드 파이프라인을 다룹니다.

### 빌드 프로세스의 이해

**빌드란 무엇인가?**

빌드(Build)는 다음 단계들을 포함하는 복합적인 과정입니다:

1. **복원(Restore)**: NuGet 패키지 종속성을 다운로드하고 해결
2. **컴파일(Compile)**: C# 소스 코드를 IL(Intermediate Language)로 변환
3. **링크(Link)**: 모든 종속성을 결합하여 최종 어셈블리 생성
4. **복사(Copy)**: 필요한 파일들을 출력 디렉터리로 복사

**.NET의 빌드 구성(Configuration):**

.NET 프로젝트는 일반적으로 두 가지 주요 빌드 구성을 사용합니다:

- **Debug (기본값)**: 디버깅 정보 포함, 최적화 비활성화, 조건부 컴파일 심볼 DEBUG 정의
  - 장점: 디버거에서 정확한 라인별 디버깅 가능
  - 단점: 실행 속도가 느리고, 바이너리 크기가 큼
  
- **Release**: 최적화 활성화, 디버깅 정보 최소화, 프로덕션 배포용
  - 장점: 빠른 실행 속도, 작은 바이너리 크기
  - 단점: 디버깅이 어려움 (인라이닝, 최적화로 인한 코드 재배치)

```bash
# 기본 빌드 - Debug 구성으로 현재 디렉터리의 프로젝트/솔루션 빌드
dotnet build

# 특정 프로젝트 파일 또는 솔루션 파일 지정
dotnet build MyApp.csproj
dotnet build MySolution.sln

# Release 구성으로 빌드 - 프로덕션 배포용 최적화 버전
dotnet build --configuration Release
dotnet build -c Release

# 빌드 출력 디렉터리 지정
# 기본 경로(bin/Debug/net10.0) 대신 사용자 정의 경로 사용
dotnet build -o ./output
dotnet build --output ./build/artifacts

# 증분 빌드 비활성화 (전체 재빌드)
# 빌드 캐시 문제 해결이나 클린 빌드가 필요할 때
dotnet build --no-incremental

# 특정 런타임 대상으로 빌드
# 크로스 플랫폼 빌드에 유용
dotnet build -r win-x64
dotnet build -r linux-x64
dotnet build -r osx-arm64
```

**주요 옵션 상세 설명:**

- `-c` 또는 `--configuration`: 빌드 구성 지정
  - `Debug`: 개발/디버깅용 (기본값)
  - `Release`: 프로덕션 배포용
  - 사용자 정의 구성(프로젝트 파일에서 정의 가능)

- `-o` 또는 `--output`: 빌드 산출물의 출력 경로
  ```bash
  # CI/CD 파이프라인에서 유용
  dotnet build -c Release -o /tmp/build-output
  ```

- `--no-restore`: 빌드 전 자동 패키지 복원 생략
  ```bash
  # 별도로 restore를 실행한 경우 빌드 시간 단축
  dotnet restore
  dotnet build --no-restore
  ```

- `-v` 또는 `--verbosity`: 빌드 출력의 상세 수준 제어
  - `quiet` (q): 최소 출력 (오류와 경고만)
  - `minimal` (m): 요약 정보
  - `normal` (n): 기본 출력 수준
  - `detailed` (d): 상세 정보
  - `diagnostic` (diag): 모든 진단 정보 (빌드 문제 해결용)

  ```bash
  # 빌드 문제를 디버깅할 때
  dotnet build -v diagnostic
  
  # CI/CD에서 간결한 로그를 원할 때
  dotnet build -v minimal
  ```

- `--no-dependencies`: 프로젝트 참조를 빌드하지 않음
  ```bash
  # 현재 프로젝트만 빌드 (참조된 프로젝트는 이미 빌드된 상태)
  dotnet build --no-dependencies
  ```

**빌드 최적화 전략:**

```bash
# 1. 병렬 빌드 활성화 (기본값이지만 명시적 제어 가능)
dotnet build --property:MaxCpuCount=0  # 0 = 사용 가능한 모든 CPU 코어

# 2. 증분 빌드 최대 활용
# 변경된 파일만 재컴파일 (기본 동작)
dotnet build

# 3. 멀티 타겟팅 프로젝트에서 특정 프레임워크만 빌드
dotnet build --framework net10.0
```

**실전 예제: CI/CD 파이프라인 빌드**

```bash
# GitHub Actions, Azure DevOps 등에서 사용하는 전형적인 빌드 스크립트

# 1. 종속성 복원 (별도 단계로 캐싱 가능)
dotnet restore --verbosity minimal

# 2. Debug 빌드 (개발/테스트용)
dotnet build --no-restore --configuration Debug

# 3. Release 빌드 (배포용)
dotnet build --no-restore --configuration Release --output ./artifacts

# 4. 빌드 경고를 오류로 처리 (코드 품질 강제)
dotnet build --warnaserror
```

### 애플리케이션 실행: 개발 중인 코드의 테스트

`dotnet run` 명령어는 개발 과정에서 가장 자주 사용하는 명령어입니다. 이 명령어는 빌드와 실행을 한 번에 수행하여, 코드 변경 후 즉시 결과를 확인할 수 있게 합니다.

**실행 프로세스:**

`dotnet run`을 실행하면 내부적으로 다음 단계가 진행됩니다:

1. 프로젝트가 변경되었는지 확인 (증분 빌드)
2. 필요시 자동으로 `dotnet build` 실행
3. 빌드된 어셈블리를 .NET 런타임으로 실행
4. 프로그램의 표준 출력/에러를 콘솔에 연결

```bash
# 기본 실행 - 현재 디렉터리의 프로젝트 빌드 및 실행
dotnet run

# 명령줄 인수 전달
# `--` 이후의 모든 인수는 애플리케이션으로 전달됩니다
dotnet run -- arg1 arg2 arg3
dotnet run -- --config production --port 5000

# 특정 프로젝트 파일 지정하여 실행
dotnet run --project ./MyApp/MyApp.csproj

# Release 구성으로 실행
# 개발 중에도 프로덕션과 유사한 성능 테스트 가능
dotnet run --configuration Release
dotnet run -c Release

# 빌드 없이 이전 빌드 결과만 실행 (빌드 시간 절약)
dotnet run --no-build

# 복원 없이 실행 (모든 패키지가 이미 복원된 상태)
dotnet run --no-restore

# 특정 프레임워크 대상으로 실행 (멀티 타겟팅 프로젝트)
dotnet run --framework net10.0
```

**명령줄 인수 처리 예제:**

```csharp
// Program.cs
if (args.Length > 0)
{
    Console.WriteLine($"받은 인수: {string.Join(", ", args)}");
}

// 실행:
// $ dotnet run -- hello world test
// 출력: 받은 인수: hello, world, test
```

**환경 변수와 함께 실행:**

```bash
# Linux/macOS
ASPNETCORE_ENVIRONMENT=Production dotnet run

# Windows (PowerShell)
$env:ASPNETCORE_ENVIRONMENT="Production"; dotnet run

# Windows (CMD)
set ASPNETCORE_ENVIRONMENT=Production && dotnet run
```

**개발 워크플로우 최적화:**

```bash
# Hot Reload 활용 (dotnet watch)
# 파일 변경 시 자동으로 재빌드 및 재실행
dotnet watch run

# 이렇게 하면 코드를 수정하고 저장하는 즉시
# 애플리케이션이 자동으로 재시작됩니다
```

**디버깅 정보와 함께 실행:**

```bash
# 환경 변수를 통한 상세 로깅 활성화
DOTNET_LOG_LEVEL=Debug dotnet run

# ASP.NET Core 애플리케이션의 개발자 예외 페이지 활성화
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

### 빌드 산출물 정리

개발 과정에서 빌드 산출물이 쌓이면 디스크 공간을 차지하고, 때로는 오래된 빌드 결과로 인한 혼란을 야기할 수 있습니다. `dotnet clean` 명령어는 이러한 산출물을 깔끔하게 제거합니다.

```bash
# 기본 정리 - bin과 obj 디렉터리 삭제
dotnet clean

# 특정 구성의 출력만 정리
dotnet clean --configuration Release
dotnet clean -c Debug

# 특정 프레임워크의 출력만 정리
dotnet clean --framework net10.0

# 출력 디렉터리도 함께 정리
dotnet clean --output ./build
```

**정리되는 디렉터리:**

- `bin/`: 빌드된 어셈블리(.dll, .exe)와 관련 파일
- `obj/`: 중간 빌드 파일, 컴파일 캐시, 프로젝트 자산 파일

**실무 활용 시나리오:**

```bash
# 빌드 문제 해결: 완전한 클린 빌드
dotnet clean
dotnet build

# Git 브랜치 전환 전 정리
dotnet clean
git checkout feature-branch
dotnet restore
dotnet build

# CI/CD에서 항상 깨끗한 상태로 빌드
dotnet clean --configuration Release
dotnet build --configuration Release
```

### 배포를 위한 게시: 프로덕션 준비

`dotnet publish` 명령어는 애플리케이션을 배포 가능한 형태로 패키징합니다. 단순한 빌드와 달리, publish는 런타임에 필요한 모든 파일을 포함하여 자체 완결적인 배포 패키지를 생성합니다.

**프레임워크 의존(Framework-Dependent) vs 자체 포함(Self-Contained):**

.NET 애플리케이션은 두 가지 방식으로 배포할 수 있습니다:

1. **프레임워크 의존 배포 (FDD - Framework-Dependent Deployment)**:
   - 대상 시스템에 .NET 런타임이 설치되어 있어야 함
   - 작은 배포 크기 (애플리케이션 코드만 포함)
   - 여러 애플리케이션이 동일한 런타임 공유 가능
   - 런타임 버그 수정 시 자동으로 혜택

2. **자체 포함 배포 (SCD - Self-Contained Deployment)**:
   - .NET 런타임이 애플리케이션과 함께 패키징됨
   - 큰 배포 크기 (런타임 포함으로 100MB 이상 가능)
   - 대상 시스템에 .NET 설치 불필요
   - 런타임 버전 완전 제어 가능

```bash
# 기본 게시 - 프레임워크 의존 배포
dotnet publish

# Release 구성으로 게시 (권장)
dotnet publish -c Release

# 특정 런타임용 자체 포함 게시
# Windows 64비트
dotnet publish -c Release -r win-x64 --self-contained

# Linux 64비트
dotnet publish -c Release -r linux-x64 --self-contained

# macOS ARM64 (Apple Silicon)
dotnet publish -c Release -r osx-arm64 --self-contained

# 프레임워크 의존 배포 (명시적)
dotnet publish -c Release --self-contained false

# 단일 파일로 게시 - 모든 파일을 하나의 실행 파일로 번들링
dotnet publish -c Release -r win-x64 --self-contained \
  -p:PublishSingleFile=true

# 단일 파일 + 네이티브 코드 트리밍으로 크기 최소화
dotnet publish -c Release -r win-x64 --self-contained \
  -p:PublishSingleFile=true \
  -p:PublishTrimmed=true \
  -p:TrimMode=link

# ReadyToRun 컴파일 - 시작 시간 개선
dotnet publish -c Release -r win-x64 --self-contained \
  -p:PublishReadyToRun=true
```

**게시 옵션 상세:**

- `-r` 또는 `--runtime`: 런타임 식별자(RID) 지정
  - 일반적인 RID:
    - `win-x64`, `win-x86`, `win-arm64`
    - `linux-x64`, `linux-arm64`, `linux-musl-x64`
    - `osx-x64`, `osx-arm64`
  - 전체 목록: https://docs.microsoft.com/en-us/dotnet/core/rid-catalog

- `--self-contained`: 자체 포함 여부 (true/false)

- `-o` 또는 `--output`: 게시 출력 디렉터리
  ```bash
  dotnet publish -c Release -o /var/www/myapp
  ```

**고급 게시 옵션 (프로젝트 속성):**

```bash
# PublishSingleFile - 단일 실행 파일 생성
-p:PublishSingleFile=true

# PublishTrimmed - 사용하지 않는 어셈블리 제거
-p:PublishTrimmed=true

# PublishReadyToRun - AOT 컴파일로 시작 시간 단축
-p:PublishReadyToRun=true

# IncludeNativeLibrariesForSelfExtract - 네이티브 라이브러리도 단일 파일에 포함
-p:IncludeNativeLibrariesForSelfExtract=true

# DebugType=None - 디버그 심볼 제외하여 크기 감소
-p:DebugType=None
```

**컨테이너 배포를 위한 게시:**

```bash
# Docker 컨테이너용 Linux 게시
dotnet publish -c Release -r linux-x64 --self-contained false

# Alpine Linux용 (더 작은 이미지)
dotnet publish -c Release -r linux-musl-x64 --self-contained false
```

**실전 배포 예제:**

```bash
# 1. 프로덕션 웹 애플리케이션 배포
dotnet publish -c Release \
  -o /var/www/production \
  --self-contained false \
  -p:PublishReadyToRun=true

# 2. 독립 실행형 도구 배포 (사용자가 .NET을 설치할 필요 없음)
dotnet publish -c Release \
  -r win-x64 \
  --self-contained true \
  -p:PublishSingleFile=true \
  -p:PublishTrimmed=true \
  -o ./release

# 3. 최소 크기 배포 (임베디드 시스템, IoT)
dotnet publish -c Release \
  -r linux-arm \
  --self-contained true \
  -p:PublishSingleFile=true \
  -p:PublishTrimmed=true \
  -p:TrimMode=link \
  -p:DebugType=None
```

**배포 크기 비교:**

- 프레임워크 의존: ~200KB (애플리케이션만)
- 자체 포함: ~60-80MB (런타임 포함)
- 자체 포함 + Trimmed: ~30-40MB
- 자체 포함 + Trimmed + SingleFile: ~30-40MB (단일 파일)

---

## B.3 패키지 관리

현대적인 소프트웨어 개발에서 외부 라이브러리와 패키지의 활용은 필수적입니다. "바퀴를 재발명하지 말라(Don't reinvent the wheel)"는 개발 원칙에 따라, 검증된 오픈소스 라이브러리를 활용하면 개발 시간을 크게 단축하고 코드 품질을 향상시킬 수 있습니다. .NET 생태계는 NuGet이라는 강력한 패키지 관리 시스템을 중심으로 형성되어 있으며, .NET CLI는 이를 완벽하게 통합하고 있습니다.

**NuGet의 역할과 중요성:**

NuGet(https://www.nuget.org)은 .NET을 위한 공식 패키지 매니저입니다. 2023년 기준으로 30만 개 이상의 패키지가 등록되어 있으며, 다음과 같은 역할을 수행합니다:

1. **의존성 해결(Dependency Resolution)**: 패키지 간의 복잡한 의존 관계를 자동으로 해결
2. **버전 관리(Version Management)**: Semantic Versioning을 통한 체계적인 버전 관리
3. **중앙 저장소(Central Repository)**: 신뢰할 수 있는 패키지의 중앙 집중식 배포
4. **보안 감사(Security Auditing)**: 알려진 보안 취약점이 있는 패키지 경고

### 패키지 추가: 외부 라이브러리 통합

프로젝트에 NuGet 패키지를 추가하는 것은 매우 간단하지만, 버전 관리와 호환성을 신중하게 고려해야 합니다.

```bash
# 최신 안정 버전 추가
# NuGet.org에서 가장 최신의 안정(stable) 버전을 자동으로 선택
dotnet add package Newtonsoft.Json

# 특정 버전 지정
# Semantic Versioning: major.minor.patch
dotnet add package Newtonsoft.Json --version 13.0.3

# 버전 범위 지정
dotnet add package Newtonsoft.Json --version "13.*"    # 13.x.x 최신
dotnet add package Newtonsoft.Json --version "[13.0,14.0)"  # 13.0 이상 14.0 미만

# 프리릴리스 버전 포함하여 최신 버전 추가
# 베타, RC 등의 사전 릴리스 버전도 고려
dotnet add package Microsoft.EntityFrameworkCore --prerelease

# 특정 프로젝트에 패키지 추가 (솔루션 루트에서 실행 시)
dotnet add ./src/MyApp/MyApp.csproj package Serilog

# 특정 NuGet 소스에서 패키지 추가
dotnet add package MyCompany.Internal.Library \
  --source https://nuget.mycompany.com/v3/index.json
```

**Semantic Versioning 이해하기:**

NuGet은 Semantic Versioning(SemVer)을 사용합니다:

- **Major (주 버전)**: 호환되지 않는 API 변경
  - 예: 1.x.x → 2.0.0
  - 기존 코드가 깨질 수 있음
  
- **Minor (부 버전)**: 이전 버전과 호환되는 기능 추가
  - 예: 2.1.x → 2.2.0
  - 기존 코드는 그대로 작동하지만 새 기능 사용 가능
  
- **Patch (패치 버전)**: 버그 수정
  - 예: 2.2.1 → 2.2.2
  - 기능 변경 없이 버그만 수정

**프로젝트 파일 변경 이해:**

패키지를 추가하면 `.csproj` 파일에 다음과 같이 기록됩니다:

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
</ItemGroup>
```

**실무 패키지 선택 가이드:**

```bash
# 1. JSON 처리
dotnet add package Newtonsoft.Json  # 전통적, 풍부한 기능
# 또는
dotnet add package System.Text.Json  # .NET 내장, 고성능

# 2. HTTP 클라이언트 (고급 기능)
dotnet add package RestSharp
dotnet add package Flurl.Http

# 3. 로깅
dotnet add package Serilog
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.File

# 4. ORM (데이터베이스 액세스)
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Dapper

# 5. 테스팅
dotnet add package xunit
dotnet add package Moq  # Mocking 프레임워크
dotnet add package FluentAssertions  # 가독성 높은 어설션

# 6. 의존성 주입
dotnet add package Microsoft.Extensions.DependencyInjection

# 7. 구성 관리
dotnet add package Microsoft.Extensions.Configuration
dotnet add package Microsoft.Extensions.Configuration.Json
```

**주요 옵션 설명:**

- `--version`: 특정 버전이나 버전 범위 지정
  ```bash
  --version 1.2.3          # 정확한 버전
  --version "1.2.*"        # 1.2.x 중 최신
  --version "[1.0,2.0)"    # 1.0 이상 2.0 미만
  --version "[1.0,)"       # 1.0 이상 모든 버전
  ```

- `--prerelease`: 안정 버전뿐만 아니라 프리릴리스 버전도 포함
  ```bash
  # 최신 RC, Beta 버전 설치
  dotnet add package AspNetCore.HealthChecks --prerelease
  ```

- `--source`: 대체 NuGet 피드 지정
  ```bash
  # 회사 내부 NuGet 서버
  dotnet add package MyCompany.SharedLibrary \
    --source https://nuget.internal.com
  
  # 로컬 디렉터리
  dotnet add package CustomPackage \
    --source ./local-packages
  ```

**취약점 확인:**

```bash
# 프로젝트의 패키지에서 알려진 보안 취약점 확인
dotnet list package --vulnerable

# 더 상세한 취약점 정보 표시
dotnet list package --vulnerable --include-transitive
```

### 패키지 제거와 관리

시간이 지나면서 더 이상 사용하지 않는 패키지나 더 나은 대안으로 교체해야 하는 패키지가 생깁니다. 깨끗한 의존성 관리는 프로젝트의 유지보수성을 높입니다.

```bash
# 프로젝트에서 패키지 제거
dotnet remove package Newtonsoft.Json

# 특정 프로젝트에서 패키지 제거 (솔루션 루트에서)
dotnet remove ./src/MyApp/MyApp.csproj package Serilog
```

**패키지 제거 시 주의사항:**

1. **의존성 확인**: 제거하려는 패키지에 의존하는 다른 코드가 있는지 확인
2. **전이적 의존성**: 다른 패키지가 해당 패키지에 의존할 수 있음
3. **빌드 검증**: 제거 후 반드시 빌드하여 오류 확인

### 패키지 목록 확인과 분석

프로젝트의 패키지 의존성을 파악하는 것은 보안, 유지보수, 업데이트 관리에 필수적입니다.

```bash
# 프로젝트의 직접 종속성만 나열
dotnet list package

# 전이적 종속성(간접 의존성)까지 모두 표시
dotnet list package --include-transitive

# 오래된 패키지 확인
dotnet list package --outdated

# 오래된 패키지 + 프리릴리스 버전 포함
dotnet list package --outdated --include-prerelease

# 더 높은 버전이 있는 패키지의 자세한 정보
dotnet list package --outdated --include-transitive

# 특정 프로젝트의 패키지 나열
dotnet list ./src/MyApp/MyApp.csproj package

# 솔루션의 모든 프로젝트 패키지 나열
dotnet list MySolution.sln package --include-transitive
```

**출력 이해하기:**

```
프로젝트 'MyApp'에는 다음 패키지 참조가 있습니다.
   [net10.0]: 
   최상위 수준 패키지                      요청됨    해결됨
   > Newtonsoft.Json                       13.0.1    13.0.3
   > Serilog                               2.12.0    2.12.0
   
   전이적 패키지                           해결됨
   > Microsoft.Extensions.Logging.Abstractions  7.0.0
```

- **최상위 수준 패키지**: 프로젝트 파일에 명시적으로 추가한 패키지
- **전이적 패키지**: 최상위 패키지가 의존하는 패키지
- **요청됨**: 프로젝트 파일에 명시된 버전
- **해결됨**: 실제로 사용되는 버전 (의존성 해결 후)

### 패키지 복원: 의존성 다운로드

`dotnet restore` 명령어는 프로젝트 파일에 정의된 모든 패키지를 NuGet 소스에서 다운로드합니다. 이는 프로젝트를 처음 클론했을 때나 패키지를 추가/제거한 후 실행합니다.

```bash
# 현재 프로젝트/솔루션의 모든 패키지 복원
dotnet restore

# 특정 프로젝트만 복원
dotnet restore MyApp.csproj

# 특정 NuGet 소스만 사용
dotnet restore --source https://api.nuget.org/v3/index.json

# 여러 소스 사용
dotnet restore \
  --source https://api.nuget.org/v3/index.json \
  --source https://nuget.internal.com

# 패키지를 특정 디렉터리에 복원 (캐시 제어)
dotnet restore --packages ./packages

# 복원 시 자세한 로그 출력
dotnet restore --verbosity detailed

# HTTP 캐시 무시 (신선한 패키지 가져오기)
dotnet restore --no-cache

# Lock 파일 생성/업데이트 (재현 가능한 빌드)
dotnet restore --use-lock-file
```

**복원 프로세스 이해:**

1. **Lock 파일 확인**: `packages.lock.json` 존재 시 해당 버전 사용
2. **NuGet 소스 쿼리**: 패키지 정보를 NuGet 서버에서 조회
3. **의존성 해결**: 패키지 간 의존 관계를 분석하여 호환되는 버전 선택
4. **다운로드**: 로컬 캐시(`~/.nuget/packages`)에 패키지 다운로드
5. **프로젝트 자산 생성**: `obj/project.assets.json` 파일 생성

**중요 참고사항:**

대부분의 경우 `dotnet restore`를 명시적으로 실행할 필요가 없습니다. `dotnet build`, `dotnet run`, `dotnet test` 등의 명령어는 자동으로 복원을 수행합니다. 하지만 다음 경우에는 유용합니다:

```bash
# CI/CD 파이프라인에서 빌드 단계 분리
dotnet restore  # 복원 (캐싱 가능)
dotnet build --no-restore  # 빌드만
dotnet test --no-restore --no-build  # 테스트만

# 오프라인 환경 준비
dotnet restore --packages ./offline-packages
# 패키지를 포함하여 배포
```

### 패키지 업데이트 전략

소프트웨어 패키지는 지속적으로 업데이트되며, 보안 패치, 버그 수정, 새로운 기능이 추가됩니다. 체계적인 업데이트 전략이 필요합니다.

```bash
# 특정 패키지를 최신 버전으로 업데이트
# 기존 패키지를 제거하지 않고 버전만 업데이트
dotnet add package Newtonsoft.Json

# 메이저 버전 업그레이드 (호환성 주의)
dotnet add package EntityFramework --version 7.*

# 모든 패키지 업데이트를 위한 스크립트 예제 (PowerShell)
# 주의: 수동으로 각 패키지를 검토하는 것이 안전
dotnet list package --outdated --format json | 
  ConvertFrom-Json |
  ForEach-Object { dotnet add package $_.name }
```

**업데이트 모범 사례:**

1. **점진적 업데이트**: 한 번에 모든 패키지를 업데이트하지 말고 하나씩
2. **테스트 필수**: 업데이트 후 전체 테스트 스위트 실행
3. **변경 로그 확인**: Breaking changes가 있는지 릴리스 노트 검토
4. **메이저 버전 신중**: 메이저 버전 업그레이드는 충분한 테스트 후
5. **보안 우선**: 보안 취약점이 발견된 패키지는 최우선 업데이트

**NuGet.config 파일 활용:**

프로젝트나 솔루션 루트에 `NuGet.config` 파일을 생성하여 패키지 소스를 중앙에서 관리:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    <add key="company" value="https://nuget.company.com/v3/index.json" />
  </packageSources>
</configuration>
```

---

## B.4 테스트 실행

"테스트되지 않은 코드는 작동하지 않는 것으로 간주한다"는 소프트웨어 개발의 격언이 있습니다. 자동화된 테스트는 현대 소프트웨어 개발의 필수 요소이며, 코드 품질, 리팩토링 안전성, 그리고 지속적 통합/배포(CI/CD)의 기반이 됩니다. .NET CLI는 다양한 테스트 프레임워크와 완벽하게 통합되어 효율적인 테스트 워크플로우를 지원합니다.

**테스트 피라미드와 .NET:**

소프트웨어 테스팅에는 여러 계층이 있습니다:

1. **단위 테스트(Unit Tests)**: 개별 함수/메서드를 독립적으로 테스트
   - 가장 많은 수, 가장 빠른 실행
   - 격리된 환경에서 실행 (모킹 활용)
   
2. **통합 테스트(Integration Tests)**: 여러 컴포넌트의 상호작용 테스트
   - 데이터베이스, 파일 시스템, 외부 API 등과의 통합
   
3. **E2E 테스트(End-to-End Tests)**: 전체 시스템의 사용자 시나리오 테스트
   - 가장 적은 수, 가장 느린 실행

**.NET 테스트 프레임워크 생태계:**

.NET은 세 가지 주요 테스트 프레임워크를 지원합니다:

- **xUnit**: 현대적 설계, .NET Core/5+ 팀이 선호, 병렬 실행 최적화
- **NUnit**: 오래된 역사, JUnit에서 영감, 풍부한 어설션 API
- **MSTest**: Microsoft 공식, Visual Studio와 긴밀한 통합

### 테스트 프로젝트 생성과 구조

테스트 프로젝트는 일반적으로 테스트 대상 프로젝트와 별도로 생성하며, 명명 규칙으로 `.Tests` 접미사를 사용합니다.

```bash
# xUnit 테스트 프로젝트 생성 (권장)
dotnet new xunit -n MyApp.Tests

# NUnit 테스트 프로젝트 생성
dotnet new nunit -n MyApp.Tests

# MSTest 테스트 프로젝트 생성
dotnet new mstest -n MyApp.Tests

# 테스트 프로젝트에 테스트 대상 프로젝트 참조 추가
cd MyApp.Tests
dotnet add reference ../MyApp/MyApp.csproj

# 일반적인 테스팅 라이브러리 추가
dotnet add package Moq  # Mocking 프레임워크
dotnet add package FluentAssertions  # 표현력있는 어설션
dotnet add package Bogus  # 테스트 데이터 생성
```

**프로젝트 구조 예시:**

```
MySolution/
├── src/
│   ├── MyApp/
│   │   └── MyApp.csproj
│   └── MyApp.Core/
│       └── MyApp.Core.csproj
├── tests/
│   ├── MyApp.Tests/
│   │   ├── MyApp.Tests.csproj
│   │   ├── UnitTests/
│   │   │   ├── CalculatorTests.cs
│   │   │   └── StringHelperTests.cs
│   │   └── IntegrationTests/
│   │       ├── DatabaseTests.cs
│   │       └── ApiTests.cs
│   └── MyApp.Core.Tests/
│       └── MyApp.Core.Tests.csproj
└── MySolution.sln
```

### 테스트 실행: 다양한 시나리오

`dotnet test` 명령어는 프로젝트의 모든 테스트를 발견하고 실행합니다.

```bash
# 가장 기본적인 사용 - 현재 디렉터리의 테스트 프로젝트 실행
dotnet test

# 특정 테스트 프로젝트 실행
dotnet test MyApp.Tests/MyApp.Tests.csproj

# 솔루션의 모든 테스트 프로젝트 실행
dotnet test MySolution.sln

# Release 구성으로 테스트 (프로덕션과 유사한 환경)
dotnet test --configuration Release

# 빌드 없이 테스트 실행 (빌드가 이미 완료된 경우)
dotnet test --no-build

# 복원과 빌드 모두 생략 (CI/CD 최적화)
dotnet test --no-restore --no-build

# 상세한 출력으로 테스트 실행
dotnet test --verbosity detailed

# 조용한 모드 (오류만 표시)
dotnet test --verbosity quiet

# 병렬 실행 비활성화 (순차 실행, 디버깅 시 유용)
dotnet test -- RunConfiguration.MaxCpuCount=1
```

**테스트 결과 이해하기:**

```
테스트 실행 중입니다. MyApp.Tests.dll(.NETCoreApp,Version=v10.0)
Microsoft (R) Test Execution Command Line Tool 버전 17.8.0

총 테스트 수: 45. 통과: 43. 실패: 1. 건너뜀: 1.
테스트 실행 시간: 1.2345초
```

- **통과(Passed)**: 모든 어설션을 통과한 테스트
- **실패(Failed)**: 하나 이상의 어설션이 실패하거나 예외가 발생
- **건너뜀(Skipped)**: `[Fact(Skip = "이유")]`로 표시된 테스트

**주요 옵션 상세:**

- `--no-build`: 빌드 과정 생략 (빌드가 최신인 경우)
  ```bash
  dotnet build
  dotnet test --no-build  # 빌드 시간 절약
  ```

- `--no-restore`: 패키지 복원 생략

- `-c` 또는 `--configuration`: 테스트할 빌드 구성
  ```bash
  dotnet test -c Release  # 프로덕션 최적화 버전 테스트
  ```

- `--logger`: 테스트 결과 로거 지정
  ```bash
  dotnet test --logger "console;verbosity=detailed"
  dotnet test --logger "trx;LogFileName=test-results.trx"
  dotnet test --logger "html;LogFileName=test-results.html"
  ```

- `--filter`: 특정 테스트만 선택적으로 실행 (다음 섹션 참조)

- `--collect`: 코드 커버리지 또는 기타 진단 데이터 수집

### 테스트 필터링: 선택적 실행으로 효율성 향상

대규모 프로젝트에서는 수천 개의 테스트가 있을 수 있으며, 매번 모든 테스트를 실행하면 시간이 오래 걸립니다. 필터를 사용하면 특정 부분만 선택적으로 테스트할 수 있습니다.

```bash
# 완전히 정규화된 이름(Full Qualified Name)으로 필터링
# 특정 네임스페이스의 모든 테스트
dotnet test --filter "FullyQualifiedName~MyApp.Tests.UnitTests"

# 특정 클래스의 모든 테스트
dotnet test --filter "FullyQualifiedName~MyApp.Tests.UnitTests.CalculatorTests"

# 특정 테스트 메서드만 실행
dotnet test --filter "FullyQualifiedName=MyApp.Tests.UnitTests.CalculatorTests.Add_TwoNumbers_ReturnsSum"

# 테스트 이름으로 필터링 (간단한 방식)
dotnet test --filter "Name~Calculator"

# 여러 조건 조합 (AND)
dotnet test --filter "FullyQualifiedName~UnitTests&Category=Fast"

# 여러 조건 중 하나만 만족 (OR)
dotnet test --filter "Category=Fast|Category=Smoke"

# NOT 조건
dotnet test --filter "Category!=Slow"

# 복잡한 조건 조합
dotnet test --filter "(Category=Fast|Category=Smoke)&FullyQualifiedName~Calculator"
```

**테스트 카테고리/특성(Trait) 활용:**

xUnit에서 특성을 정의하여 테스트를 그룹화:

```csharp
// xUnit 테스트 예제
public class CalculatorTests
{
    [Fact]
    [Trait("Category", "Unit")]
    [Trait("Priority", "High")]
    public void Add_TwoNumbers_ReturnsSum()
    {
        // 테스트 코드
    }
    
    [Fact]
    [Trait("Category", "Integration")]
    [Trait("Priority", "Medium")]
    public void Add_WithDatabase_SavesResult()
    {
        // 테스트 코드
    }
}

// 필터링 실행
dotnet test --filter "Category=Unit"
dotnet test --filter "Priority=High"
dotnet test --filter "Category=Unit&Priority=High"
```

**실무 필터링 전략:**

```bash
# 1. 빠른 피드백 루프: 단위 테스트만 실행 (개발 중)
dotnet test --filter "Category=Unit"

# 2. 통합 테스트는 CI/CD에서만
dotnet test --filter "Category=Integration"

# 3. Smoke 테스트: 기본 기능 빠른 검증
dotnet test --filter "Category=Smoke"

# 4. 느린 테스트 제외
dotnet test --filter "Category!=Slow"

# 5. 변경된 컴포넌트 관련 테스트만
dotnet test --filter "FullyQualifiedName~Payment"
```

**필터 표현식 문법:**

- `=`: 정확히 일치
- `!=`: 일치하지 않음
- `~`: 포함 (부분 일치)
- `!~`: 포함하지 않음
- `|`: OR 연산
- `&`: AND 연산
- `()`: 그룹화

### 코드 커버리지: 테스트의 효과성 측정

코드 커버리지는 테스트가 소스 코드의 몇 %를 실행하는지 측정합니다. 100% 커버리지가 버그 없음을 보장하지는 않지만, 테스트되지 않은 코드를 식별하는 데 유용합니다.

```bash
# 기본 코드 커버리지 수집
dotnet test --collect:"XPlat Code Coverage"

# 특정 형식으로 커버리지 리포트 생성
# Coverlet을 사용한 OpenCover 형식
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover

# JSON 형식으로 출력
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=json

# Cobertura 형식 (Azure DevOps, Jenkins 등에서 사용)
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura

# 커버리지 출력 파일 경로 지정
dotnet test /p:CollectCoverage=true /p:CoverletOutput=./coverage/

# 특정 임계값 설정 (80% 미만이면 실패)
dotnet test /p:CollectCoverage=true /p:Threshold=80 /p:ThresholdType=line

# 여러 형식으로 동시 출력
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=\"opencover,lcov,json\"
```

**Coverlet 설정 (패키지 설치 필요):**

```bash
# Coverlet collector 추가 (테스트 프로젝트에)
dotnet add package coverlet.collector

# Coverlet MSBuild 추가 (더 많은 제어)
dotnet add package coverlet.msbuild
```

**커버리지 제외 규칙:**

```xml
<!-- csproj 파일에서 특정 파일/디렉터리 제외 -->
<PropertyGroup>
  <CoverletOutputFormat>cobertura</CoverletOutputFormat>
  <Exclude>[*]*.Models.*,[*]*.Migrations.*</Exclude>
</PropertyGroup>
```

**고급 커버리지 옵션:**

```bash
# 브랜치 커버리지 포함
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover /p:IncludeBranchCoverage=true

# 테스트 실패 시에도 커버리지 생성
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover /p:SkipAutoProps=true

# 특정 어셈블리만 커버리지 수집
dotnet test /p:CollectCoverage=true /p:Include="[MyApp]*"
```

**커버리지 리포트 시각화:**

```bash
# ReportGenerator 도구 설치
dotnet tool install -g dotnet-reportgenerator-globaltool

# 커버리지 데이터 수집
dotnet test --collect:"XPlat Code Coverage"

# HTML 리포트 생성
reportgenerator \
  -reports:./TestResults/*/coverage.cobertura.xml \
  -targetdir:./coverage-report \
  -reporttypes:Html

# 생성된 리포트 확인
# 브라우저에서 ./coverage-report/index.html 열기
```

### 테스트 결과 출력과 리포팅

CI/CD 파이프라인에서는 테스트 결과를 파일로 저장하여 분석하고 리포트를 생성합니다.

```bash
# TRX (Visual Studio Test Results) 형식으로 저장
dotnet test --logger "trx;LogFileName=test-results.trx"

# HTML 형식으로 저장
dotnet test --logger "html;LogFileName=test-results.html"

# JUnit 형식 (많은 CI 도구에서 지원)
dotnet test --logger "junit;LogFileName=test-results.xml"

# 여러 로거 동시 사용
dotnet test \
  --logger "console;verbosity=detailed" \
  --logger "trx;LogFileName=results.trx" \
  --logger "html;LogFileName=results.html"

# 결과 파일 경로 지정
dotnet test --logger "trx;LogFileName=/tmp/test-results/results.trx"
```

**CI/CD 통합 예제 (GitHub Actions):**

```bash
# 1. 테스트 실행 및 결과 저장
dotnet test \
  --configuration Release \
  --logger "trx;LogFileName=test-results.trx" \
  --collect:"XPlat Code Coverage" \
  /p:CollectCoverage=true \
  /p:CoverletOutputFormat=cobertura

# 2. 커버리지 리포트 생성
reportgenerator \
  -reports:./TestResults/*/coverage.cobertura.xml \
  -targetdir:./coverage-report \
  -reporttypes:"Html;Badges"

# 3. 아티팩트로 저장 (CI 시스템에서)
# - test-results.trx
# - coverage-report/
```

### 테스트 실행 모범 사례

```bash
# 개발 중 (빠른 피드백)
dotnet test --filter "Category=Unit" --verbosity minimal

# PR 검증 (중요한 테스트)
dotnet test --filter "Category=Unit|Category=Smoke" --logger "trx"

# 전체 테스트 (병합 전, 배포 전)
dotnet test --configuration Release --collect:"XPlat Code Coverage"

# CI/CD 파이프라인 (최적화된 순서)
dotnet restore
dotnet build --no-restore --configuration Release
dotnet test --no-build --no-restore --configuration Release \
  --logger "trx;LogFileName=test-results.trx" \
  --collect:"XPlat Code Coverage"
```

**성능 최적화:**

```bash
# 병렬 실행 최대화 (기본값)
dotnet test --parallel

# 테스트 어댑터의 병렬 처리 수준 제어
dotnet test -- xUnit.ParallelizeTestCollections=true

# 메모리 부족 시 순차 실행
dotnet test -- RunConfiguration.MaxCpuCount=1
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
