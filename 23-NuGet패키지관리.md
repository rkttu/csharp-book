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

.NET 생태계의 풍부함은 수십만 개의 NuGet 패키지에서 비롯됩니다. 이 절에서는 실무에서 필수적으로 사용되는 세 가지 핵심 패키지를 깊이 있게 다룹니다: Newtonsoft.Json (JSON 직렬화의 사실상 표준), Serilog (구조화된 로깅의 혁신), Dapper (Micro-ORM의 성능 모범사례). 이 패키지들은 각각 50억, 3억, 2억 회 이상 다운로드되어 .NET 개발자들의 일상적인 도구가 되었습니다.

**패키지 선택 시 고려사항:**

실무에서 패키지를 선택할 때는 다음 요소들을 종합적으로 평가해야 합니다:

1. **커뮤니티와 성숙도**: 다운로드 수, GitHub 스타, 이슈 해결 속도
2. **유지보수 상태**: 최근 업데이트, 보안 패치 이력
3. **성능 특성**: 벤치마크 결과, 메모리 사용량, CPU 효율성
4. **API 설계**: 학습 곡선, 문서화 품질, 예제 풍부함
5. **호환성**: 타겟 프레임워크, 다른 패키지와의 충돌 가능성
6. **라이선스**: MIT, Apache 2.0 등 프로젝트 요구사항 충족 여부

### 23.3.1 Newtonsoft.Json - JSON 직렬화의 황금 표준

**Newtonsoft.Json**(일명 Json.NET)은 James Newton-King이 2006년 시작한 오픈소스 프로젝트로, .NET 역사상 가장 성공적인 서드파티 라이브러리입니다. 50억 회 이상 다운로드되었으며, StackOverflow에서 "JSON .NET"으로 검색하면 10만 개 이상의 질문이 나옵니다. 이는 JSON 직렬화의 사실상 표준(de facto standard)이 되었으며, 많은 Microsoft 제품도 내부적으로 Newtonsoft.Json을 사용해왔습니다.

**역사적 배경과 중요성:**

2006년 .NET Framework에는 적절한 JSON 라이브러리가 없었습니다. `JavaScriptSerializer`는 느리고 제한적이었으며, WCF의 `DataContractJsonSerializer`는 복잡하고 무거웠습니다. James Newton-King은 이러한 공백을 메우기 위해 Json.NET을 개발했고, 2010년 NuGet의 등장과 함께 폭발적으로 확산되었습니다.

2019년 Microsoft는 .NET Core 3.0에 `System.Text.Json`을 도입하여 내장 JSON 라이브러리를 제공했지만, Newtonsoft.Json은 여전히 다음과 같은 이유로 널리 사용됩니다:

- 더 많은 기능과 유연한 설정
- 레거시 코드베이스와의 호환성
- 복잡한 타입 변환과 커스텀 컨버터
- 방대한 커뮤니티 지식과 예제

**설치:**

```bash
# 최신 안정 버전 설치
dotnet add package Newtonsoft.Json

# 설치 후 확인
dotnet list package

# 출력:
# 프로젝트 'MyApp'에 다음 패키지 참조가 있습니다.
#    [net10.0]:
#    최상위 패키지                요청됨    해결됨
#    > Newtonsoft.Json           13.0.3    13.0.3
```

**아키텍처와 설계 철학:**

Newtonsoft.Json은 다음과 같은 계층적 아키텍처를 가집니다:

```
┌─────────────────────────────────────────────────────┐
│ 고수준 API (High-Level API)                         │
│ • JsonConvert (정적 메서드)                          │
│ • JObject, JArray, JToken (LINQ to JSON)           │
└─────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────┐
│ 직렬화 엔진 (Serialization Engine)                   │
│ • JsonSerializer (구성 가능한 직렬화)                 │
│ • JsonSerializerSettings (설정 집합)                │
│ • Contract Resolver (타입 메타데이터)                │
└─────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────┐
│ 타입 변환 계층 (Type Conversion)                     │
│ • JsonConverter (커스텀 변환기)                      │
│ • TypeConverter 통합                                │
└─────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────┐
│ 저수준 파서 (Low-Level Parser)                       │
│ • JsonReader (스트리밍 읽기)                         │
│ • JsonWriter (스트리밍 쓰기)                         │
└─────────────────────────────────────────────────────┘
```

**기본 사용법 - 직렬화와 역직렬화:**

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;

// 도메인 모델 정의
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
    public List<string> Hobbies { get; set; }
    public Address HomeAddress { get; set; }
    public DateTime CreatedAt { get; set; }
}

public class Address
{
    public string City { get; set; }
    public string Street { get; set; }
}

// 객체를 JSON으로 직렬화 (Serialization)
Person person = new Person
{
    Id = 1,
    Name = "홍길동",
    Age = 30,
    Hobbies = new List<string> { "독서", "코딩", "여행" },
    HomeAddress = new Address { City = "서울", Street = "테헤란로" },
    CreatedAt = DateTime.Now
};

// 기본 직렬화 (압축 형식)
string jsonCompact = JsonConvert.SerializeObject(person);
Console.WriteLine("압축 JSON:");
Console.WriteLine(jsonCompact);

// 들여쓰기 포맷 (가독성 우선)
string jsonIndented = JsonConvert.SerializeObject(person, Formatting.Indented);
Console.WriteLine("\n들여쓰기 JSON:");
Console.WriteLine(jsonIndented);

// JSON 문자열을 객체로 역직렬화 (Deserialization)
string jsonString = @"{
  ""Id"": 2,
  ""Name"": ""김철수"",
  ""Age"": 25,
  ""Hobbies"": [""게임"", ""운동""],
  ""HomeAddress"": {
    ""City"": ""부산"",
    ""Street"": ""해운대로""
  },
  ""CreatedAt"": ""2024-01-15T10:30:00""
}";

Person deserializedPerson = JsonConvert.DeserializeObject<Person>(jsonString);
Console.WriteLine($"\n역직렬화 결과:");
Console.WriteLine($"이름: {deserializedPerson.Name}");
Console.WriteLine($"나이: {deserializedPerson.Age}세");
Console.WriteLine($"주소: {deserializedPerson.HomeAddress.City}");
Console.WriteLine($"취미: {string.Join(", ", deserializedPerson.Hobbies)}");

