# 부록 E. 라이선스 및 비용 고려사항

C# 및 .NET 개발 환경을 구축할 때 주의해야 할 라이선스 및 비용 관련 사항들을 정리합니다. 오픈소스와 상용 소프트웨어가 혼재되어 있어 착오하기 쉬운 부분들을 명확히 이해하는 것이 중요합니다.

## E.1 .NET 및 C# 언어

### .NET SDK 및 Runtime

- **라이선스**: MIT License (완전 오픈소스)
- **비용**: 무료
- **용도 제한**: 없음 (개인, 상업, 기업 모두 자유롭게 사용 가능)
- **공식 저장소**: <https://github.com/dotnet/runtime>

.NET 자체는 완전한 오픈소스 프로젝트이며, 어떤 목적으로든 자유롭게 사용할 수 있습니다.

### C# 언어

- **라이선스**: MIT License
- **비용**: 무료
- **용도 제한**: 없음

C# 언어 명세와 컴파일러(Roslyn)는 모두 오픈소스입니다.

## E.2 Visual Studio Code 및 확장 프로그램

### Visual Studio Code

- **라이선스**: MIT License
- **비용**: 무료
- **용도 제한**: 없음
- **공식 사이트**: <https://code.visualstudio.com/>

VS Code는 완전한 오픈소스 프로젝트이며 상업적 용도를 포함하여 자유롭게 사용할 수 있습니다.

### C# 확장 프로그램

- **라이선스**: MIT License  
- **비용**: 무료
- **용도 제한**: 없음
- **공식 저장소**: <https://github.com/dotnet/vscode-csharp>

이 책에서 사용하는 기본 C# 확장 프로그램은 오픈소스이며 무료로 사용할 수 있습니다.

### ⚠️ C# Dev Kit의 경우 (본 책에서는 다루지 않음)

**중요**: C# Dev Kit는 별도의 상용 라이선스가 필요한 제품입니다.

- **라이선스**: Microsoft 소프트웨어 라이선스 조항
- **비용**: Visual Studio 라이선스 필요
- **제한사항**:
  - 개인 학습 및 오픈소스 프로젝트: 무료 사용 가능
  - **상업적 개발 목적**: Visual Studio 라이선스 또는 구독 필요
  - 기업에서 사용하는 경우 라이선스 준수 여부 확인 필수

**상업용 개발을 위해 C# Dev Kit을 사용하려는 경우**, 반드시 조직이 적절한 Visual Studio 라이선스를 보유하고 있는지 확인해야 합니다. 라이선스 없이 상업적 목적으로 사용하면 법적 문제가 발생할 수 있습니다.

**대안**: 본 책에서 소개하는 기본 C# 확장 프로그램은 완전히 무료이며 상업적 용도에도 제한이 없습니다.

## E.3 Visual Studio IDE

### Visual Studio Community

- **라이선스**: Microsoft 소프트웨어 라이선스 조항
- **비용 및 제한**:
  - ✅ **무료 사용 가능한 경우**:
    - 개인 개발자
    - 학생
    - 오픈소스 프로젝트 기여자
    - 비영리 조직
    - 5명 이하의 소규모 조직
  - ⚠️ **유료 라이선스 필요한 경우**:
    - PC 250대 이상 또는 연 매출 100만 달러 이상의 기업
    - 기업 환경에서 5명 이상이 사용하는 경우

### Visual Studio Professional / Enterprise

- **라이선스**: 상용 라이선스
- **비용**: 구독 방식 (월간/연간)
- **용도**: 전문 개발자 및 기업용
- **주요 차이점**: 
  - Professional: 개인 및 소규모 팀용 전문 도구
  - Enterprise: 대규모 팀 및 엔터프라이즈급 기능

**중요**: Visual Studio Community를 기업 환경에서 사용하려는 경우, 조직의 규모와 매출을 확인하여 라이선스 조건을 충족하는지 반드시 확인해야 합니다.

## E.4 컨테이너 및 가상화 도구

### Docker Desktop

