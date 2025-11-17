# C# 프로그래밍 실습

Visual Studio Code와 .NET 10 File-based App을 활용한 C# 프로그래밍 학습서

!!! info "AI 콘텐츠 생성에 대한 안내"
    이 학습서의 콘텐츠는 AI(인공지능)를 활용하여 생성되었습니다.  
    내용의 정확성을 위해 최선을 다하고 있으나, 오류나 개선이 필요한 부분이 있을 수 있습니다.  
    피드백이나 수정 제안은 [GitHub Issues](https://github.com/rkttu/csharp-book/issues)를 통해 제보해 주세요.

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

## 주요 내용

### 제1부: 시작하기
1. **개발 환경 구축**: Visual Studio Code 설치, .NET 10 SDK 설치 및 확인
2. **C# 기초 문법**: 변수, 데이터 타입, 연산자, 콘솔 입출력
3. **제어문**: 조건문, 반복문, 분기문

### 제2부: 데이터 구조와 컬렉션
4. **배열**: 1차원 배열, 다차원 배열, 배열 관련 메서드
5. **컬렉션**: List, Dictionary, HashSet, Queue, Stack
6. **문자열 처리**: 문자열 연산, 메서드, StringBuilder

### 제3부: 함수와 메서드
7. **메서드 기초**: 메서드 정의, 매개변수, 반환값, 오버로딩
8. **고급 메서드 활용**: 람다 식, 확장 메서드, 재귀 함수

### 제4부: 객체지향 프로그래밍
9. **클래스와 객체**: 클래스 정의, 객체 생성, 접근 제한자, 정적 멤버
10. **상속과 다형성**: 상속, 추상 클래스, 인터페이스, 다형성
11. **고급 객체지향 개념**: sealed, record 타입, 구조체, 열거형

### 제5부: 예외 처리와 파일 I/O
12. **예외 처리**: try-catch-finally, 예외 던지기, 사용자 정의 예외
13. **파일 입출력**: 텍스트 파일 읽기/쓰기, 파일 관리, JSON 처리

### 제6부: LINQ와 함수형 프로그래밍
14. **LINQ 기초**: 쿼리 구문, 메서드 구문, 기본 연산자
15. **고급 LINQ**: 조인, 그룹화, 집합 연산자
16. **함수형 프로그래밍 개념**: 불변성, 순수 함수, 고차 함수

### 제7부: 비동기 프로그래밍
17. **비동기 프로그래밍 기초**: async/await, Task
18. **고급 비동기 패턴**: Task.WhenAll, CancellationToken, IAsyncEnumerable

### 제8부: 실전 프로젝트
19. **콘솔 계산기 만들기**
20. **할 일 관리 앱**
21. **간단한 텍스트 기반 게임**
22. **REST API 클라이언트**

### 제9부: .NET 생태계와 도구
23. **NuGet 패키지 관리**
24. **단위 테스트**: xUnit, Moq
25. **디버깅과 로깅**

### 제10부: 모던 C# 기능
26. **C# 10-14 주요 기능**: Global using, File-scoped 네임스페이스, Record struct, Raw String Literals, Primary Constructors, Collection Expressions 등
27. **패턴 매칭**: 타입 패턴, 속성 패턴, 관계 패턴
28. **Nullable 참조 타입**
29. **성능과 메모리 최적화**: Span&lt;T&gt;, ValueTask, 구조체 최적화

## 필요한 준비물

- Windows, macOS, 또는 Linux 운영체제
- Visual Studio Code (최신 버전)
- .NET 10 SDK
- C# 확장 프로그램 (VS Code)

## 시작하기

```bash
# .NET 10 설치 확인
dotnet --version

# 첫 번째 C# 프로그램 만들기
echo 'Console.WriteLine("Hello, C#!");' > Program.cs
dotnet run
```

## 피드백 및 기여

이 학습서에 대한 피드백, 오류 수정, 개선 제안은 언제나 환영합니다!

- **이슈 등록**: [GitHub Issues](https://github.com/rkttu/csharp-book/issues)에서 문제를 보고하거나 제안을 남겨주세요.
- **질문 및 토론**: GitHub Issues를 통해 질문하시면 커뮤니티가 도움을 드립니다.

## 라이선스

이 프로젝트의 라이선스 정보는 추후 추가될 예정입니다.

---

**© 2024 C# 프로그래밍 실습** | AI 생성 콘텐츠 | [GitHub Repository](https://github.com/rkttu/csharp-book)