// 출력:
// 압축 JSON:
// {"Id":1,"Name":"홍길동","Age":30,"Hobbies":["독서","코딩","여행"],"HomeAddress":{"City":"서울","Street":"테헤란로"},"CreatedAt":"2024-11-15T07:00:00"}
//
// 들여쓰기 JSON:
// {
//   "Id": 1,
//   "Name": "홍길동",
//   "Age": 30,
//   "Hobbies": [
//     "독서",
//     "코딩",
//     "여행"
//   ],
//   "HomeAddress": {
//     "City": "서울",
//     "Street": "테헤란로"
//   },
//   "CreatedAt": "2024-11-15T07:00:00"
// }
```

**JsonSerializerSettings - 세밀한 제어:**

```csharp
// 고급 설정으로 직렬화 동작 커스터마이징
var settings = new JsonSerializerSettings
{
    // null 값 제외
    NullValueHandling = NullValueHandling.Ignore,
    
    // 기본값 제외 (0, false 등)
    DefaultValueHandling = DefaultValueHandling.Ignore,
    
    // 날짜 형식 지정
    DateFormatString = "yyyy-MM-dd HH:mm:ss",
    DateTimeZoneHandling = DateTimeZoneHandling.Utc,
    
    // 속성 이름을 camelCase로
    ContractResolver = new Newtonsoft.Json.Serialization.CamelCasePropertyNamesContractResolver(),
    
    // 들여쓰기
    Formatting = Formatting.Indented,
    
    // 참조 루프 처리 (순환 참조)
    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
    
    // 타입 정보 포함 (다형성 지원)
    TypeNameHandling = TypeNameHandling.Auto,
    
    // 알 수 없는 속성 무시
    MissingMemberHandling = MissingMemberHandling.Ignore
};

string customJson = JsonConvert.SerializeObject(person, settings);
Console.WriteLine(customJson);
```

**특성을 통한 직렬화 제어:**

```csharp
public class Product
{
    // 속성 이름 변경
    [JsonProperty("product_id")]
    public int Id { get; set; }
    
    // 속성 제외
    [JsonIgnore]
    public string InternalCode { get; set; }
    
    // 조건부 직렬화
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int Stock { get; set; }
    
    // 필수 속성
    [JsonRequired]
    public string Name { get; set; }
    
    // 읽기 전용 (역직렬화 시 무시)
    [JsonProperty(Required = Required.Default)]
    public string ReadOnlyProperty => $"{Name} (Readonly)";
    
    // 커스텀 컨버터 적용
    [JsonConverter(typeof(StringEnumConverter))]
    public ProductStatus Status { get; set; }
}

public enum ProductStatus
{
    Available,
    OutOfStock,
    Discontinued
}

// 사용
var product = new Product
{
    Id = 100,
    Name = "노트북",
    Stock = 0,  // DefaultValueHandling으로 제외됨
    Status = ProductStatus.Available,
    InternalCode = "SECRET"  // JsonIgnore로 제외됨
};

string productJson = JsonConvert.SerializeObject(product, Formatting.Indented);
Console.WriteLine(productJson);

// 출력:
// {
//   "product_id": 100,
//   "Name": "노트북",
//   "ReadOnlyProperty": "노트북 (Readonly)",
//   "Status": "Available"
// }
```

**LINQ to JSON - 동적 JSON 처리:**

타입이 미리 정의되지 않은 JSON을 처리할 때 LINQ to JSON을 사용합니다:

```csharp
// JSON 문자열 파싱
string dynamicJson = @"{
  ""users"": [
    { ""name"": ""Alice"", ""age"": 30, ""city"": ""Seoul"" },
    { ""name"": ""Bob"", ""age"": 25, ""city"": ""Busan"" },
    { ""name"": ""Charlie"", ""age"": 35, ""city"": ""Seoul"" }
  ],
  ""metadata"": {
    ""version"": ""1.0"",
    ""timestamp"": ""2024-11-15T07:00:00Z""
  }
}";

// JObject로 파싱
JObject root = JObject.Parse(dynamicJson);

// 속성 접근
string version = (string)root["metadata"]["version"];
Console.WriteLine($"Version: {version}");

// 배열 순회
JArray users = (JArray)root["users"];
foreach (JObject user in users)
{
    string name = (string)user["name"];
    int age = (int)user["age"];
    string city = (string)user["city"];
    Console.WriteLine($"{name} ({age}세, {city})");
}

// LINQ 쿼리
var seoulUsers = root["users"]
    .Where(u => (string)u["city"] == "Seoul")
    .Select(u => (string)u["name"])
    .ToList();

Console.WriteLine($"\n서울 거주자: {string.Join(", ", seoulUsers)}");

// 동적 생성
JObject newUser = new JObject
{
    ["name"] = "David",
    ["age"] = 28,
    ["city"] = "Incheon",
    ["hobbies"] = new JArray("게임", "독서")
};

users.Add(newUser);
Console.WriteLine($"\n업데이트된 JSON:\n{root.ToString(Formatting.Indented)}");

// 출력:
// Version: 1.0
// Alice (30세, Seoul)
// Bob (25세, Busan)
// Charlie (35세, Seoul)
// 
// 서울 거주자: Alice, Charlie
```

**커스텀 JsonConverter - 복잡한 타입 변환:**

```csharp
// 한국 날짜 형식 변환기
public class KoreanDateConverter : JsonConverter<DateTime>
{
    public override void WriteJson(JsonWriter writer, DateTime value, JsonSerializer serializer)
    {
        writer.WriteValue($"{value:yyyy년 MM월 dd일}");
    }

    public override DateTime ReadJson(JsonReader reader, Type objectType, DateTime existingValue, 
        bool hasExistingValue, JsonSerializer serializer)
    {
        string dateString = (string)reader.Value;
        // 간단한 파싱 (실제로는 더 견고한 구현 필요)
        return DateTime.ParseExact(dateString, "yyyy년 MM월 dd일", null);
    }
}

public class Event
{
    public string Title { get; set; }
    
    [JsonConverter(typeof(KoreanDateConverter))]
    public DateTime EventDate { get; set; }
}

// 사용
var evt = new Event
{
    Title = "회의",
    EventDate = new DateTime(2024, 11, 15)
};

string eventJson = JsonConvert.SerializeObject(evt, Formatting.Indented);
Console.WriteLine(eventJson);

// 출력:
// {
//   "Title": "회의",
//   "EventDate": "2024년 11월 15일"
// }
```

**Newtonsoft.Json vs System.Text.Json 비교:**

| 특성 | Newtonsoft.Json | System.Text.Json |
|------|-----------------|------------------|
| 릴리스 | 2006년 | 2019년 (.NET Core 3.0) |
| 성능 | 보통 | 매우 빠름 (2-3배) |
| 메모리 | 보통 | 낮음 (Span<T> 활용) |
| 기능 | 매우 풍부 | 기본적 (점차 개선 중) |
| 타입 지원 | 광범위 | 제한적 |
| 커스터마이징 | 매우 유연 | 제한적 |
| 레거시 호환 | 완벽 | 없음 |
| 학습 곡선 | 낮음 (많은 예제) | 중간 |
| 사용 사례 | 복잡한 JSON, 레거시 | 고성능, 새 프로젝트 |

**권장 사항:**
- **Newtonsoft.Json 선택**: 기존 코드, 복잡한 타입, 유연한 설정 필요
- **System.Text.Json 선택**: 새 프로젝트, 성능 중시, 간단한 JSON

**실무 활용 패턴:**

```csharp
// 1. REST API 응답 파싱
public class ApiResponse<T>
{
    public bool Success { get; set; }
    public T Data { get; set; }
    public string ErrorMessage { get; set; }
}

string apiJson = await httpClient.GetStringAsync("https://api.example.com/users");
var response = JsonConvert.DeserializeObject<ApiResponse<List<Person>>>(apiJson);