- **라이선스**: Docker Subscription Service Agreement
- **비용 및 제한**:
  - ✅ **무료 사용 가능한 경우**:
    - 개인 사용
    - 교육 목적
    - 비상업적 오픈소스 프로젝트
    - 직원 250명 미만이고 연 매출 1천만 달러 미만인 소규모 기업
  - ⚠️ **유료 구독 필요한 경우**:
    - 직원 250명 이상 또는 연 매출 1천만 달러 이상의 기업
    - 상업적 용도로 사용하는 중대형 조직

**중요**: macOS 및 Windows에서 Docker Desktop을 기업 환경에서 사용하려는 경우, 조직의 규모를 확인하여 라이선스 조건을 충족하는지 반드시 확인해야 합니다.

### ✅ Podman Desktop (무료 대안)

- **라이선스**: Apache License 2.0
- **비용**: 완전 무료
- **용도 제한**: 없음 (개인, 상업, 기업 모두 자유롭게 사용 가능)
- **공식 사이트**: <https://podman-desktop.io/>
- **특징**:
  - Docker Desktop의 오픈소스 대안
  - Docker 명령어와 호환되는 인터페이스
  - macOS, Windows, Linux 모두 지원
  - 컨테이너 및 Kubernetes Pod 관리

**권장사항**: macOS 또는 Windows에서 리눅스 개발 환경을 구축하고자 하는 경우, 특히 기업 환경에서는 라이선스 제약이 없는 Podman Desktop 사용을 권장합니다.

#### Podman Desktop 설치

**macOS (Homebrew):**
```bash
brew install podman-desktop
```

**Linux:**
- 배포판별 설치 방법은 공식 문서 참조: <https://podman-desktop.io/docs/installation>

**Windows:**
- 공식 웹사이트에서 설치 프로그램 다운로드: <https://podman-desktop.io/downloads>

## E.5 VS Code 호환 에디터

본 책에서 소개한 VS Code 호환 에디터들의 라이선스 정보입니다:

### VS Codium

- **홈페이지**: <https://vscodium.com>
- **라이선스**: MIT License
- **비용**: 무료
- **특징**: 완전 오픈소스 빌드, Microsoft 텔레메트리 제거

### Amazon Kiro

- **홈페이지**: <https://kiro.dev>
- **라이선스**: 상용
- **비용**: 무료 플랜 제공, 고급 기능은 유료

### Cursor

- **홈페이지**: <https://cursor.com>
- **라이선스**: 상용 (프리미엄 기능)
- **비용**: 무료 플랜 제공, 프리미엄 기능은 유료

### Windsurf

- **홈페이지**: <https://windsurf.com/editor>
- **라이선스**: 상용
- **비용**: 무료 플랜 제공, 고급 기능은 유료

### Eclipse Theia

- **홈페이지**: <https://theia-ide.org>
- **라이선스**: Eclipse Public License 2.0
- **비용**: 무료 (오픈소스)

### coder/code-server

- **홈페이지**: <https://github.com/coder/code-server>
- **라이선스**: MIT License
- **비용**: 무료 (오픈소스)

### Google Firebase Studio

- **홈페이지**: <https://firebase.studio>
- **라이선스**: 상용
- **비용**: IDE 및 NixOS 내부 환경 사용은 무료, 작업 가능한 워크스페이스 갯수를 늘리려면 결제 필요

### Podman Desktop에 포함된 에디터들

Podman Desktop과 함께 사용할 수 있는 다양한 오픈소스 에디터들도 있으니 필요에 따라 선택하시면 됩니다.

## E.6 기타 C# 개발 도구

C# 개발에 사용할 수 있는 다양한 도구들의 라이선스 정보입니다.

### JetBrains Rider

- **라이선스**: 상용 라이선스
- **비용**: 유료 구독 (월간/연간)
- **공식 사이트**: <https://www.jetbrains.com/rider/>
- **무료 사용 가능한 경우**:
  - 학생 및 교육 기관: 무료 교육용 라이선스
  - 오픈소스 프로젝트 개발자: 무료 라이선스 신청 가능
  - 30일 무료 평가판
