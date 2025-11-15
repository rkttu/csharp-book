# 23장. NuGet 패키지 관리

NuGet은 .NET 생태계의 공식 패키지 관리자로, 현대 소프트웨어 개발의 핵심 인프라입니다. 2010년 Microsoft의 Outercurve Foundation 프로젝트로 시작된 NuGet은 .NET 개발자들에게 코드 재사용성(Code Reusability), 의존성 관리(Dependency Management), 그리고 생산성 향상(Productivity Enhancement)을 제공하는 필수 도구가 되었습니다.

**패키지 관리자의 역사적 발전:**

소프트웨어 개발에서 패키지 관리자의 개념은 1990년대 초반 Linux 배포판의 등장과 함께 시작되었습니다. Debian의 APT(Advanced Package Tool, 1998)와 Red Hat의 RPM(Red Hat Package Manager, 1997)은 시스템 수준의 패키지 관리를 혁신했습니다. 이후 프로그래밍 언어별 패키지 관리자가 등장하기 시작했습니다:

- **CPAN** (Comprehensive Perl Archive Network, 1995): Perl 커뮤니티의 선구적 시도
- **RubyGems** (2004): Ruby 언어의 패키지 관리 표준 확립
- **npm** (Node Package Manager, 2010): JavaScript 생태계의 폭발적 성장 견인
- **NuGet** (2010): .NET 플랫폼의 공식 패키지 관리자 탄생
- **pip** (2011): Python Package Index(PyPI)와 함께 Python 생태계 통합

NuGet의 등장은 .NET 개발 패러다임의 근본적 전환점이었습니다. 이전에는 DLL 파일을 수동으로 다운로드하고 참조를 추가하는 번거로운 과정이 필요했으며, 버전 충돌(DLL Hell)과 의존성 추적의 어려움이 개발자들의 큰 고민이었습니다. NuGet은 이러한 문제를 해결하고 **선언적 의존성 관리(Declarative Dependency Management)**를 도입하여, .NET 생태계의 오픈소스 공유 문화를 꽃피웠습니다.

**NuGet의 아키텍처와 설계 철학:**

NuGet은 다음과 같은 핵심 설계 원칙을 따릅니다:

1. **버전 관리의 엄격성**: 시맨틱 버저닝(Semantic Versioning)을 통한 호환성 보장
2. **의존성 그래프 해석**: 복잡한 의존성 트리를 자동으로 해결
3. **패키지 불변성**: 게시된 패키지는 수정 불가능 (Immutable)
4. **분산 캐싱**: 로컬 캐시를 통한 네트워크 효율성 최적화
5. **복원 가능성**: 프로젝트 파일만으로 전체 의존성 재구성 가능

NuGet의 의존성 해석 알고리즘은 **그래프 이론(Graph Theory)**에 기반을 두고 있습니다. 각 패키지를 노드(Node)로, 의존성을 간선(Edge)로 표현하는 방향성 비순환 그래프(Directed Acyclic Graph, DAG)를 구성하여, 위상 정렬(Topological Sort) 알고리즘으로 설치 순서를 결정합니다. 이는 npm이나 Maven과 유사한 접근 방식이지만, .NET의 강력한 타입 시스템과 어셈블리 바인딩 리디렉션(Assembly Binding Redirection)을 고려한 독특한 해석 전략을 가지고 있습니다.

**NuGet과 소프트웨어 공학 원칙:**

NuGet의 설계는 여러 소프트웨어 공학 원칙을 실현합니다:

- **DRY (Don't Repeat Yourself)**: 코드 재사용을 통한 중복 제거
- **관심사의 분리(Separation of Concerns)**: 기능별 패키지 분리
- **느슨한 결합(Loose Coupling)**: 인터페이스 기반 의존성 주입
- **단일 책임 원칙(Single Responsibility Principle)**: 각 패키지는 하나의 명확한 목적

2024년 기준 NuGet Gallery에는 50만 개 이상의 패키지가 등록되어 있으며, 일일 다운로드 수는 수억 건에 달합니다. Newtonsoft.Json은 50억 회 이상 다운로드되어 .NET 역사상 가장 성공적인 패키지가 되었으며, Microsoft의 공식 패키지들도 대부분 NuGet을 통해 배포됩니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 NuGet 패키지 관리의 이론적 기반과 실무 활용 방법을 체계적으로 학습하게 됩니다:

- **NuGet의 역사와 아키텍처**: 패키지 관리자의 진화 과정, 의존성 해석 알고리즘, 시맨틱 버저닝의 원리와 실무적 적용, 그리고 .NET SDK와 NuGet의 깊은 통합 방식을 이해합니다.

- **패키지 생명주기 관리**: .NET CLI를 통한 패키지 설치/제거/업데이트/복원의 내부 동작 원리, 프로젝트 파일의 구조와 역할, 버전 충돌 해결 전략, 그리고 전이적 의존성(Transitive Dependencies) 관리 방법을 학습합니다.

- **핵심 NuGet 패키지의 깊이 있는 활용**: Newtonsoft.Json의 JSON 직렬화 메커니즘과 System.Text.Json과의 비교, Serilog의 구조화된 로깅 아키텍처와 Sink 생태계, Dapper의 Micro-ORM 설계 철학과 Entity Framework 대비 성능 특성을 실무 예제와 함께 이해합니다.

- **실무 패키지 관리 전략**: 패키지 보안 취약점 검사, 라이선스 준수, Private NuGet Feed 구축, CI/CD 파이프라인과의 통합, 그리고 패키지 버전 업그레이드 전략을 배웁니다.

**학습 목표:**

- NuGet의 아키텍처와 의존성 해석 메커니즘 이해
- 시맨틱 버저닝과 패키지 버전 관리 전략 수립
- .NET CLI를 통한 효율적인 패키지 관리 워크플로우 구축
- Newtonsoft.Json, Serilog, Dapper의 실무 활용 능력 배양
- 패키지 보안과 라이선스 관리의 중요성 인식
- 오픈소스 생태계 참여와 기여 방법 학습

---

## 23.1 NuGet이란?

NuGet(누겟, 발음: /ˈnuː.ɡɛt/)은 .NET 플랫폼의 공식 패키지 관리자로, 2010년 Microsoft의 Outercurve Foundation 프로젝트로 시작되어 현재는 .NET Foundation의 관리 하에 있습니다. NuGet이라는 이름은 "New Get"의 축약어이자 금덩이(nugget)의 이중적 의미를 담고 있으며, 가치 있는 코드 조각을 쉽게 얻을 수 있다는 철학을 반영합니다.

**NuGet의 탄생과 진화:**

2010년 이전 .NET 생태계는 코드 공유와 재사용에서 심각한 어려움을 겪고 있었습니다. 개발자들은 CodePlex나 SourceForge에서 수동으로 DLL을 다운로드하고, 버전을 추적하며, 의존성을 직접 관리해야 했습니다. 이는 다음과 같은 문제를 야기했습니다:

- **DLL Hell**: 서로 다른 라이브러리가 같은 이름의 DLL을 요구하는 버전 충돌
- **수동 업데이트**: 보안 패치나 버그 수정을 위한 수동 다운로드와 교체
- **의존성 추적의 어려움**: 라이브러리 A가 라이브러리 B를 필요로 할 때 수동 관리
- **재현 불가능한 빌드**: 팀원마다 다른 버전의 라이브러리를 사용하는 문제

Phil Haack, David Fowler 등 Microsoft 엔지니어들이 주도한 NuGet 프로젝트는 이러한 문제들을 해결하기 위해 시작되었습니다. 2010년 10월 처음 공개된 NuGet은 빠르게 .NET 커뮤니티의 지지를 받았으며, 2012년 Visual Studio 2012부터는 IDE에 기본 통합되었습니다. 2016년 .NET Core의 등장과 함께 NuGet은 .NET SDK의 핵심 구성 요소가 되었고, `dotnet` CLI를 통해 더욱 강력하고 일관된 경험을 제공하게 되었습니다.

**NuGet의 아키텍처 구성 요소:**

NuGet 생태계는 여러 계층의 구성 요소로 이루어져 있습니다:

```
┌─────────────────────────────────────────────────────────┐
│ 개발자 도구 (Developer Tools)                            │
│ • dotnet CLI                                            │
│ • Visual Studio Package Manager                         │
│ • Visual Studio Code Extensions                         │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ NuGet 클라이언트 (NuGet Client)                          │
│ • 패키지 설치/제거/업데이트 로직                           │
│ • 의존성 해석 엔진 (Dependency Resolver)                  │
│ • 버전 충돌 해결 알고리즘                                  │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ 로컬 캐시 (Local Cache)                                  │
│ • %USERPROFILE%\.nuget\packages (Windows)               │
│ • ~/.nuget/packages (Linux/macOS)                       │
│ • 빠른 패키지 복원을 위한 로컬 저장소                       │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ NuGet Gallery (Package Repository)                      │
│ • nuget.org (공식 공개 저장소)                            │
│ • Private Feeds (기업 내부 저장소)                        │
│ • Azure Artifacts, GitHub Packages 등                   │
└─────────────────────────────────────────────────────────┘
```

**패키지(Package)의 구조와 형식:**

NuGet 패키지는 `.nupkg` 확장자를 가진 ZIP 아카이브 파일입니다. 내부 구조는 Open Packaging Conventions (OPC) 표준을 따르며, 다음과 같은 요소를 포함합니다:

```
MyPackage.1.0.0.nupkg
├── [Content_Types].xml          # OPC 표준 메타데이터
├── _rels/                        # 관계 메타데이터
├── package/
│   └── services/
│       └── metadata/
│           └── core-properties/
│               └── *.psmdcp      # 패키지 메타데이터
├── MyPackage.nuspec              # 패키지 매니페스트 (핵심)
├── lib/
│   ├── net10.0/                  # 타겟 프레임워크별 어셈블리
│   │   └── MyPackage.dll
│   ├── netstandard2.0/
│   │   └── MyPackage.dll
│   └── net48/
│       └── MyPackage.dll
├── contentFiles/                 # 프로젝트에 포함될 파일
├── build/                        # MSBuild targets 및 props
└── tools/                        # 설치 스크립트 및 도구
```

**.nuspec 파일 - 패키지 매니페스트:**

`.nuspec` 파일은 XML 형식의 패키지 메타데이터로, 다음과 같은 정보를 포함합니다:

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <id>Newtonsoft.Json</id>
    <version>13.0.3</version>
    <authors>James Newton-King</authors>
    <description>Json.NET is a popular high-performance JSON framework for .NET</description>
    <projectUrl>https://www.newtonsoft.com/json</projectUrl>
    <license type="expression">MIT</license>
    <dependencies>
      <group targetFramework="net6.0">
        <!-- 이 패키지는 의존성이 없음 -->
      </group>
      <group targetFramework="netstandard2.0">
        <!-- netstandard2.0 타겟도 의존성 없음 -->
      </group>
    </dependencies>
    <tags>json serialization</tags>
  </metadata>
</package>
```

**시맨틱 버저닝 (Semantic Versioning)의 이론과 실무:**

NuGet은 Tom Preston-Werner가 제안한 Semantic Versioning 2.0.0 명세를 따릅니다. 버전 번호는 `MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]` 형식을 가지며, 각 요소는 명확한 의미를 가집니다:

```
13.0.3-beta.1+20240315
│  │ │  │    │ └─ 빌드 메타데이터 (선택적)
│  │ │  │    └─── 프리릴리스 레이블
│  │ │  └──────── 패치 버전 (하위 호환 버그 수정)
│  │  └─────────── 부 버전 (하위 호환 기능 추가)
│  └────────────── 주 버전 (호환성 깨지는 변경)
└───────────────── 패키지 ID와 함께 고유 식별자
```

**버전 증가 규칙:**

1. **MAJOR (주 버전)**: 하위 호환성을 깨는 API 변경 (Breaking Changes)
   - 공개 메서드 제거 또는 시그니처 변경
   - 기본 동작의 근본적 변경
   - 예: 1.5.2 → 2.0.0

2. **MINOR (부 버전)**: 하위 호환성을 유지하는 기능 추가
   - 새로운 메서드나 클래스 추가
   - 기존 기능 확장
   - 예: 1.5.2 → 1.6.0

3. **PATCH (패치 버전)**: 하위 호환 버그 수정
   - 버그 수정
   - 성능 개선
   - 예: 1.5.2 → 1.5.3

**프리릴리스 버전과 안정성:**

- **알파(alpha)**: 초기 개발 단계, 불완전한 기능
- **베타(beta)**: 기능 완료, 테스트 중
- **RC (Release Candidate)**: 출시 후보, 안정화 단계

예: `1.0.0-alpha.1 < 1.0.0-beta.1 < 1.0.0-rc.1 < 1.0.0`

**NuGet 갤러리와 패키지 생태계:**

NuGet Gallery (https://www.nuget.org)는 .NET 패키지의 중앙 저장소로, 다음과 같은 특징을 가집니다:

- **무료 호스팅**: 오픈소스 패키지를 무료로 호스팅
- **검색과 발견**: 강력한 검색 엔진과 태그 시스템
- **통계와 분석**: 다운로드 수, 의존성 그래프, 보안 취약점 정보
- **검증된 패키지**: Microsoft, .NET Foundation 등 신뢰할 수 있는 소스 표시
- **라이선스 정보**: SPDX 표준을 따르는 라이선스 식별자

2024년 기준 주요 통계:

- **총 패키지 수**: 500,000개 이상
- **일일 다운로드**: 1억 건 이상
- **가장 인기 있는 패키지**: Newtonsoft.Json (50억+ 다운로드)
- **Microsoft 공식 패키지**: 10,000개 이상

**의존성 해석 알고리즘 (Dependency Resolution):**

NuGet의 의존성 해석은 복잡한 그래프 문제를 해결하는 과정입니다. 다음과 같은 시나리오를 고려합니다:

```
프로젝트
├── 패키지A (1.0.0)
│   ├── 패키지C (>= 2.0.0)
│   └── 패키지D (1.5.0)
└── 패키지B (2.0.0)
    ├── 패키지C (>= 2.5.0)
    └── 패키지E (3.0.0)