// 2. 설정 파일 저장/로드
public class AppConfig
{
    public string ApiKey { get; set; }
    public int Timeout { get; set; }
    public Dictionary<string, string> Settings { get; set; }
}

// 저장
var config = new AppConfig { ApiKey = "abc123", Timeout = 30 };
File.WriteAllText("config.json", JsonConvert.SerializeObject(config, Formatting.Indented));

// 로드
var loadedConfig = JsonConvert.DeserializeObject<AppConfig>(File.ReadAllText("config.json"));

// 3. 깊은 복사 (Deep Copy)
var original = new Person { Name = "원본" };
var deepCopy = JsonConvert.DeserializeObject<Person>(JsonConvert.SerializeObject(original));
// 완전히 독립적인 복사본 생성
```

**성능 최적화 팁:**

```csharp
// 1. JsonSerializerSettings 재사용
private static readonly JsonSerializerSettings CachedSettings = new JsonSerializerSettings
{
    // 설정...
};

// 매번 새로 생성하지 말고 재사용
string json = JsonConvert.SerializeObject(obj, CachedSettings);

// 2. 스트리밍 API 사용 (대용량 JSON)
using (var stringReader = new StringReader(largeJson))
using (var jsonReader = new JsonTextReader(stringReader))
{
    var serializer = new JsonSerializer();
    var largeObject = serializer.Deserialize<LargeType>(jsonReader);
}

// 3. 불필요한 타입 정보 제외
// TypeNameHandling.Auto 대신 None 사용 (보안과 성능)
```

### 23.3.2 Serilog - 구조화된 로깅의 혁명

**Serilog**는 Nicholas Blumhardt가 2013년 시작한 오픈소스 프로젝트로, .NET 생태계에 **구조화된 로깅(Structured Logging)**의 개념을 도입한 선구자입니다. 전통적인 문자열 기반 로깅을 넘어, 로그 이벤트를 구조화된 데이터로 취급하여 검색, 필터링, 분석을 혁신적으로 개선했습니다. 3억 회 이상 다운로드되었으며, ASP.NET Core, Azure Functions, AWS Lambda 등 현대 클라우드 애플리케이션의 표준 로깅 라이브러리로 자리 잡았습니다.

**구조화된 로깅의 철학:**

전통적인 로깅 (문자열 보간):
```csharp
// ❌ 검색과 분석이 어려움
logger.Info($"User {userName} logged in from {ipAddress}");
// 출력: "User Alice logged in from 192.168.1.1"
// 문제: userName과 ipAddress를 프로그래밍 방식으로 추출 불가
```

구조화된 로깅 (템플릿과 속성):
```csharp
// ✅ 구조화된 데이터로 저장
Log.Information("User {UserName} logged in from {IpAddress}", userName, ipAddress);
// 저장: { "UserName": "Alice", "IpAddress": "192.168.1.1", "Message": "User Alice logged in..." }
// 장점: Elasticsearch, Splunk 등에서 UserName="Alice" 쿼리 가능
```

**Serilog의 아키텍처:**

```
┌──────────────────────────────────────────────────────────┐
│ 로거 API (Logger API)                                     │
│ • Log.Information(), Log.Error() 등 정적 메서드           │
│ • ILogger 인터페이스 (의존성 주입 지원)                    │
└──────────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────────┐
│ 구성 계층 (Configuration Layer)                           │
│ • LoggerConfiguration (Fluent API)                       │
│ • MinimumLevel, Enrich, Filter                           │
└──────────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────────┐
│ 파이프라인 (Pipeline)                                      │
│ • Enrichers (컨텍스트 정보 추가)                           │
│ • Filters (조건부 필터링)                                  │
│ • Destructuring (객체 분해)                               │
└──────────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────────┐
│ Sinks (출력 대상)                                          │
│ • Console, File, Elasticsearch, Seq, Application Insights│
└──────────────────────────────────────────────────────────┘
```

**설치:**

```bash
# 핵심 라이브러리
dotnet add package Serilog

# 콘솔 출력 Sink
dotnet add package Serilog.Sinks.Console

# 파일 출력 Sink (로테이션 지원)
dotnet add package Serilog.Sinks.File

# ASP.NET Core 통합 (선택적)
dotnet add package Serilog.AspNetCore

# Enrichers (컨텍스트 정보 추가)
dotnet add package Serilog.Enrichers.Thread
dotnet add package Serilog.Enrichers.Environment
```

**기본 설정과 사용:**

```csharp
using Serilog;
using System;

// 글로벌 로거 구성
Log.Logger = new LoggerConfiguration()
    // 최소 로그 레벨 설정
    .MinimumLevel.Debug()
    
    // 콘솔 출력
    .WriteTo.Console(
        outputTemplate: "[{Timestamp:HH:mm:ss} {Level:u3}] {Message:lj}{NewLine}{Exception}")
    
    // 파일 출력 (일별 로테이션)
    .WriteTo.File(
        path: "logs/myapp-.txt",
        rollingInterval: RollingInterval.Day,
        outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level:u3}] {Message:lj}{NewLine}{Exception}",
        retainedFileCountLimit: 30  // 30일간 보관
    )
    
    // 컨텍스트 정보 추가
    .Enrich.WithThreadId()
    .Enrich.WithMachineName()
    
    .CreateLogger();

// 애플리케이션 시작 시
Log.Information("애플리케이션 시작: {AppName} v{Version}", "MyApp", "1.0.0");

// 다양한 로그 레벨
Log.Verbose("상세한 추적 정보 (개발 시에만)");
Log.Debug("디버깅 정보: 변수 값 확인");
Log.Information("일반적인 정보성 메시지");
Log.Warning("경고: 잠재적 문제 감지");
Log.Error("오류 발생: 처리 가능한 예외");
Log.Fatal("치명적 오류: 애플리케이션 종료 필요");

// 애플리케이션 종료 시 (필수!)
Log.CloseAndFlush();

// 출력 예시:
// [10:30:15 INF] 애플리케이션 시작: "MyApp" v"1.0.0"
// [10:30:15 DBG] 디버깅 정보: 변수 값 확인
// [10:30:15 WRN] 경고: 잠재적 문제 감지
```

**구조화된 로깅의 강력함:**

```csharp
// 구조화된 속성 로깅
string userName = "홍길동";
int userId = 12345;
string ipAddress = "192.168.1.100";

Log.Information(
    "사용자 로그인: {UserName} (ID: {UserId}) from {IpAddress}",
    userName, userId, ipAddress
);

// JSON 형식으로 저장됨:
// {
//   "Timestamp": "2024-11-15T10:30:15.123Z",
//   "Level": "Information",
//   "MessageTemplate": "사용자 로그인: {UserName} (ID: {UserId}) from {IpAddress}",
//   "Properties": {
//     "UserName": "홍길동",
//     "UserId": 12345,
//     "IpAddress": "192.168.1.100"
//   }
// }

// 이제 Elasticsearch나 Splunk에서 쿼리 가능:
// UserName:"홍길동" AND UserId:12345
```

**객체 분해 (Destructuring):**

복잡한 객체를 구조화된 로그로 기록:

```csharp
public class Order
{
    public int OrderId { get; set; }
    public string CustomerName { get; set; }
    public decimal TotalAmount { get; set; }
    public List<OrderItem> Items { get; set; }
}