- **특징**: 
  - 크로스 플랫폼 .NET IDE (Windows, macOS, Linux)
  - Visual Studio와 유사한 기능의 상용 IDE
  - 강력한 리팩토링 도구와 코드 분석 기능
  - Unity 게임 개발 지원

**중요**: 상업적 개발 목적으로 사용 시 유료 구독이 필요합니다.

### ReSharper for Visual Studio

- **라이선스**: 상용 라이선스
- **비용**: 유료 구독 (월간/연간)
- **공식 사이트**: <https://www.jetbrains.com/resharper/>
- **무료 사용 가능한 경우**:
  - 학생 및 교육 기관: 무료 교육용 라이선스
  - 오픈소스 프로젝트 개발자: 무료 라이선스 신청 가능
  - 30일 무료 평가판
- **특징**: 
  - Visual Studio용 확장 프로그램
  - 코드 품질 분석 및 리팩토링 도구
  - Windows 전용

**중요**: Visual Studio 라이선스와 별도로 ReSharper 라이선스가 필요합니다.

### ReSharper for VS Code Extension

- **라이선스**: 상용 라이선스
- **비용**: 2025년 11월 현재 프리뷰 버전으로 무료 제공, 추후 상용으로 전환 가능성이 있음
- **공식 사이트**: <https://marketplace.visualstudio.com/items?itemName=jetbrains.resharper-code>

### OmniSharp

- **라이선스**: MIT License
- **비용**: 완전 무료
- **용도 제한**: 없음
- **공식 저장소**: <https://github.com/OmniSharp>
- **특징**:
  - 오픈소스 .NET 개발 플랫폼
  - VS Code의 C# 확장 프로그램이 사용하는 언어 서버
  - 여러 에디터에서 사용 가능 (Vim, Emacs, Sublime Text 등)
  - 완전히 무료이며 상업적 용도 제한 없음

**권장**: VS Code 사용자는 기본 C# 확장 프로그램을 통해 자동으로 OmniSharp를 사용하게 됩니다.

### Unity Editor

- **라이선스**: Unity Software License
- **비용 및 제한**:
  - ✅ **무료 사용 가능한 경우** (Unity Personal):
    - 개인 개발자
    - 연 매출 10만 달러 미만인 기업 또는 단체
    - 교육 및 학습 목적
  - ⚠️ **유료 라이선스 필요한 경우**:
    - 연 매출 10만 달러 이상인 기업: Unity Plus, Pro, Enterprise 구독 필요
- **공식 사이트**: <https://unity.com/>
- **특징**:
  - 게임 엔진 및 통합 개발 환경
  - C# 스크립팅 지원 (Unity 전용 API)
  - 크로스 플랫폼 게임 개발

**중요**: Unity에서 C# 개발을 위해서는 Visual Studio Community (Unity와 함께 번들 제공) 또는 Rider를 사용할 수 있습니다. 기업 환경에서 Unity를 사용하는 경우 조직의 매출 규모에 따라 적절한 Unity 라이선스를 확인해야 합니다.

### LINQPad

- **라이선스**: 프리미엄 기능 포함 라이선스
- **비용**:
  - ✅ 무료 버전: 기본 기능 사용 가능
  - ⚠️ 유료 버전 (LINQPad Premium): 추가 기능 제공 (개인 라이선스)
- **공식 사이트**: <https://www.linqpad.net/>
- **특징**:
  - C# 코드 스니펫 실행 및 테스트 도구
  - LINQ 쿼리 학습 및 테스트에 최적화
  - 데이터베이스 연결 및 쿼리 실행
  - Windows 전용
  - 무료 버전으로도 충분히 학습 및 개발에 활용 가능

**권장**: C# 및 LINQ 학습, 빠른 코드 테스트에 유용합니다. 무료 버전으로도 대부분의 기능 사용 가능합니다.

### 도구 선택 가이드