```

이 경우 NuGet은 다음 과정을 거칩니다:

1. **그래프 구축**: 모든 의존성을 방향성 그래프로 표현
2. **버전 범위 분석**: 각 패키지의 버전 제약 조건 분석
3. **충돌 감지**: 호환되지 않는 버전 요구사항 찾기
4. **최적 버전 선택**: 모든 제약을 만족하는 최신 안정 버전 선택
5. **전이적 의존성 포함**: 의존성의 의존성까지 재귀적으로 해결

위 예제에서 패키지C는 2.5.0 이상 버전이 선택됩니다 (패키지B의 제약이 더 엄격).

**버전 충돌 해결 전략:**

NuGet은 다음과 같은 전략으로 버전 충돌을 해결합니다:

1. **최소 적용 가능 버전(Minimal Applicable Version)**: 제약을 만족하는 가장 낮은 버전 선택
2. **최대 안정 버전**: 프리릴리스를 제외한 최신 안정 버전 우선
3. **명시적 버전 고정**: 프로젝트 파일에 명시된 버전은 최우선
4. **바인딩 리디렉션**: .NET Framework에서 런타임 버전 매핑

**NuGet과 .NET SDK의 깊은 통합:**

.NET 5 이후 NuGet은 .NET SDK와 밀접하게 통합되어, 다음과 같은 이점을 제공합니다:

- **암시적 복원**: `dotnet build`나 `dotnet run` 실행 시 자동 복원
- **중앙 패키지 관리**: Directory.Packages.props를 통한 버전 중앙 관리
- **보안 감사**: `dotnet list package --vulnerable`로 취약점 검사
- **라이선스 검토**: `dotnet list package --include-transitive`로 모든 라이선스 확인

**로컬 캐시와 성능 최적화:**

NuGet은 다운로드한 패키지를 로컬 캐시에 저장하여 네트워크 사용을 최소화합니다:

```bash
# Windows
%USERPROFILE%\.nuget\packages