public class OrderItem
{
    public string Product { get; set; }
    public int Quantity { get; set; }
}

var order = new Order
{
    OrderId = 1001,
    CustomerName = "김철수",
    TotalAmount = 150000,
    Items = new List<OrderItem>
    {
        new OrderItem { Product = "노트북", Quantity = 1 },
        new OrderItem { Product = "마우스", Quantity = 2 }
    }
};

// @ 연산자로 객체 분해 (Destructure)
Log.Information("주문 생성됨: {@Order}", order);

// 출력 (JSON 형식):
// {
//   "Order": {
//     "OrderId": 1001,
//     "CustomerName": "김철수",
//     "TotalAmount": 150000,
//     "Items": [
//       { "Product": "노트북", "Quantity": 1 },
//       { "Product": "마우스", "Quantity": 2 }
//     ]
//   }
// }

// $ 연산자로 ToString() 호출
Log.Information("주문 ID: {$OrderId}", order.OrderId);
```

**예외 로깅:**

```csharp
try
{
    // 위험한 작업
    ProcessPayment(order);
}
catch (PaymentException ex)
{
    // 예외를 첫 번째 인자로 전달
    Log.Error(ex, "결제 실패: 주문 {OrderId}, 고객 {CustomerName}",
        order.OrderId, order.CustomerName);
    
    // 예외의 스택 추적, InnerException 등이 자동으로 포함됨
}
catch (Exception ex)
{
    Log.Fatal(ex, "예상치 못한 오류 발생");
    throw;  // 재발생
}
```

**로그 레벨과 필터링:**

```csharp
Log.Logger = new LoggerConfiguration()
    // 전역 최소 레벨
    .MinimumLevel.Information()
    
    // 특정 네임스페이스는 더 높은 레벨
    .MinimumLevel.Override("Microsoft", LogEventLevel.Warning)
    .MinimumLevel.Override("System", LogEventLevel.Warning)
    
    // 개발 환경에서만 Debug 레벨
    .MinimumLevel.Override("MyApp", 
#if DEBUG
        LogEventLevel.Debug
#else
        LogEventLevel.Information
#endif
    )
    
    // 필터: 특정 조건의 로그만 기록
    .Filter.ByIncludingOnly(logEvent =>
        logEvent.Level >= LogEventLevel.Warning ||
        logEvent.Properties.ContainsKey("Important"))
    
    .WriteTo.Console()
    .CreateLogger();
```

**Enrichers - 컨텍스트 정보 자동 추가:**

```csharp
Log.Logger = new LoggerConfiguration()
    // 스레드 정보
    .Enrich.WithThreadId()
    .Enrich.WithThreadName()
    
    // 환경 정보
    .Enrich.WithMachineName()
    .Enrich.WithEnvironmentName()
    .Enrich.WithEnvironmentUserName()
    
    // 프로세스 정보
    .Enrich.WithProcessId()
    .Enrich.WithProcessName()
    
    // 사용자 정의 속성
    .Enrich.WithProperty("Application", "MyApp")
    .Enrich.WithProperty("Version", "1.0.0")
    
    .WriteTo.Console()
    .CreateLogger();

// 모든 로그에 자동으로 추가됨:
// {
//   "ThreadId": 5,
//   "MachineName": "SERVER01",
//   "Application": "MyApp",
//   "Version": "1.0.0",
//   ...
// }
```

**동적 로그 컨텍스트:**

```csharp
using Serilog.Context;

// HTTP 요청 처리 중
using (LogContext.PushProperty("RequestId", Guid.NewGuid()))
using (LogContext.PushProperty("UserId", currentUser.Id))
{
    Log.Information("API 호출 시작");
    ProcessRequest();
    Log.Information("API 호출 완료");
    
    // 이 블록 내의 모든 로그에 RequestId와 UserId가 자동 포함
}
```

**다양한 Sinks:**

```csharp
Log.Logger = new LoggerConfiguration()
    // 콘솔 (개발)
    .WriteTo.Console()
    
    // 파일 (프로덕션)
    .WriteTo.File(
        "logs/app.txt",
        rollingInterval: RollingInterval.Day,
        fileSizeLimitBytes: 10 * 1024 * 1024,  // 10MB
        rollOnFileSizeLimit: true
    )
    
    // Elasticsearch (중앙 집중식 로깅)
    // dotnet add package Serilog.Sinks.Elasticsearch
    .WriteTo.Elasticsearch(new ElasticsearchSinkOptions(new Uri("http://localhost:9200"))
    {
        AutoRegisterTemplate = true,
        IndexFormat = "myapp-logs-{0:yyyy.MM.dd}"
    })
    
    // Seq (구조화된 로그 서버)
    // dotnet add package Serilog.Sinks.Seq
    .WriteTo.Seq("http://localhost:5341")
    
    // Azure Application Insights
    // dotnet add package Serilog.Sinks.ApplicationInsights
    .WriteTo.ApplicationInsights(
        telemetryConfiguration,
        TelemetryConverter.Traces)
    
    // SQL Server
    // dotnet add package Serilog.Sinks.MSSqlServer
    .WriteTo.MSSqlServer(
        connectionString,
        sinkOptions: new MSSqlServerSinkOptions { TableName = "Logs" })
    
    .CreateLogger();
```

**ASP.NET Core와의 통합:**

```csharp
// Program.cs
using Serilog;

Log.Logger = new LoggerConfiguration()
    .ReadFrom.Configuration(builder.Configuration)  // appsettings.json에서 읽기
    .Enrich.FromLogContext()
    .WriteTo.Console()
    .CreateLogger();

try
{
    Log.Information("Starting web application");
    
    var builder = WebApplication.CreateBuilder(args);
    
    // Serilog를 ASP.NET Core 로깅에 연결
    builder.Host.UseSerilog();
    
    var app = builder.Build();
    
    // HTTP 요청 로깅 (자동)
    app.UseSerilogRequestLogging(options =>
    {
        options.MessageTemplate = "HTTP {RequestMethod} {RequestPath} responded {StatusCode} in {Elapsed:0.0000} ms";
        options.EnrichDiagnosticContext = (diagnosticContext, httpContext) =>
        {
            diagnosticContext.Set("RequestHost", httpContext.Request.Host.Value);
            diagnosticContext.Set("UserAgent", httpContext.Request.Headers["User-Agent"]);
        };
    });
    
    app.Run();
}
catch (Exception ex)
{
    Log.Fatal(ex, "Application terminated unexpectedly");
}
finally
{
    Log.CloseAndFlush();
}
```

**appsettings.json 구성:**

```json
{
  "Serilog": {
    "Using": ["Serilog.Sinks.Console", "Serilog.Sinks.File"],
    "MinimumLevel": {
      "Default": "Information",
      "Override": {
        "Microsoft": "Warning",
        "System": "Warning"
      }
    },
    "WriteTo": [
      {
        "Name": "Console",
        "Args": {
          "theme": "Serilog.Sinks.SystemConsole.Themes.AnsiConsoleTheme::Code, Serilog.Sinks.Console"
        }
      },
      {
        "Name": "File",
        "Args": {
          "path": "logs/log-.txt",
          "rollingInterval": "Day",
          "retainedFileCountLimit": 30,
          "outputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level:u3}] {Message:lj}{NewLine}{Exception}"
        }
      }
    ],
    "Enrich": ["FromLogContext", "WithThreadId", "WithMachineName"]
  }
}
```

**성능 고려사항:**

```csharp
// ✅ 좋은 예: 조건부 로깅
if (Log.IsEnabled(LogEventLevel.Debug))
{
    var expensiveData = ComputeExpensiveDebugInfo();
    Log.Debug("디버그 데이터: {@Data}", expensiveData);
}

