# 메모장으로 코딩하는 C\#

Visual Studio Code와 .NET 10 File-based App을 활용한 C# 프로그래밍 학습서

> **📢 AI 콘텐츠 생성에 대한 안내**  
> 이 학습서의 콘텐츠는 AI(인공지능)를 활용하여 생성되었습니다.  
> 내용의 정확성을 위해 최선을 다하고 있으나, 오류나 개선이 필요한 부분이 있을 수 있습니다.  
> 피드백이나 수정 제안은 [GitHub Issues](https://github.com/rkttu/csharp-book/issues)를 통해 제보해 주세요.

## 🌐 온라인 웹사이트

이 학습서는 GitHub Pages를 통해 온라인 웹사이트로 제공됩니다:

**👉 [https://rkttu.github.io/csharp-book/](https://rkttu.github.io/csharp-book/)**

## 소개

이 책은 Visual Studio Code와 .NET 10의 새로운 File-based App 기능만을 사용하여 C# 프로그래밍을 처음부터 배우고 실습할 수 있도록 구성된 학습서입니다. 복잡한 프로젝트 구조 없이 간단한 `.cs` 파일 하나로 시작하여 점진적으로 C#의 강력한 기능들을 익혀나갈 수 있습니다.

## 주요 특징

- ✅ Visual Studio Code 기반의 가벼운 개발 환경
- ✅ .NET 10의 최신 File-based App 구조 활용
- ✅ 복잡한 프로젝트 설정 없이 바로 시작
- ✅ 기초부터 실전까지 단계별 학습
- ✅ 풍부한 실습 예제와 프로젝트

## 대상 독자

- C# 프로그래밍을 처음 시작하는 입문자
- 다른 언어 경험은 있지만 C#은 처음인 개발자
- 간단하고 빠르게 C#을 배우고 싶은 학습자
- Visual Studio Code를 선호하는 개발자

## 목차

전체 목차는 [docs/index.md](./docs/index.md) 파일을 참고하세요.

### 주요 내용

1. **시작하기**: 개발 환경 구축, C# 기초 문법, 제어문
2. **데이터 구조**: 배열, 컬렉션, 문자열 처리
3. **함수와 메서드**: 메서드 기초, 람다 식, 확장 메서드
4. **객체지향**: 클래스, 상속, 인터페이스, record 타입
5. **예외 처리와 파일 I/O**: 예외 처리, 파일 입출력, JSON
6. **LINQ**: LINQ 기초와 고급, 함수형 프로그래밍
7. **비동기 프로그래밍**: async/await, Task, 비동기 패턴
8. **실전 프로젝트**: 계산기, 할 일 관리, 게임, API 클라이언트
9. **.NET 생태계**: NuGet, 단위 테스트, 디버깅, 로깅
10. **모던 C#**: C# 10-14 기능, 패턴 매칭, Nullable 참조 타입, 최신 언어 기능

## 필요한 준비물

- .NET 10 SDK
- Visual Studio Code (최신 버전) 또는 호환 에디터 (VS Codium, Cursor 등)
- Linux 또는 macOS 운영체제 (Windows에서도 사용 가능)
- C# 확장 프로그램 (VS Code)

## 시작하기

### C# 프로그래밍 시작하기

```bash
# .NET 10 설치 확인
dotnet --version

# 첫 번째 C# 프로그램 만들기
echo 'Console.WriteLine("Hello, C#!");' > Program.cs
dotnet run
```

### 로컬에서 웹사이트 빌드하기

이 리포지터리는 mkdocs-material을 사용하여 정적 웹사이트를 생성합니다.

```bash
# Python 3.12 이상 필요
python3 --version

# 의존성 설치
pip install -r requirements.txt

# 로컬 개발 서버 실행 (http://127.0.0.1:8000)
mkdocs serve

# 정적 사이트 빌드
mkdocs build
```

## 프로젝트 구조

```text
csharp-book/
├── docs/                    # 문서 소스 파일 (Markdown)
│   ├── index.md            # 홈페이지
│   ├── 01-개발환경구축.md   # 각 장의 내용
│   ├── ...
│   └── stylesheets/        # 커스텀 CSS
├── .github/
│   └── workflows/          # GitHub Actions 워크플로우
├── mkdocs.yml              # MkDocs 설정 파일
├── requirements.txt        # Python 의존성
└── README.md               # 이 파일
```

## 라이선스

이 프로젝트의 라이선스 정보는 추후 추가될 예정입니다.

## 기여하기

이 책에 대한 피드백, 오류 수정, 개선 제안은 언제나 환영합니다.

- **이슈 등록**: [GitHub Issues](https://github.com/rkttu/csharp-book/issues)에서 문제를 보고하거나 제안을 남겨주세요.
- **질문 및 토론**: GitHub Issues를 통해 질문하시면 커뮤니티가 도움을 드립니다.

이 프로젝트의 콘텐츠는 AI를 활용하여 생성되었으며, 지속적으로 개선하고 있습니다.