# Linux/macOS
~/.nuget/packages
```

캐시 구조:

```
packages/
├── newtonsoft.json/
│   ├── 13.0.1/
│   │   ├── lib/
│   │   ├── newtonsoft.json.13.0.1.nupkg
│   │   └── newtonsoft.json.nuspec
│   └── 13.0.3/
│       ├── lib/
│       ├── newtonsoft.json.13.0.3.nupkg
│       └── newtonsoft.json.nuspec
└── serilog/
    └── 3.1.1/
```

이를 통해:
- 동일한 패키지를 여러 프로젝트에서 재사용
- 오프라인 환경에서도 복원 가능
- 빌드 속도 대폭 향상

**NuGet CLI와 도구:**

```bash
# .NET SDK에 포함된 NuGet 기능
dotnet --version                    # .NET SDK 버전 확인
dotnet nuget --version              # NuGet 클라이언트 버전 확인

# 패키지 소스 관리
dotnet nuget list source            # 등록된 패키지 소스 목록
dotnet nuget add source https://api.nuget.org/v3/index.json --name official

# 캐시 관리
dotnet nuget locals all --list      # 모든 캐시 위치 확인
dotnet nuget locals all --clear     # 모든 캐시 삭제 (문제 해결용)
```

---

## 23.2 패키지 설치 및 제거

NuGet 패키지 관리의 핵심은 .NET CLI를 통한 선언적이고 재현 가능한 의존성 관리입니다. Visual Studio Code의 통합 터미널을 통해 프로젝트 폴더에서 명령어를 실행하며, 모든 변경사항은 프로젝트 파일(`*.csproj`)에 자동으로 반영되어 버전 관리 시스템과 자연스럽게 통합됩니다.

**패키지 관리의 워크플로우:**

```
1. 패키지 검색 및 선택
   ↓
2. 패키지 설치 (dotnet add package)
   ↓
3. 프로젝트 파일 업데이트 (*.csproj)
   ↓
4. 의존성 해석 및 다운로드
   ↓
5. 로컬 캐시에 저장
   ↓
6. 프로젝트에서 사용
```

### 패키지 검색과 선택 전략

**NuGet Gallery 웹 검색:**

https://www.nuget.org에서 패키지를 검색할 때 다음 요소들을 고려해야 합니다:

1. **다운로드 수**: 인기도와 커뮤니티 신뢰의 지표
2. **최근 업데이트**: 활발한 유지보수 여부 확인
3. **라이선스**: MIT, Apache 2.0 등 프로젝트 요구사항 확인
4. **의존성**: 추가로 설치될 패키지 확인
5. **타겟 프레임워크**: .NET 10 호환성 확인
6. **검증 배지**: Microsoft, .NET Foundation 등 신뢰 표시

**명령줄을 통한 패키지 검색:**

```bash
# dotnet CLI로 직접 검색 (제한적)
dotnet package search Serilog --take 5
```

**평가 기준:**

- **성능**: 벤치마크 결과 확인
- **문서화**: API 문서, 예제, 튜토리얼 품질
- **커뮤니티**: GitHub 스타 수, 이슈 해결 속도
- **호환성**: 다른 패키지와의 충돌 가능성
- **보안**: 알려진 취약점 확인

### 패키지 설치의 내부 동작

**.NET CLI 패키지 설치 명령:**

```bash
# 기본 설치 - 최신 안정 버전
dotnet add package Newtonsoft.Json

# 내부 동작:
# 1. nuget.org에서 Newtonsoft.Json의 최신 안정 버전 조회
# 2. 메타데이터 다운로드 및 의존성 분석
# 3. 호환되는 최신 버전 결정 (예: 13.0.3)
# 4. 패키지 다운로드 및 로컬 캐시에 저장
# 5. 프로젝트 파일에 PackageReference 추가
```

**특정 버전 설치:**

```bash
# 정확한 버전 지정
dotnet add package Newtonsoft.Json --version 13.0.3