// ✅ 좋은 예: 비동기 Sink (성능 중요 시나리오)
Log.Logger = new LoggerConfiguration()
    .WriteTo.Async(a => a.File("logs/app.txt"))  // 비동기 파일 쓰기
    .CreateLogger();

// ❌ 나쁜 예: 불필요한 객체 생성
Log.Information($"사용자 {user.Name} 로그인");  // 문자열 보간 피하기
```

**로깅 모범 사례:**

1. **로그 레벨을 적절히 사용**:
   - Verbose: 상세한 추적 (개발만)
   - Debug: 디버깅 정보 (개발/테스트)
   - Information: 정상 흐름의 중요 이벤트
   - Warning: 비정상이지만 처리 가능
   - Error: 오류 발생, 기능 실패
   - Fatal: 애플리케이션 종료 필요

2. **구조화된 속성 사용**: 문자열 보간 대신 템플릿
3. **민감 정보 로깅 금지**: 비밀번호, 신용카드 등
4. **일관된 네이밍**: 속성 이름을 PascalCase로 통일
5. **컨텍스트 정보 활용**: RequestId, UserId 등 추적 가능하게

### 23.3.3 Dapper - Micro-ORM의 성능 모범사례

**Dapper**는 Stack Overflow 팀이 2011년 개발한 Micro-ORM(Object-Relational Mapper)으로, "King of Micro ORMs"라는 별명을 가지고 있습니다. 2억 회 이상 다운로드되었으며, Stack Overflow의 초당 수천 건의 쿼리를 처리하는 실전 검증된 라이브러리입니다. Dapper는 ADO.NET의 `IDbConnection`에 대한 확장 메서드 집합으로, 원시 SQL의 성능을 유지하면서도 객체 매핑의 편의성을 제공하는 완벽한 균형을 달성했습니다.

**Dapper의 탄생 배경:**

Stack Overflow는 초기에 LINQ to SQL을 사용했으나, 복잡한 쿼리의 성능 문제와 생성된 SQL의 예측 불가능성으로 어려움을 겪었습니다. Entity Framework는 더 무거웠고, 순수 ADO.NET은 보일러플레이트 코드가 너무 많았습니다. Sam Saffron과 Marc Gravell은 이러한 간극을 메우기 위해 Dapper를 개발했으며, 다음 원칙을 추구했습니다:

- **성능 최우선**: ADO.NET에 가까운 속도 (오버헤드 < 5%)
- **단순성**: SQL 작성자가 완전한 제어권 유지
- **최소주의**: 핵심 기능만 포함, 작은 코드베이스
- **확장성**: 확장 메서드 패턴으로 기존 ADO.NET과 자연스러운 통합

**Dapper의 아키텍처와 설계:**

```
┌───────────────────────────────────────────────────────┐
│ 개발자 코드 (Developer Code)                            │
│ • SQL 쿼리 작성 (완전한 제어)                            │
│ • 익명 객체 또는 강타입 매개변수                          │
└───────────────────────────────────────────────────────┘
                        ↓
┌───────────────────────────────────────────────────────┐
│ Dapper 확장 메서드 (Extension Methods)                  │
│ • Query<T>, Execute, QueryFirst 등                    │
│ • IDbConnection 인터페이스 확장                         │
└───────────────────────────────────────────────────────┘
                        ↓
┌───────────────────────────────────────────────────────┐
│ 매핑 엔진 (Mapping Engine)                             │
│ • IL Emit을 통한 동적 코드 생성                          │
│ • 매핑 함수 캐싱 (성능 최적화)                           │
│ • 타입 핸들러 (사용자 정의 변환)                         │
└───────────────────────────────────────────────────────┘
                        ↓
┌───────────────────────────────────────────────────────┐
│ ADO.NET (System.Data)                                 │
│ • IDbConnection, IDbCommand                           │
│ • 데이터베이스 제공자 (SQL Server, SQLite, MySQL 등)    │
└───────────────────────────────────────────────────────┘
```

**설치:**

```bash
# Dapper 핵심 라이브러리
dotnet add package Dapper

# 데이터베이스 제공자 (예시들)
dotnet add package Microsoft.Data.Sqlite        # SQLite
dotnet add package Microsoft.Data.SqlClient     # SQL Server
dotnet add package MySql.Data                   # MySQL
dotnet add package Npgsql                       # PostgreSQL
```

**기본 CRUD 작업:**

```csharp
using Dapper;
using Microsoft.Data.Sqlite;
using System;
using System.Collections.Generic;
using System.Linq;

// 도메인 모델
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
    public int Stock { get; set; }
    public DateTime CreatedAt { get; set; }
}

// 데이터베이스 연결
string connectionString = "Data Source=shop.db";

