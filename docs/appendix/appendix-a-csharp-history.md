# 부록 A. C# 버전 히스토리

C#은 2000년 첫 출시 이후 지속적으로 발전해온 프로그래밍 언어입니다. 이 부록에서는 각 버전에서 추가된 주요 기능들을 간략히 정리합니다. C# 10-14의 최신 기능은 본문 26장에서 자세히 다루었으므로, 여기서는 요약만 제공합니다.

## A.1 C# 1.0 ~ 5.0

### C# 1.0 (2002) - 시작
- 기본 객체지향 기능
- 속성(Properties), 이벤트, 델리게이트

### C# 2.0 (2005) - 제네릭
- 제네릭 (`List<T>`, `Dictionary<TKey,TValue>`)
- Nullable 타입 (`int?`)
- 익명 메서드, 반복자 (yield)

### C# 3.0 (2007) - LINQ
- LINQ (Language Integrated Query)
- 람다 식 (`=>`)
- 익명 타입, 자동 속성, 확장 메서드

### C# 4.0 (2010) - 동적 프로그래밍
- `dynamic` 키워드
- 선택적 매개변수, 명명된 인수

### C# 5.0 (2012) - 비동기
- `async`/`await` 키워드
- `Task` 기반 비동기 패턴

## A.2 C# 6.0 ~ 9.0

### C# 6.0 (2015) - 문법 개선
- 문자열 보간 (`$"Hello {name}"`)
- null 조건 연산자 (`?.`, `?[]`)
- Expression-bodied 멤버
- 자동 속성 초기화
- `nameof` 연산자

### C# 7.0 (2017) - 튜플과 패턴
- 튜플 개선
- 패턴 매칭 기초
- 로컬 함수
- `out` 변수 인라인 선언
- 디스카드 (`_`)

### C# 8.0 (2019) - Nullable 참조 타입
- Nullable 참조 타입
- 비동기 스트림 (`IAsyncEnumerable<T>`)
- 인덱스와 범위 (`^`, `..`)
- switch 식 개선
- 기본 인터페이스 멤버

### C# 9.0 (2020) - Record 타입
- Record 타입
- Init-only 속성
- 최상위 문 (Top-level statements)
- 패턴 매칭 향상

## A.3 C# 10 (2021) - 간결성
*자세한 내용은 26장 참조*

- Global using 지시문
- File-scoped 네임스페이스
- Record struct
- 필수 속성 (required)

## A.4 C# 11 (2022) - 문자열과 제네릭
*자세한 내용은 26장 참조*

- Raw string literals
- Generic attributes
- List patterns
- Required members
- File-local types

## A.5 C# 12 (2023) - 간결성 극대화
*자세한 내용은 26장 참조*

- Primary constructors
- Collection expressions
- Default lambda parameters
- Alias any type

## A.6 C# 13 (2024) - 성능과 유연성
*자세한 내용은 26장 참조*

- params collections
- New lock type
- ref struct interfaces

## A.7 C# 14 (프리뷰) - 미래 기능
*자세한 내용은 26장 참조*

- Field keyword
- Extensions
- Discriminated unions (검토 중)

## A.8 버전별 주요 특징 요약

| 버전 | 년도 | 주요 특징 |
|------|------|----------|
| C# 1.0 | 2002 | 객체지향 기초 |
| C# 2.0 | 2005 | 제네릭 |
| C# 3.0 | 2007 | LINQ |
| C# 4.0 | 2010 | Dynamic |
| C# 5.0 | 2012 | Async/Await |
| C# 6.0 | 2015 | 문법 개선 |
| C# 7.0 | 2017 | 튜플, 패턴 |
| C# 8.0 | 2019 | Nullable 참조 |
| C# 9.0 | 2020 | Record |
| C# 10 | 2021 | 간결성 |
| C# 11 | 2022 | 문자열 개선 |
| C# 12 | 2023 | 간결성 극대화 |
| C# 13 | 2024 | 성능 개선 |
| C# 14 | 2025+ | 미래 기능 |

---

이 부록은 C# 언어의 발전 과정을 한눈에 파악할 수 있도록 요약한 것입니다. 각 기능의 자세한 사용법과 예제는 해당 장(특히 26장)을 참조하세요.