# 버전 범위 지정 (프로젝트 파일 직접 편집)
# <PackageReference Include="Newtonsoft.Json" Version="[13.0,14.0)" />
# [13.0,14.0) = 13.0 이상, 14.0 미만
```

**버전 범위 표기법:**

NuGet은 다음과 같은 버전 범위 표기를 지원합니다:

```
1.0           = 1.0 ≤ 버전 (1.0 이상 최신)
[1.0]         = 1.0 (정확히 1.0)
(,1.0]        = 버전 ≤ 1.0
[1.0,2.0]     = 1.0 ≤ 버전 ≤ 2.0
(1.0,2.0)     = 1.0 < 버전 < 2.0
[1.0,2.0)     = 1.0 ≤ 버전 < 2.0
(1.0,)        = 1.0 < 버전
*             = 모든 버전 (비권장)
```

**프리릴리스 버전 설치:**

```bash
# 프리릴리스 버전 포함
dotnet add package Serilog --version 3.2.0-dev-02097

# 프리릴리스 포함 최신 버전
dotnet add package Serilog --prerelease
```

**프로젝트 파일 변화의 이해:**

패키지를 설치하면 `.csproj` 파일에 다음과 같이 추가됩니다:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <!-- NuGet 패키지 참조 -->
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
    <PackageReference Include="Serilog" Version="3.1.1" />
    <PackageReference Include="Serilog.Sinks.Console" Version="5.0.1" />
    <PackageReference Include="Dapper" Version="2.1.28" />
  </ItemGroup>

</Project>
```

**프로젝트 파일의 역할:**

1. **의존성 선언**: 필요한 패키지와 버전 명시
2. **버전 관리**: Git 등에 커밋하여 팀 전체가 동일한 버전 사용
3. **재현성**: 다른 환경에서 `dotnet restore`로 동일한 의존성 복원
4. **최소 정보**: 전이적 의존성은 자동 해석되므로 직접 의존성만 명시

**obj/project.assets.json - 의존성 잠금 파일:**

실제 설치된 패키지의 전체 의존성 트리는 `obj/project.assets.json`에 기록됩니다. 이 파일은 NuGet의 의존성 해석 결과를 캐싱하여 빌드 성능을 향상시킵니다.

```json
{
  "version": 3,
  "targets": {
    "net10.0": {
      "Newtonsoft.Json/13.0.3": {
        "type": "package",
        "compile": {
          "lib/net6.0/Newtonsoft.Json.dll": {}
        },
        "runtime": {
          "lib/net6.0/Newtonsoft.Json.dll": {}
        }
      }
    }
  },
  "libraries": {
    "Newtonsoft.Json/13.0.3": {
      "sha512": "HrC5BXdl00IP9zeV+0Z3Zvp+SeQiS8Z3TPyJvGcQiP+...",
      "type": "package",
      "path": "newtonsoft.json/13.0.3",
      "files": [...]
    }
  }
}
```

이 파일은 Git에 커밋하지 않아야 합니다 (`.gitignore`에 `obj/` 포함).

### 패키지 제거와 정리

**패키지 제거:**

```bash
# 패키지 제거
dotnet remove package Newtonsoft.Json

# 내부 동작:
# 1. 프로젝트 파일에서 PackageReference 제거
# 2. 의존성 그래프 재계산
# 3. 더 이상 필요하지 않은 전이적 의존성 식별
# 4. 로컬 캐시는 유지 (다른 프로젝트에서 사용 가능)
```

**고아 패키지 정리:**

패키지 A를 제거했을 때, A가 의존하던 패키지 B가 다른 곳에서 사용되지 않으면 자동으로 제외됩니다. 하지만 로컬 캐시는 유지되므로:

```bash
# 사용하지 않는 캐시 정리 (선택적)
dotnet nuget locals all --clear

# 주의: 모든 프로젝트의 캐시가 삭제되므로 다음 복원 시 다시 다운로드
```

### 패키지 업데이트 전략

**개별 패키지 업데이트:**

```bash
# 특정 패키지 최신 버전으로 업데이트
dotnet add package Newtonsoft.Json

# 이미 설치된 경우 최신 버전으로 업그레이드
```

**모든 패키지 업데이트:**

```bash
# 프로젝트의 모든 패키지 목록 확인
dotnet list package

# 업데이트 가능한 패키지 확인
dotnet list package --outdated

# 출력 예시:
# 프로젝트 'MyApp'에 다음 업데이트가 있습니다:
#    [net10.0]:
#    최상위 패키지        요청됨    해결됨    최신
#    > Serilog           3.0.0     3.0.0     3.1.1
#    > Newtonsoft.Json   13.0.1    13.0.1    13.0.3
```

**업데이트 시 고려사항:**

1. **주 버전 변경 (Major)**: Breaking Changes 가능성 - 코드 수정 필요
2. **부 버전 변경 (Minor)**: 새 기능 추가 - 하위 호환
3. **패치 변경 (Patch)**: 버그 수정 - 안전한 업데이트
4. **보안 패치**: 즉시 적용 권장

**전이적 의존성 확인:**

```bash
# 모든 의존성 트리 확인 (전이적 포함)
dotnet list package --include-transitive

# 출력 예시:
# 최상위 패키지        해결됨
# > Serilog           3.1.1
# 
# 전이적 패키지                        해결됨
# > Serilog.Sinks.File                5.0.0
#   > System.Text.Encodings.Web       6.0.0
```