using (var connection = new SqliteConnection(connectionString))
{
    connection.Open();
    
    // 테이블 생성 (DDL)
    connection.Execute(@"
        CREATE TABLE IF NOT EXISTS Products (
            Id INTEGER PRIMARY KEY AUTOINCREMENT,
            Name TEXT NOT NULL,
            Category TEXT NOT NULL,
            Price REAL NOT NULL,
            Stock INTEGER NOT NULL,
            CreatedAt TEXT NOT NULL
        )
    ");
    
    // CREATE - 단일 삽입
    var newProduct = new Product
    {
        Name = "게이밍 노트북",
        Category = "전자제품",
        Price = 2500000,
        Stock = 10,
        CreatedAt = DateTime.Now
    };
    
    int newId = connection.QuerySingle<int>(@"
        INSERT INTO Products (Name, Category, Price, Stock, CreatedAt)
        VALUES (@Name, @Category, @Price, @Stock, @CreatedAt);
        SELECT last_insert_rowid();",
        newProduct
    );
    
    Console.WriteLine($"제품 추가됨: ID={newId}");
    
    // CREATE - 대량 삽입 (배치)
    var productsToInsert = new[]
    {
        new Product { Name = "무선 마우스", Category = "주변기기", Price = 35000, Stock = 50, CreatedAt = DateTime.Now },
        new Product { Name = "기계식 키보드", Category = "주변기기", Price = 120000, Stock = 30, CreatedAt = DateTime.Now },
        new Product { Name = "모니터", Category = "전자제품", Price = 350000, Stock = 15, CreatedAt = DateTime.Now }
    };
    
    int affectedRows = connection.Execute(@"
        INSERT INTO Products (Name, Category, Price, Stock, CreatedAt)
        VALUES (@Name, @Category, @Price, @Stock, @CreatedAt)",
        productsToInsert  // 컬렉션 전달 시 자동으로 배치 실행
    );
    
    Console.WriteLine($"{affectedRows}개 제품 추가됨");
    
    // READ - 단일 객체 조회
    var product = connection.QueryFirstOrDefault<Product>(
        "SELECT * FROM Products WHERE Id = @Id",
        new { Id = newId }
    );
    
    if (product != null)
    {
        Console.WriteLine($"\n조회된 제품: {product.Name} - {product.Price:C}");
    }
    
    // READ - 여러 객체 조회
    var electronics = connection.Query<Product>(
        "SELECT * FROM Products WHERE Category = @Category ORDER BY Price DESC",
        new { Category = "전자제품" }
    ).ToList();
    
    Console.WriteLine($"\n전자제품 카테고리 ({electronics.Count}개):");
    foreach (var p in electronics)
    {
        Console.WriteLine($"  {p.Name}: {p.Price:C} (재고: {p.Stock})");
    }
    
    // UPDATE - 데이터 수정
    int updatedRows = connection.Execute(@"
        UPDATE Products 
        SET Price = @NewPrice, Stock = Stock + @StockChange
        WHERE Id = @Id",
        new { Id = newId, NewPrice = 2300000, StockChange = 5 }
    );
    
    Console.WriteLine($"\n{updatedRows}개 제품 업데이트됨");
    
    // DELETE - 데이터 삭제
    int deletedRows = connection.Execute(
        "DELETE FROM Products WHERE Stock = 0",
        null
    );
    
    Console.WriteLine($"{deletedRows}개 제품 삭제됨 (재고 0)");
    
    // 집계 쿼리
    var stats = connection.QueryFirst<(int Count, decimal TotalValue, decimal AvgPrice)>(@"
        SELECT 
            COUNT(*) as Count,
            SUM(Price * Stock) as TotalValue,
            AVG(Price) as AvgPrice
        FROM Products"
    );
    
    Console.WriteLine($"\n통계:");
    Console.WriteLine($"  총 제품 수: {stats.Count}");
    Console.WriteLine($"  재고 총 가치: {stats.TotalValue:C}");
    Console.WriteLine($"  평균 가격: {stats.AvgPrice:C}");
}
```

**고급 매핑 기법:**

```csharp
// 1. Multi-Mapping (JOIN 결과 매핑)
public class Order
{
    public int Id { get; set; }
    public DateTime OrderDate { get; set; }
    public Customer Customer { get; set; }  // 관계 객체
    public List<OrderItem> Items { get; set; }
}

public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class OrderItem
{
    public int Id { get; set; }
    public string ProductName { get; set; }
    public int Quantity { get; set; }
    public decimal Price { get; set; }
}

// 1:1 관계 매핑
var ordersWithCustomers = connection.Query<Order, Customer, Order>(@"
    SELECT 
        o.Id, o.OrderDate,
        c.Id, c.Name, c.Email
    FROM Orders o
    INNER JOIN Customers c ON o.CustomerId = c.Id",
    (order, customer) =>
    {
        order.Customer = customer;
        return order;
    },
    splitOn: "Id"  // Customer 시작 지점
).ToList();

// 1:N 관계 매핑 (One-to-Many)
var orderDictionary = new Dictionary<int, Order>();

var ordersWithItems = connection.Query<Order, OrderItem, Order>(@"
    SELECT 
        o.Id, o.OrderDate,
        oi.Id, oi.ProductName, oi.Quantity, oi.Price
    FROM Orders o
    LEFT JOIN OrderItems oi ON o.Id = oi.OrderId
    ORDER BY o.Id",
    (order, orderItem) =>
    {
        if (!orderDictionary.TryGetValue(order.Id, out var orderEntry))
        {
            orderEntry = order;
            orderEntry.Items = new List<OrderItem>();
            orderDictionary.Add(orderEntry.Id, orderEntry);
        }
        
        if (orderItem != null)
        {
            orderEntry.Items.Add(orderItem);
        }
        
        return orderEntry;
    },
    splitOn: "Id"
);

var result = orderDictionary.Values.ToList();
```

**동적 쿼리와 파라미터:**

```csharp
// DynamicParameters - 복잡한 파라미터 구성
var parameters = new DynamicParameters();
parameters.Add("@MinPrice", 100000);
parameters.Add("@MaxPrice", 500000);
parameters.Add("@Category", "전자제품");

// 출력 파라미터
parameters.Add("@TotalCount", dbType: DbType.Int32, direction: ParameterDirection.Output);

connection.Execute(@"
    SELECT @TotalCount = COUNT(*)
    FROM Products
    WHERE Price BETWEEN @MinPrice AND @MaxPrice
    AND Category = @Category",
    parameters
);

int totalCount = parameters.Get<int>("@TotalCount");
Console.WriteLine($"조건에 맞는 제품 수: {totalCount}");

// 동적 SQL 구성 (주의: SQL Injection 방지 필요)
var searchCriteria = new List<string>();
var queryParams = new DynamicParameters();

if (!string.IsNullOrEmpty(categoryFilter))
{
    searchCriteria.Add("Category = @Category");
    queryParams.Add("@Category", categoryFilter);
}

if (minPrice.HasValue)
{
    searchCriteria.Add("Price >= @MinPrice");
    queryParams.Add("@MinPrice", minPrice.Value);
}

string whereClause = searchCriteria.Any() 
    ? "WHERE " + string.Join(" AND ", searchCriteria)
    : "";

var products = connection.Query<Product>(
    $"SELECT * FROM Products {whereClause}",
    queryParams
).ToList();
```

**트랜잭션 처리:**

```csharp
using (var connection = new SqliteConnection(connectionString))
{
    connection.Open();
    
    using (var transaction = connection.BeginTransaction())
    {
        try
        {
            // 여러 작업을 원자적으로 실행
            connection.Execute(
                "UPDATE Products SET Stock = Stock - @Quantity WHERE Id = @ProductId",
                new { ProductId = 1, Quantity = 5 },
                transaction
            );
            
            connection.Execute(
                "INSERT INTO OrderItems (OrderId, ProductId, Quantity) VALUES (@OrderId, @ProductId, @Quantity)",
                new { OrderId = 100, ProductId = 1, Quantity = 5 },
                transaction
            );
            
            // 모두 성공 시 커밋
            transaction.Commit();
            Console.WriteLine("트랜잭션 완료");
        }
        catch (Exception ex)
        {
            // 오류 발생 시 롤백
            transaction.Rollback();
            Console.WriteLine($"트랜잭션 롤백: {ex.Message}");
            throw;
        }
    }
}
```

**stored procedure 호출:**

```csharp
// 저장 프로시저 실행
var result = connection.Query<Product>(
    "sp_GetProductsByCategory",
    new { Category = "전자제품", MinPrice = 100000 },
    commandType: CommandType.StoredProcedure
).ToList();

// 출력 파라미터가 있는 저장 프로시저
var parameters = new DynamicParameters();
parameters.Add("@ProductId", 1);
parameters.Add("@NewStock", dbType: DbType.Int32, direction: ParameterDirection.Output);

connection.Execute(
    "sp_UpdateProductStock",
    parameters,
    commandType: CommandType.StoredProcedure
);

int newStock = parameters.Get<int>("@NewStock");
```

**비동기 작업:**

```csharp
// 비동기 메서드 사용 (ASP.NET Core 권장)
using (var connection = new SqliteConnection(connectionString))
{
    await connection.OpenAsync();
    
    // 비동기 쿼리
    var products = await connection.QueryAsync<Product>(
        "SELECT * FROM Products WHERE Category = @Category",
        new { Category = "전자제품" }
    );
    
    // 비동기 단일 객체 조회
    var product = await connection.QueryFirstOrDefaultAsync<Product>(
        "SELECT * FROM Products WHERE Id = @Id",
        new { Id = 1 }
    );
    
    // 비동기 실행
    await connection.ExecuteAsync(
        "UPDATE Products SET Stock = Stock + @Amount WHERE Id = @Id",
        new { Id = 1, Amount = 10 }
    );
}
```

**타입 핸들러 - 사용자 정의 변환:**

```csharp
// JSON 컬럼을 C# 객체로 자동 변환
public class JsonTypeHandler<T> : SqlMapper.TypeHandler<T>
{
    public override void SetValue(IDbDataParameter parameter, T value)
    {
        parameter.Value = JsonSerializer.Serialize(value);
    }

    public override T Parse(object value)
    {
        return JsonSerializer.Deserialize<T>((string)value);
    }
}

// 등록
SqlMapper.AddTypeHandler(new JsonTypeHandler<Dictionary<string, string>>());

// 사용
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public Dictionary<string, string> Metadata { get; set; }  // JSON 컬럼
}