**완전 무료 도구 (본 책 권장)**:
- Visual Studio Code + C# 확장
- OmniSharp (VS Code에 포함)
- LINQPad (무료 버전)

**학생/교육용**:
- JetBrains Rider (교육용 무료 라이선스)
- ReSharper (교육용 무료 라이선스)
- Unity Personal (학습 목적)

**상업용 개발**:
- JetBrains Rider (유료 구독)
- ReSharper (유료 구독, Visual Studio와 함께)
- Unity Plus/Pro/Enterprise (매출 규모에 따라)

## E.7 라이선스 준수를 위한 체크리스트

개발 환경 구축 시 다음 사항들을 확인하세요:

### 개인 개발자 / 학생 / 오픈소스 기여자

- ✅ 모든 도구를 자유롭게 사용 가능
- ✅ .NET, VS Code, C# 확장, C# Dev Kit 모두 무료
- ✅ Visual Studio Community 사용 가능
- ✅ Docker Desktop 무료 사용 가능

### 소규모 스타트업 (5명 미만)

- ✅ .NET, VS Code, C# 확장 모두 무료
- ⚠️ Visual Studio Community: 조직 규모 확인 필요
  - ⚠️ C# Dev Kit: 조직 규모 확인 필요
- ⚠️ Docker Desktop: 조직 규모 확인 필요 (250명 미만, 매출 1천만 달러 미만)
  - ✅ 대안: Podman Desktop 사용 권장

### 중대형 기업

- ✅ .NET, VS Code, C# 확장 모두 무료
- ⚠️ Visual Studio Community 사용 불가 → Professional/Enterprise 구독 필요
  - ⚠️ C# Dev Kit 사용 시 라이선스 확인 필요
- ⚠️ Docker Desktop 유료 구독 필요 → Podman Desktop 사용 권장
  - ✅ 대안: Podman Desktop 및 기본 C# 확장 사용

## E.8 라이선스 관련 추가 정보

### 공식 라이선스 문서

- **Visual Studio 라이선스**: <https://visualstudio.microsoft.com/license-terms/>
- **Docker Desktop 라이선스**: <https://www.docker.com/pricing/faq/>
- **.NET 라이선스**: <https://github.com/dotnet/runtime/blob/main/LICENSE.TXT>
- **VS Code 라이선스**: <https://github.com/microsoft/vscode/blob/main/LICENSE.txt>

### 라이선스 확인이 필요한 경우

- 조직의 IT 부서 또는 법무팀에 문의
- Microsoft 파트너 또는 리셀러에게 상담
- 각 제품의 공식 라이선스 FAQ 참조

### 라이선스 위반 방지

1. **사용 전 확인**: 도구 도입 전 라이선스 조건 검토
2. **정기적 검토**: 조직 규모 변화 시 라이선스 재검토
3. **문서화**: 사용 중인 도구와 라이선스 목록 관리
4. **오픈소스 우선**: 가능한 경우 오픈소스 대안 검토

## E.9 본 책의 권장 구성 (무료 & 제약 없음)

이 책에서 소개하는 다음 구성은 **모든 용도에서 완전히 무료**이며 **라이선스 제약이 없습니다**:

1. **.NET 10 SDK** - MIT License, 완전 무료
2. **Visual Studio Code** - MIT License, 완전 무료
3. **C# 확장 프로그램** - MIT License, 완전 무료
4. **Podman Desktop** (컨테이너 필요 시) - Apache 2.0, 완전 무료

이 구성은 개인, 스타트업, 대기업 모두에서 추가 비용이나 라이선스 제약 없이 사용할 수 있습니다.

---

**면책 조항**: 이 문서의 라이선스 정보는 작성 시점을 기준으로 하며, 참고용으로만 제공됩니다. 정확하고 최신의 라이선스 정보는 각 제품의 공식 라이선스 문서를 확인하시기 바랍니다. 상업적 용도로 사용하기 전에는 반드시 해당 조직의 법무팀 또는 전문가와 상담하시기 바랍니다.