### 패키지 복원 (Restore)

**자동 복원:**

```bash
# 명시적 복원 (대부분의 경우 불필요)
dotnet restore

# 다음 명령어들은 자동으로 restore를 수행:
dotnet build
dotnet run
dotnet test
dotnet publish
```

**복원 프로세스:**

1. 프로젝트 파일에서 PackageReference 읽기
2. NuGet Gallery에서 패키지 메타데이터 조회
3. 의존성 그래프 구축 및 버전 해석
4. 로컬 캐시 확인
5. 필요한 패키지 다운로드
6. `obj/project.assets.json` 생성

**오프라인 복원:**

로컬 캐시에 모든 패키지가 있으면 네트워크 없이 복원 가능:

```bash
# 로컬 캐시만 사용 (오프라인 모드)
dotnet restore --source ~/.nuget/packages
```

**CI/CD 환경에서의 복원:**

빌드 서버에서는 패키지 복원이 중요한 단계입니다:

```yaml
# GitHub Actions 예시
steps:
  - name: Restore dependencies
    run: dotnet restore
  - name: Build
    run: dotnet build --no-restore
  - name: Test
    run: dotnet test --no-restore
```

`--no-restore` 플래그를 사용하여 중복 복원을 방지하고 빌드 시간을 단축합니다.

### 설치된 패키지 확인과 관리

**패키지 목록 조회:**

```bash
# 현재 프로젝트의 패키지
dotnet list package

# 솔루션 내 모든 프로젝트의 패키지
dotnet list package --include-transitive

# 특정 프레임워크의 패키지
dotnet list package --framework net10.0
```

**보안 취약점 검사:**

```bash
# 알려진 보안 취약점이 있는 패키지 확인
dotnet list package --vulnerable

# 출력 예시:
# 다음 소스에 취약점이 있는 패키지가 검색되었습니다: GitHub Advisory Database
# 
#    [net10.0]:
#    최상위 패키지    요청됨    해결됨    심각도    권고 URL
#    > OldPackage    1.0.0     1.0.0     High      https://github.com/advisories/...
```

**더 이상 사용되지 않는 패키지 확인:**

```bash
# deprecated 패키지 확인
dotnet list package --deprecated

# 출력: 유지보수가 중단된 패키지 목록과 권장 대체 패키지
```

### Private NuGet Feed 설정

기업 환경에서는 내부 패키지를 위한 비공개 피드가 필요합니다:

**nuget.config 파일:**

프로젝트 또는 솔루션 루트에 `nuget.config` 파일 생성:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!-- 공식 NuGet Gallery -->
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    
    <!-- 회사 내부 피드 -->
    <add key="CompanyInternal" value="https://pkgs.dev.azure.com/company/_packaging/feed/nuget/v3/index.json" />
    
    <!-- 로컬 디렉토리 -->
    <add key="LocalPackages" value="./local-packages" />
  </packageSources>
  
  <packageSourceCredentials>
    <CompanyInternal>
      <add key="Username" value="%INTERNAL_FEED_USER%" />
      <add key="ClearTextPassword" value="%INTERNAL_FEED_PASSWORD%" />
    </CompanyInternal>
  </packageSourceCredentials>
</configuration>
```

**패키지 소스 관리 명령:**

```bash
# 패키지 소스 목록
dotnet nuget list source

# 새 소스 추가
dotnet nuget add source https://api.nuget.org/v3/index.json --name nuget.org

# 소스 제거
dotnet nuget remove source SourceName

# 소스 활성화/비활성화
dotnet nuget enable source SourceName
dotnet nuget disable source SourceName
```

### 패키지 관리의 모범 사례

**1. 버전 고정 vs 범위 지정:**

```xml
<!-- ✅ 권장: 특정 버전 고정 (재현성) -->
<PackageReference Include="Newtonsoft.Json" Version="13.0.3" />

<!-- ⚠️ 주의: 범위 지정 (자동 업데이트) -->
<PackageReference Include="Newtonsoft.Json" Version="13.*" />

<!-- ❌ 비권장: 와일드카드 (예측 불가) -->
<PackageReference Include="Newtonsoft.Json" Version="*" />
```

**2. 중앙 패키지 관리 (.NET 6+):**

솔루션 루트에 `Directory.Packages.props` 생성:

```xml
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>

  <ItemGroup>
    <PackageVersion Include="Newtonsoft.Json" Version="13.0.3" />
    <PackageVersion Include="Serilog" Version="3.1.1" />
  </ItemGroup>
</Project>
```

프로젝트 파일에서는 버전 생략:

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" />
  <PackageReference Include="Serilog" />
</ItemGroup>
```

**3. 정기적인 업데이트 및 보안 검토:**

```bash
# 주간 또는 월간 보안 검사
dotnet list package --vulnerable

# 업데이트 가능한 패키지 확인
dotnet list package --outdated

# 라이선스 준수 확인
dotnet list package --include-transitive | grep -i license
```

**4. 최소 의존성 원칙:**