var product = connection.QueryFirst<Product>("SELECT * FROM Products WHERE Id = 1");
// Metadata가 자동으로 Dictionary로 변환됨
```

**성능 벤치마크 (Stack Overflow 데이터):**

```
작업: 500개 행 조회 및 매핑
환경: SQL Server 2019, .NET 6

순수 ADO.NET (DataReader):    47ms (기준선)
Dapper:                        49ms (+4% 오버헤드)
Entity Framework Core:         72ms (+53% 오버헤드)
LINQ to SQL:                   95ms (+102% 오버헤드)

작업: 1000개 행 삽입
순수 ADO.NET:                  52ms
Dapper (배치):                 54ms
Entity Framework Core:         124ms
```

**Dapper vs Entity Framework Core:**

| 특성 | Dapper | Entity Framework Core |
|------|--------|----------------------|
| 성능 | 매우 빠름 (ADO.NET 근접) | 보통 (오버헤드 존재) |
| 메모리 | 낮음 | 높음 (변경 추적) |
| SQL 제어 | 완전한 제어 | 제한적 (LINQ 변환) |
| 학습 곡선 | 낮음 (SQL 지식 필요) | 높음 |
| 변경 추적 | 없음 (수동 관리) | 자동 |
| 마이그레이션 | 없음 (수동) | 자동 코드 우선 |
| 관계 매핑 | 수동 (Multi-Mapping) | 자동 (Navigation Properties) |
| 복잡한 쿼리 | 쉬움 (직접 SQL) | 어려움 (LINQ 제한) |
| 사용 사례 | 읽기 중심, 성능 중요 | CRUD 중심, 생산성 중요 |

**언제 Dapper를 선택할까?**

✅ **Dapper 권장:**
- 읽기 작업이 많은 경우 (조회 중심)
- 성능이 매우 중요한 경우
- 복잡한 SQL 쿼리가 필요한 경우
- 레거시 데이터베이스와 작업
- 마이크로서비스 (가벼운 ORM 선호)

✅ **Entity Framework 권장:**
- CRUD 위주의 비즈니스 애플리케이션
- 코드 우선(Code First) 개발
- 복잡한 도메인 모델과 관계
- 빠른 프로토타이핑
- 팀원이 SQL에 익숙하지 않은 경우

**실무 모범 사례:**

```csharp
// 1. 연결 재사용 (using 패턴)
using (var connection = new SqliteConnection(connectionString))
{
    // 여러 작업 수행
}

// 2. 파라미터화된 쿼리 (SQL Injection 방지)
// ✅ 좋은 예
connection.Query<Product>("SELECT * FROM Products WHERE Name = @Name", new { Name = userInput });

// ❌ 나쁜 예 - SQL Injection 취약
connection.Query<Product>($"SELECT * FROM Products WHERE Name = '{userInput}'");

// 3. 대량 작업은 배치로
var items = GetLargeDataSet();
connection.Execute("INSERT INTO ...", items);  // Dapper가 자동으로 배치 처리

// 4. 필요한 컬럼만 조회
// ✅ 효율적
connection.Query<ProductSummary>("SELECT Id, Name, Price FROM Products");

// ❌ 비효율적
connection.Query<Product>("SELECT * FROM Products");  // 불필요한 컬럼 포함

// 5. Repository 패턴과 통합
public interface IProductRepository
{
    Task<Product> GetByIdAsync(int id);
    Task<IEnumerable<Product>> GetAllAsync();
    Task<int> AddAsync(Product product);
    Task<bool> UpdateAsync(Product product);
    Task<bool> DeleteAsync(int id);
}

public class DapperProductRepository : IProductRepository
{
    private readonly string _connectionString;
    
    public DapperProductRepository(string connectionString)
    {
        _connectionString = connectionString;
    }
    
    public async Task<Product> GetByIdAsync(int id)
    {
        using var connection = new SqliteConnection(_connectionString);
        return await connection.QueryFirstOrDefaultAsync<Product>(
            "SELECT * FROM Products WHERE Id = @Id",
            new { Id = id }
        );
    }
    
    // 다른 메서드 구현...
}
```

**Dapper 확장 라이브러리:**

```bash
# Dapper.Contrib - CRUD 헬퍼
dotnet add package Dapper.Contrib

# SimpleCRUD
dotnet add package Dapper.SimpleCRUD

# DapperExtensions - 람다 식 쿼리
dotnet add package DapperExtensions
```

```csharp
using Dapper.Contrib.Extensions;

[Table("Products")]  // 테이블 이름 명시
public class Product
{
    [Key]  // 기본 키
    public int Id { get; set; }
    
    public string Name { get; set; }
    
    [Computed]  // 계산된 속성 (DB에 저장 안 함)
    public string DisplayName => $"{Name} (ID: {Id})";
}