- 필요한 패키지만 설치
- 대형 패키지보다 경량 대안 고려
- 전이적 의존성 최소화

---

## 23.3 유용한 NuGet 패키지 소개

실무에서 자주 사용되는 세 가지 핵심 패키지를 소개합니다. 이 패키지들은 JSON 처리, 로깅, 데이터베이스 접근 등 일반적인 작업을 크게 단순화해줍니다.

### 23.3.1 Newtonsoft.Json

**Newtonsoft.Json**(Json.NET)은 .NET에서 가장 인기 있는 JSON 직렬화 라이브러리입니다. 복잡한 JSON 데이터를 C# 객체로 변환하거나 그 반대 작업을 수행할 때 사용합니다.

**설치:**

```bash
dotnet add package Newtonsoft.Json
```

**기본 사용법:**

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;

// C# 객체 정의
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public List<string> Hobbies { get; set; }
}

// 객체를 JSON 문자열로 변환 (직렬화)
Person person = new Person
{
    Name = "홍길동",
    Age = 30,
    Hobbies = new List<string> { "독서", "코딩", "여행" }
};

string json = JsonConvert.SerializeObject(person, Formatting.Indented);
Console.WriteLine("JSON 출력:");
Console.WriteLine(json);

// JSON 문자열을 객체로 변환 (역직렬화)
string jsonString = @"
{
  ""Name"": ""김철수"",
  ""Age"": 25,
  ""Hobbies"": [""게임"", ""운동""]
}";

Person deserializedPerson = JsonConvert.DeserializeObject<Person>(jsonString);
Console.WriteLine($"\n역직렬화 결과: {deserializedPerson.Name}, {deserializedPerson.Age}세");

// 출력:
// JSON 출력:
// {
//   "Name": "홍길동",
//   "Age": 30,
//   "Hobbies": [
//     "독서",
//     "코딩",
//     "여행"
//   ]
// }
// 
// 역직렬화 결과: 김철수, 25세
```

**주요 기능:**

- **직렬화(Serialization)**: C# 객체 → JSON 문자열
- **역직렬화(Deserialization)**: JSON 문자열 → C# 객체
- **다양한 설정**: 날짜 형식, 들여쓰기, null 처리 등
- **LINQ to JSON**: JSON 데이터를 쿼리 가능

**System.Text.Json과의 비교:**

.NET Core 3.0부터 내장된 `System.Text.Json`도 있지만, Newtonsoft.Json은:
- 더 많은 기능과 유연성 제공
- 레거시 코드와의 호환성
- 복잡한 JSON 처리에 유리

### 23.3.2 Serilog

**Serilog**는 구조화된 로깅(Structured Logging)을 제공하는 강력한 로깅 라이브러리입니다. 애플리케이션의 실행 상태, 오류, 정보 메시지 등을 체계적으로 기록할 수 있습니다.

**설치:**

```bash
# Serilog 코어
dotnet add package Serilog

# 콘솔 출력을 위한 Sink
dotnet add package Serilog.Sinks.Console

# 파일 출력을 위한 Sink
dotnet add package Serilog.Sinks.File
```

**기본 사용법:**

```csharp
using Serilog;
using System;

// Serilog 설정
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .WriteTo.Console()
    .WriteTo.File("logs/myapp.txt", rollingInterval: RollingInterval.Day)
    .CreateLogger();

// 로그 작성
Log.Information("애플리케이션 시작됨");
Log.Debug("디버그 메시지: 변수 값 확인");
Log.Warning("경고: 리소스 사용량이 높습니다");
Log.Error("오류 발생: 파일을 찾을 수 없습니다");

// 구조화된 로깅 (변수를 포함)
string userName = "홍길동";
int loginAttempts = 3;
Log.Information("사용자 {UserName}이(가) {Attempts}번 로그인 시도", userName, loginAttempts);

// 예외 로깅
try
{
    int result = 10 / 0;
}
catch (Exception ex)
{
    Log.Error(ex, "계산 중 오류 발생");
}

// 로그 종료
Log.CloseAndFlush();

// 출력 예시:
// [10:30:15 INF] 애플리케이션 시작됨
// [10:30:15 DBG] 디버그 메시지: 변수 값 확인
// [10:30:15 WRN] 경고: 리소스 사용량이 높습니다
// [10:30:15 ERR] 오류 발생: 파일을 찾을 수 없습니다
// [10:30:15 INF] 사용자 "홍길동"이(가) 3번 로그인 시도
// [10:30:15 ERR] 계산 중 오류 발생
// System.DivideByZeroException: Attempted to divide by zero.
```

**주요 개념:**

- **로그 레벨**: Verbose, Debug, Information, Warning, Error, Fatal
- **Sink**: 로그를 출력할 대상 (콘솔, 파일, 데이터베이스 등)
- **구조화된 로깅**: 문자열 보간이 아닌 템플릿 사용으로 검색과 분석 용이

**Serilog의 장점:**

- 강력한 필터링 및 포맷팅
- 다양한 출력 대상 지원
- 성능 최적화
- 풍부한 생태계

### 23.3.3 Dapper

**Dapper**는 경량 ORM(Object-Relational Mapper)으로, SQL 데이터베이스 작업을 간소화해줍니다. Entity Framework보다 가볍고 빠르면서도, ADO.NET보다 사용하기 쉽습니다.

**설치:**

```bash
dotnet add package Dapper
dotnet add package Microsoft.Data.Sqlite
```

**기본 사용법:**

```csharp
using Dapper;
using Microsoft.Data.Sqlite;
using System;
using System.Collections.Generic;
using System.Linq;

// 데이터 모델
class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// SQLite 데이터베이스 연결
string connectionString = "Data Source=products.db";

using (var connection = new SqliteConnection(connectionString))
{
    connection.Open();
    
    // 테이블 생성
    connection.Execute(@"
        CREATE TABLE IF NOT EXISTS Products (
            Id INTEGER PRIMARY KEY AUTOINCREMENT,
            Name TEXT NOT NULL,
            Price REAL NOT NULL
        )
    ");
    
    // 데이터 삽입
    var newProduct = new Product { Name = "노트북", Price = 1500000 };
    connection.Execute(
        "INSERT INTO Products (Name, Price) VALUES (@Name, @Price)",
        newProduct
    );
    
    Console.WriteLine("제품 추가됨");
    
    // 데이터 조회 (단일 객체)
    var product = connection.QueryFirstOrDefault<Product>(
        "SELECT * FROM Products WHERE Id = @Id",
        new { Id = 1 }
    );
    
    if (product != null)
    {
        Console.WriteLine($"조회된 제품: {product.Name}, {product.Price:C}");
    }
    
    // 데이터 조회 (여러 객체)
    var products = connection.Query<Product>(
        "SELECT * FROM Products WHERE Price > @MinPrice",
        new { MinPrice = 1000000 }
    ).ToList();
    
    Console.WriteLine($"\n100만원 이상 제품 ({products.Count}개):");
    foreach (var p in products)
    {
        Console.WriteLine($"- {p.Name}: {p.Price:C}");
    }
    
    // 데이터 업데이트
    connection.Execute(
        "UPDATE Products SET Price = @Price WHERE Id = @Id",
        new { Id = 1, Price = 1450000 }
    );
    
    Console.WriteLine("\n가격 업데이트됨");
    
    // 데이터 삭제
    connection.Execute(
        "DELETE FROM Products WHERE Id = @Id",
        new { Id = 1 }
    );
    
    Console.WriteLine("제품 삭제됨");
}

// 출력:
// 제품 추가됨
// 조회된 제품: 노트북, ₩1,500,000.00
// 
// 100만원 이상 제품 (1개):
// - 노트북: ₩1,500,000.00
// 
// 가격 업데이트됨
// 제품 삭제됨
```

**주요 기능:**

- **Query<T>**: SQL 쿼리 결과를 객체 리스트로 변환
- **QueryFirstOrDefault<T>**: 첫 번째 결과 또는 기본값 반환
- **Execute**: INSERT, UPDATE, DELETE 등 실행
- **매개변수 바인딩**: SQL Injection 방지

**Dapper의 특징:**

- **Micro ORM**: 최소한의 오버헤드
- **SQL 제어**: 직접 SQL 작성으로 성능 최적화 가능
- **유연성**: 복잡한 쿼리 지원
- **빠른 속도**: Entity Framework보다 훨씬 빠름

**언제 Dapper를 사용할까?**

- 성능이 중요한 경우
- SQL을 직접 제어하고 싶을 때
- 간단한 CRUD 작업
- Entity Framework의 무거움을 피하고 싶을 때

---

## 23장 정리

이 장에서는 NuGet 패키지 관리자를 통해 .NET 생태계의 풍부한 라이브러리를 활용하는 방법을 배웠습니다.

**핵심 요약:**

✅ **NuGet의 이해**: .NET의 공식 패키지 관리자로 외부 라이브러리를 쉽게 관리

✅ **패키지 관리**: `dotnet add/remove package` 명령으로 설치와 제거

✅ **Newtonsoft.Json**: JSON 데이터 직렬화/역직렬화의 강력한 도구

✅ **Serilog**: 구조화된 로깅으로 애플리케이션 상태 추적

✅ **Dapper**: 경량 ORM으로 효율적인 데이터베이스 작업

**다음 단계:**

24장에서는 단위 테스트를 다루며, xUnit과 Moq를 사용하여 코드의 품질과 신뢰성을 높이는 방법을 학습합니다. 테스트 가능한 코드 작성은 전문 개발자의 필수 역량입니다.

**추가 학습 자료:**

- NuGet 공식 웹사이트: https://www.nuget.org
- Newtonsoft.Json 문서: https://www.newtonsoft.com/json/help/html/Introduction.htm
- Serilog 문서: https://serilog.net
- Dapper 문서: https://github.com/DapperLib/Dapper

**실습 과제:**

1. 간단한 콘솔 애플리케이션을 만들어 Newtonsoft.Json으로 JSON 파일을 읽고 쓰기
2. Serilog를 사용하여 애플리케이션의 실행 로그를 파일로 저장
3. Dapper로 간단한 도서 관리 시스템 만들기 (추가, 조회, 수정, 삭제)