// CRUD 작업이 간단해짐
var product = new Product { Name = "노트북" };
int id = (int)connection.Insert(product);  // INSERT
var retrieved = connection.Get<Product>(id);  // SELECT
connection.Update(product);  // UPDATE
connection.Delete(product);  // DELETE
var all = connection.GetAll<Product>();  // SELECT all
```

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

## 23장 정리 및 요약

이 장에서는 NuGet 패키지 관리자를 통해 .NET 생태계의 풍부한 라이브러리를 활용하는 방법을 체계적으로 학습했습니다. NuGet의 역사적 배경부터 의존성 해석 알고리즘, 실무 필수 패키지까지 현대 .NET 개발의 핵심 인프라를 포괄적으로 다루었습니다.

### 핵심 개념의 재조명

**1. NuGet의 아키텍처와 설계 철학:**

2010년 Microsoft Outercurve Foundation 프로젝트로 시작된 NuGet은 .NET 생태계에 **선언적 의존성 관리(Declarative Dependency Management)**를 도입하여 DLL Hell 문제를 근본적으로 해결했습니다. 그래프 이론 기반의 의존성 해석 알고리즘은 복잡한 전이적 의존성 트리를 자동으로 해결하며, 시맨틱 버저닝(Semantic Versioning 2.0.0)을 통해 호환성을 보장합니다.

**2. 의존성 해석과 버전 관리:**

NuGet의 의존성 해석은 방향성 비순환 그래프(DAG)를 구성하고 위상 정렬(Topological Sort)로 설치 순서를 결정합니다. 버전 충돌은 최소 적용 가능 버전(Minimal Applicable Version) 전략으로 해결하며, `project.assets.json`에 해석 결과를 캐싱하여 빌드 성능을 최적화합니다.

**3. 패키지 생태계와 커뮤니티:**

50만 개 이상의 패키지, 일일 1억 건 이상의 다운로드를 기록하는 NuGet Gallery는 .NET 오픈소스 문화의 중심입니다. Newtonsoft.Json의 50억 다운로드, Serilog의 3억 다운로드, Dapper의 2억 다운로드는 커뮤니티의 신뢰와 검증을 보여줍니다.

### 주요 패키지의 핵심 가치

**Newtonsoft.Json - JSON 직렬화의 황금 표준:**

- **역사**: James Newton-King의 2006년 프로젝트, .NET JSON 라이브러리의 선구자
- **아키텍처**: 계층적 설계 (고수준 API → 직렬화 엔진 → 타입 변환 → 저수준 파서)
- **강점**: LINQ to JSON, 커스텀 JsonConverter, 방대한 설정 옵션
- **System.Text.Json 비교**: 기능의 풍부함 vs 성능 (2-3배 차이)

**Serilog - 구조화된 로깅의 혁명:**

- **혁신**: 문자열 기반 로깅 → 구조화된 데이터 (검색/분석 가능)
- **아키텍처**: Logger API → Configuration → Pipeline (Enrichers, Filters) → Sinks
- **생태계**: Console, File, Elasticsearch, Seq, Application Insights 등 다양한 Sink
- **실무 가치**: 분산 시스템에서 RequestId, UserId 등으로 추적 가능

**Dapper - Micro-ORM의 성능 모범사례:**

- **탄생**: Stack Overflow 팀의 2011년 프로젝트, 초당 수천 건 쿼리 처리
- **설계 철학**: 성능 (ADO.NET +4% 오버헤드), 단순성 (SQL 제어), 최소주의
- **IL Emit**: 동적 코드 생성과 매핑 함수 캐싱으로 최고 성능 달성
- **Entity Framework 대비**: 읽기 중심, 성능 우선 시나리오에서 압도적 우위

### 실무 패키지 관리 전략

**1. 보안과 라이선스 준수:**

```bash
# 정기적인 보안 취약점 검사
dotnet list package --vulnerable

# 라이선스 확인
dotnet list package --include-transitive
```

**2. 중앙 패키지 관리 (Directory.Packages.props):**

솔루션 단위로 패키지 버전을 통일하여 일관성 유지

**3. Private NuGet Feed:**

기업 내부 패키지는 Azure Artifacts, GitHub Packages 등 활용

**4. CI/CD 최적화:**

```yaml
- run: dotnet restore
- run: dotnet build --no-restore
- run: dotnet test --no-restore
```

`--no-restore` 플래그로 중복 복원 방지

### 다음 단계와 심화 학습

**고급 주제:**

- **패키지 제작과 배포**: `.nuspec` 파일 작성, NuGet Gallery 게시
- **Symbol Packages**: 디버깅을 위한 소스 코드 및 PDB 파일 패키징
- **SourceLink 통합**: GitHub 소스 코드로 직접 디버깅
- **Multi-Targeting**: 여러 .NET 버전 지원하는 패키지 제작
- **NuGet Server 구축**: BaGet, NuGet.Server 등으로 자체 서버 운영

**권장 리소스:**

- **공식 문서**:
  - NuGet 공식 문서: https://docs.microsoft.com/nuget
  - Newtonsoft.Json: https://www.newtonsoft.com/json
  - Serilog: https://serilog.net
  - Dapper: https://github.com/DapperLib/Dapper

- **도서**:
  - "Pro NuGet" by Maarten Balliauw & Xavier Decoster
  - "C# 9.0 in a Nutshell" (Newtonsoft.Json 섹션)
  - "Logging in .NET" by Serilog 커뮤니티

- **커뮤니티**:
  - NuGet GitHub: https://github.com/NuGet/Home
  - Stack Overflow NuGet 태그
  - .NET Foundation 프로젝트

**실습 과제:**

**초급: 패키지 활용 기초**
1. 콘솔 앱을 만들고 Newtonsoft.Json으로 복잡한 중첩 JSON을 파싱하기
2. Serilog로 다중 Sink(Console + File + Seq) 구성하기
3. Dapper로 SQLite 데이터베이스에 CRUD 작업 구현하기

**중급: 통합 애플리케이션**
4. REST API 클라이언트를 만들어 공개 API 호출 결과를 Newtonsoft.Json으로 파싱하고 Serilog로 로깅하기
5. Dapper로 다대다(Many-to-Many) 관계를 Multi-Mapping으로 조회하기
6. Serilog의 LogContext로 HTTP 요청마다 RequestId 자동 추가하기

**고급: 프로덕션 수준**
7. ASP.NET Core 웹 API를 만들어 세 패키지 모두 통합하고 Docker 컨테이너로 배포하기
8. 자신만의 NuGet 패키지 제작하여 NuGet Gallery에 게시하기
9. Private NuGet Feed를 구축하고 CI/CD 파이프라인 구성하기

**벤치마크 과제:**
10. Newtonsoft.Json vs System.Text.Json 성능 비교 (BenchmarkDotNet 사용)
11. Dapper vs Entity Framework Core 쿼리 성능 측정
12. Serilog의 비동기 Sink 성능 영향 분석

### 마무리

NuGet 패키지 관리는 현대 .NET 개발의 기반입니다. 이 장에서 학습한 내용은:

✅ **이론적 기반**: 패키지 관리자의 역사, 의존성 해석 알고리즘, 시맨틱 버저닝
✅ **실무 기술**: .NET CLI 워크플로우, 버전 관리 전략, 보안 취약점 검사
✅ **핵심 패키지**: Newtonsoft.Json, Serilog, Dapper의 깊이 있는 이해
✅ **모범 사례**: 중앙 패키지 관리, Private Feed, CI/CD 통합

24장에서는 xUnit과 Moq를 사용한 단위 테스트를 다루며, 코드 품질과 신뢰성을 높이는 테스트 주도 개발(TDD)의 실천 방법을 학습합니다. 패키지 관리, 로깅, 데이터 접근과 함께 테스트는 전문 개발자의 필수 역량입니다!
