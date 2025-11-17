# 12장. 예외 처리

프로그램을 작성하다 보면 예상치 못한 상황이 발생할 수 있습니다. 파일이 존재하지 않거나, 네트워크 연결이 끊어지거나, 사용자가 잘못된 입력을 제공하는 등 다양한 오류 상황에 직면하게 됩니다. 이러한 오류를 적절히 처리하지 않으면 프로그램이 갑자기 종료되거나 예측할 수 없는 동작을 할 수 있습니다. **예외 처리(Exception Handling)**는 프로그램 실행 중 발생하는 오류를 감지하고 적절하게 대응하여, 프로그램의 안정성과 신뢰성을 높이는 핵심 메커니즘입니다.

**예외 처리의 역사적 발전과 철학:**

C#의 예외 처리 시스템은 구조화된 예외 처리(SEH, Structured Exception Handling) 모델을 따르며, 이는 1990년대 초반 Microsoft가 Windows NT에서 도입한 개념입니다. 그러나 예외 처리의 역사는 훨씬 더 오래되었습니다. 1960년대 초 PL/I 언어에서 처음으로 예외 처리 메커니즘이 도입되었고, 1970년대 CLU 언어의 Barbara Liskov가 현대적인 예외 처리 구조를 확립했습니다. 1980년대 C++에서 `try-catch` 구문이 도입되면서 현재 우리가 사용하는 형태의 예외 처리가 자리 잡았습니다.

예외 처리는 단순한 오류 보고 메커니즘을 넘어서, 프로그램의 제어 흐름을 관리하는 고급 추상화입니다. 이는 Edsger Dijkstra의 구조적 프로그래밍(Structured Programming) 원칙과 밀접하게 연관되어 있으며, `goto` 문의 무분별한 사용을 제한하고 프로그램의 추론 가능성(Reasoning)을 높이려는 노력의 일환입니다. 예외 처리는 "비정상적인 상황"과 "정상적인 흐름"을 명확히 분리함으로써, 프로그램의 주요 로직을 방해하지 않으면서도 오류를 체계적으로 관리할 수 있게 합니다.

**소프트웨어 공학적 관점에서의 예외 처리:**

예외 처리는 여러 소프트웨어 공학 원칙을 실현합니다. 첫째, **관심사의 분리(Separation of Concerns)** 원칙에 따라 정상 로직과 오류 처리 로직을 분리합니다. 둘째, **방어적 프로그래밍(Defensive Programming)**을 가능하게 하여 예상치 못한 입력이나 상태에 대응할 수 있습니다. 셋째, **계약에 의한 설계(Design by Contract)** 패러다임에서 사전조건(Precondition), 사후조건(Postcondition), 불변조건(Invariant)을 검증하고 위반 시 예외를 발생시킵니다.

이전의 오류 코드 반환 방식에 비해, 예외 처리는 정상적인 프로그램 흐름과 오류 처리 로직을 분리하여 코드의 가독성과 유지보수성을 크게 향상시킵니다. Tony Hoare는 null 참조를 "10억 달러짜리 실수(Billion-dollar Mistake)"라고 불렀는데, 예외 처리는 이러한 문제를 체계적으로 다루는 메커니즘을 제공합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 C#의 예외 처리 메커니즘을 체계적으로 학습하게 됩니다:

- **예외의 이론적 기초**: 예외가 무엇이며, 왜 필요한지를 역사적 맥락과 소프트웨어 공학 원칙을 통해 깊이 이해합니다. 오류 코드 방식과 예외 처리 방식의 근본적인 차이를 파악하고, .NET의 예외 계층 구조와 CLR(Common Language Runtime)의 예외 처리 메커니즘을 학습합니다.

- **try-catch-finally의 의미론**: 예외를 처리하는 기본 문법과 패턴을 익히며, 스택 해제(Stack Unwinding), 호출 스택(Call Stack) 추적, 그리고 예외 전파(Exception Propagation) 메커니즘을 이해합니다. 리소스 관리와 RAII(Resource Acquisition Is Initialization) 패턴의 C# 구현을 배웁니다.

- **예외 던지기의 전략**: `throw` 키워드를 사용하여 예외를 발생시키는 방법과, 실패 빠르기(Fail-fast) 원칙, 방어적 프로그래밍(Defensive Programming), 그리고 적절한 예외 타입 선택 기준을 학습합니다. throw vs throw ex의 스택 추적 보존 메커니즘과 성능 고려사항을 이해합니다.

- **사용자 정의 예외의 설계**: 애플리케이션의 도메인에 특화된 예외 클래스를 설계하는 방법을 배우며, Liskov 치환 원칙(Liskov Substitution Principle)과 예외 계층 구조 설계, 직렬화(Serialization) 지원, 그리고 InnerException을 통한 예외 체이닝(Exception Chaining) 패턴을 학습합니다.

- **예외 필터의 고급 활용**: C# 6.0에서 도입된 `when` 키워드를 활용한 조건부 예외 처리를 익히며, 선언적 예외 처리(Declarative Exception Handling), 로깅과 관측성(Observability), 그리고 재시도 패턴(Retry Pattern)과 회로 차단기 패턴(Circuit Breaker Pattern)의 기초를 이해합니다.

**학습 목표:**
- 예외 처리의 이론적 기반과 소프트웨어 공학 원칙 이해
- CLR의 예외 처리 메커니즘과 성능 특성 파악
- 견고한(Robust) 소프트웨어 설계를 위한 예외 처리 전략 수립
- 도메인 주도 설계(Domain-Driven Design)에서의 예외 활용
- 관측 가능한(Observable) 시스템을 위한 예외 로깅 및 모니터링

---

## 12.1 예외란 무엇인가?

**예외(Exception)**는 프로그램 실행 중에 발생하는 예상치 못한 문제 또는 오류 상황을 나타내는 객체입니다. 예외는 정상적인 프로그램 흐름을 방해하며, 적절히 처리하지 않으면 프로그램이 비정상적으로 종료될 수 있습니다. 그러나 예외를 단순히 "오류"로만 이해하는 것은 피상적입니다. 예외는 실제로 프로그램의 **제어 흐름 메커니즘(Control Flow Mechanism)**이자, **타입 안전한 오류 전달 수단**이며, **계약(Contract) 위반의 신호**입니다.

**컴퓨터 과학적 관점에서의 예외:**

예외 처리는 프로그램의 정상 실행 경로(Normal Execution Path)와 예외 실행 경로(Exceptional Execution Path)를 명확히 구분하는 메커니즘입니다. 이는 제어 흐름 그래프(Control Flow Graph)에서 두 가지 유형의 엣지(Edge)를 생성합니다: 정상 흐름 엣지와 예외 흐름 엣지. CLR(Common Language Runtime)은 예외가 발생하면 현재 실행 중인 메서드의 스택 프레임(Stack Frame)에서 적절한 예외 핸들러를 찾기 위해 **스택 해제(Stack Unwinding)** 과정을 수행합니다.

스택 해제는 다음과 같은 단계로 진행됩니다:
1. **예외 객체 생성**: 예외가 발생하면 CLR은 해당 예외 타입의 인스턴스를 힙(Heap)에 생성하고 스택 추적 정보를 캡처합니다.
2. **핸들러 검색**: 현재 메서드의 보호 블록(Protected Blocks)을 검사하여 일치하는 catch 블록을 찾습니다.
3. **스택 프레임 해제**: 일치하는 핸들러가 없으면 현재 스택 프레임을 제거하고 호출자의 프레임으로 이동합니다.
4. **반복**: 적절한 핸들러를 찾거나 스택이 완전히 해제될 때까지 반복합니다.
5. **종료**: 핸들러를 찾으면 실행하고, 찾지 못하면 프로그램을 종료합니다(UnhandledException 이벤트 발생).

이 과정은 O(n) 시간 복잡도를 가지며(n은 호출 스택 깊이), 이는 예외 처리가 정상 흐름에 비해 **상대적으로 비용이 높은 작업**임을 의미합니다. 따라서 예외는 진정으로 예외적인(Exceptional) 상황에서만 사용해야 하며, 일반적인 제어 흐름에는 사용하지 않아야 합니다.

### 예외의 필요성과 오류 처리 패러다임의 진화

전통적인 프로그래밍에서는 오류를 처리하기 위해 반환 코드(return code)나 전역 오류 변수를 사용했습니다. 예를 들어, C 언어에서는 함수가 성공하면 0을, 실패하면 음수 또는 특정 오류 코드를 반환하는 방식이 일반적이었습니다. UNIX 시스템 호출은 `errno` 전역 변수를 통해 오류 정보를 제공합니다. 그러나 이 방식은 다음과 같은 근본적인 문제점이 있습니다:

**1. 오류 처리 코드와 정상 로직의 혼재(Code Tangling):**

모든 함수 호출 후 반환 코드를 검사해야 하므로 코드가 복잡해지고 가독성이 저하됩니다. 이는 **횡단 관심사(Cross-cutting Concern)**의 전형적인 예로, 핵심 비즈니스 로직이 오류 처리 코드에 의해 가려지는 현상을 초래합니다.

```c
// C 언어 스타일의 오류 처리 - 코드 중복과 복잡성
int result = function1();
if (result != 0) {
    cleanup();
    return ERROR_CODE_1;
}

result = function2();
if (result != 0) {
    cleanup();
    return ERROR_CODE_2;
}

result = function3();
if (result != 0) {
    cleanup();
    return ERROR_CODE_3;
}
```

**2. 오류 전파의 어려움(Error Propagation Complexity):**

여러 단계의 함수 호출을 거쳐 오류를 전달하려면 각 단계에서 오류 코드를 확인하고 전파해야 합니다. 이는 **오류 무시(Error Swallowing)** 버그의 주요 원인이 되며, 중간 단계에서 오류 검사를 누락하면 치명적인 결과를 초래할 수 있습니다. Raymond Chen은 이를 "Silent Failure"라고 명명하며, 디버깅을 극도로 어렵게 만드는 요인으로 지적했습니다.

**3. 일관성 부족과 의미론적 모호성(Semantic Ambiguity):**

개발자마다 다른 오류 코드 체계를 사용할 수 있어 혼란을 야기합니다. 예를 들어, 어떤 함수는 -1을 오류로 사용하고, 다른 함수는 NULL을 반환하며, 또 다른 함수는 특정 정수 코드를 사용합니다. 이는 **타입 안전성(Type Safety)** 부족으로 이어져, 컴파일 시점에 오류를 감지할 수 없게 만듭니다.

**4. 반환값과 오류의 혼합(Return Value Confusion):**

함수가 유효한 값과 오류 코드를 같은 타입으로 반환하면, 어떤 값이 성공적인 결과이고 어떤 값이 오류인지 구분하기 어려워집니다. 이는 **대역 밖 값(Out-of-Band Value)** 문제로 알려져 있으며, C 함수가 -1이나 NULL을 특별한 의미로 사용하는 이유입니다.

**예외 처리 패러다임의 우수성:**

C#의 예외 처리 메커니즘은 이러한 문제를 체계적으로 해결합니다:

**1. 정상 로직과 오류 처리의 분리(Separation of Normal and Exceptional Flow):**

`try-catch` 구조는 정상적인 코드 흐름과 오류 처리 코드를 명확히 구분합니다. 이는 **단일 책임 원칙(Single Responsibility Principle)**을 실현하며, 코드의 주요 로직이 명확하게 드러나도록 합니다.

```csharp
// 예외 처리 - 깔끔한 정상 흐름
try {
    function1();
    function2();
    function3();
}
catch (Exception ex) {
    HandleError(ex);
}
finally {
    Cleanup();
}
```

**2. 자동 오류 전파(Automatic Error Propagation):**

예외는 적절한 처리기가 나타날 때까지 호출 스택을 자동으로 따라 올라갑니다. 이는 중간 계층에서 명시적인 오류 전파 코드를 작성할 필요를 없애며, **암묵적 전파(Implicit Propagation)**를 통해 코드를 단순화합니다. 이는 함수형 프로그래밍의 **모나드(Monad)** 개념과 유사하며, Option/Maybe 타입이나 Result 타입과 같은 대안적 오류 처리 방식의 이론적 기반이 됩니다.

**3. 타입 안전성과 계층적 처리(Type Safety and Hierarchical Handling):**

예외는 객체이므로 타입 시스템을 통해 다양한 종류의 오류를 구별할 수 있습니다. 예외 계층 구조를 통해 일반적인 예외부터 구체적인 예외까지 선택적으로 처리할 수 있으며, 이는 **리스코프 치환 원칙(Liskov Substitution Principle)**을 따릅니다. 컴파일러는 타입 검사를 통해 잘못된 예외 처리를 방지하며, 이는 **정적 타입 안전성(Static Type Safety)**의 중요한 이점입니다.

**4. 풍부한 문맥 정보(Rich Contextual Information):**

예외 객체는 오류 메시지, 스택 추적, 내부 예외 등 풍부한 디버깅 정보를 포함합니다. 이는 **관측성(Observability)**과 **디버깅 용이성(Debuggability)**을 크게 향상시키며, 분산 시스템에서는 상관 관계 ID(Correlation ID)나 추적 컨텍스트(Tracing Context)를 예외에 포함시켜 **분산 추적(Distributed Tracing)**을 구현할 수 있습니다.

**예외 처리의 이론적 기반:**

예외 처리는 **continuation-passing style (CPS)**의 특수한 형태로 볼 수 있습니다. 정상 continuation(성공 시 다음 실행할 코드)과 예외 continuation(실패 시 실행할 코드)을 구분하여, 프로그램의 제어 흐름을 명시적으로 관리합니다. 이는 함수형 프로그래밍에서 Either 모나드나 Result 타입과 개념적으로 유사하며, 성공(Right/Ok)과 실패(Left/Error) 경로를 타입으로 구분합니다.

Haskell 같은 순수 함수형 언어는 부작용(Side Effects)을 허용하지 않으므로 예외 대신 Maybe나 Either 같은 타입을 사용하지만, C#의 예외는 명령형 언어에서 오류를 처리하는 실용적이고 효율적인 방법입니다. Rust는 Result<T, E> 타입으로 성공(Ok)과 실패(Err)를 명시적으로 표현하는 중간 접근 방식을 취합니다.

### 예외가 발생하는 상황

다음은 예외가 발생할 수 있는 일반적인 상황들입니다:

**1. 잘못된 사용자 입력:**

```csharp
// 숫자가 아닌 문자열을 정수로 변환하려고 시도
string input = "abc";
int number = int.Parse(input);  // FormatException 발생
```

**2. 존재하지 않는 파일 접근:**

```csharp
// 존재하지 않는 파일을 열려고 시도
string content = File.ReadAllText("없는파일.txt");  // FileNotFoundException 발생
```

**3. 0으로 나누기:**

```csharp
int a = 10;
int b = 0;
int result = a / b;  // DivideByZeroException 발생
```

**4. null 참조 접근:**

```csharp
string text = null;
int length = text.Length;  // NullReferenceException 발생
```

**5. 배열 범위 초과:**

```csharp
int[] numbers = { 1, 2, 3 };
int value = numbers[5];  // IndexOutOfRangeException 발생
```

### 예외 계층 구조와 타입 시스템

.NET의 모든 예외 클래스는 `System.Exception` 기본 클래스를 상속하며, 이는 **단일 루트 계층 구조(Single Root Hierarchy)**를 형성합니다. 이러한 설계는 **리스코프 치환 원칙(Liskov Substitution Principle, LSP)**을 따르며, 모든 예외를 일관된 방식으로 처리할 수 있게 합니다. Java와 유사하게, C#의 예외 계층은 타입 시스템을 활용하여 **정적 타입 안전성(Static Type Safety)**을 제공합니다.

**예외 계층 구조의 설계 원칙:**

```
System.Exception (추상 기본 클래스)
├── SystemException (시스템 수준 예외)
│   ├── ArgumentException (잘못된 인수)
│   │   ├── ArgumentNullException (null 인수)
│   │   └── ArgumentOutOfRangeException (범위 밖 인수)
│   ├── ArithmeticException (산술 연산 오류)
│   │   ├── DivideByZeroException (0으로 나누기)
│   │   └── OverflowException (오버플로우)
│   ├── IndexOutOfRangeException (배열 인덱스 범위 초과)
│   ├── InvalidOperationException (잘못된 상태에서의 작업)
│   ├── NullReferenceException (null 참조 역참조)
│   ├── InvalidCastException (잘못된 타입 캐스팅)
│   ├── NotSupportedException (지원되지 않는 작업)
│   └── OutOfMemoryException (메모리 부족)
├── IO 관련 예외
│   ├── IOException (I/O 오류)
│   │   ├── FileNotFoundException (파일 없음)
│   │   ├── DirectoryNotFoundException (디렉토리 없음)
│   │   ├── PathTooLongException (경로 길이 초과)
│   │   └── EndOfStreamException (스트림 끝)
│   └── UnauthorizedAccessException (접근 권한 없음)
├── Threading 관련 예외
│   ├── ThreadAbortException (스레드 중단)
│   ├── ThreadInterruptedException (스레드 인터럽트)
│   └── SynchronizationLockException (동기화 오류)
├── Reflection 관련 예외
│   ├── TargetInvocationException (리플렉션 호출 오류)
│   └── AmbiguousMatchException (모호한 멤버 매칭)
└── ApplicationException (사용자 정의 예외 - 현재는 비권장)
```

**SystemException vs ApplicationException의 역사적 맥락:**

초기 .NET Framework 설계에서는 시스템 예외(SystemException)와 애플리케이션 예외(ApplicationException)를 구분하려 했습니다. SystemException은 CLR이나 시스템에서 발생하는 예외를, ApplicationException은 사용자 코드에서 발생하는 예외를 나타내도록 의도되었습니다. 그러나 실무에서 이 구분은 **의미론적 가치(Semantic Value)**가 거의 없는 것으로 판명되었습니다.

현재 Microsoft의 공식 가이드라인은 **Exception 클래스를 직접 상속**하도록 권장합니다. 이는 다음과 같은 이유 때문입니다:

1. **구분의 모호성**: 많은 .NET 라이브러리 예외들이 일관되지 않게 계층 구조를 사용했습니다.
2. **실용적 가치 부족**: catch 블록에서 SystemException과 ApplicationException을 구분하여 처리하는 경우가 거의 없습니다.
3. **설계 변경**: Jeffrey Richter와 같은 .NET 아키텍트들이 이 구분을 실수로 인정했습니다.

**예외 타입의 의미론적 분류:**

예외는 그 원인과 복구 가능성에 따라 다음과 같이 분류할 수 있습니다:

**1. 프로그램 오류(Programming Errors):**
- **NullReferenceException**: null 역참조 - 거의 항상 프로그래머 실수
- **IndexOutOfRangeException**: 배열 범위 초과 - 경계 검사 누락
- **InvalidCastException**: 잘못된 타입 캐스팅 - 타입 검사 누락
- **StackOverflowException**: 스택 오버플로우 - 무한 재귀나 과도한 중첩

이러한 예외는 **버그(Bugs)**를 나타내며, 일반적으로 catch하지 않고 프로그램을 수정해야 합니다. Robert C. Martin의 "Clean Code"에서는 이를 **"방어 불가능한 예외(Undefendable Exceptions)"**라고 부릅니다.

**2. 시스템 리소스 오류(System Resource Errors):**
- **OutOfMemoryException**: 메모리 부족 - 복구 매우 어려움
- **ThreadAbortException**: 스레드 중단 - 특수 처리 필요
- **StackOverflowException**: 스택 고갈 - catch 불가능

이러한 예외는 **치명적 예외(Fatal Exceptions)**로, 일반적인 try-catch로 복구할 수 없습니다. 프로그램을 재시작하거나 관리자의 개입이 필요합니다.

**3. 외부 요인 오류(External Failures):**
- **FileNotFoundException**: 파일 없음 - 사용자에게 알리거나 대안 제공
- **IOException**: I/O 오류 - 재시도 가능
- **NetworkException**: 네트워크 오류 - 일시적일 수 있음
- **UnauthorizedAccessException**: 권한 없음 - 사용자에게 권한 요청

이러한 예외는 **복구 가능한 예외(Recoverable Exceptions)**로, 적절한 처리를 통해 프로그램을 계속 실행할 수 있습니다. 이는 **회복력(Resilience)** 패턴의 대상이 됩니다.

**4. 논리적 오류(Logical Errors):**
- **ArgumentException**: 잘못된 인수 - 검증 필요
- **InvalidOperationException**: 잘못된 상태 - 상태 머신 설계 문제
- **NotSupportedException**: 지원되지 않는 작업 - API 계약 위반

이러한 예외는 **계약 위반(Contract Violations)**을 나타내며, **방어적 프로그래밍(Defensive Programming)**과 **사전조건 검사(Precondition Checking)**의 대상입니다.

**예외 계층과 다형성:**

예외 계층 구조는 **다형성(Polymorphism)**을 활용하여 일반적인 catch에서 구체적인 catch로의 점진적 처리를 가능하게 합니다. 이는 **개방-폐쇄 원칙(Open-Closed Principle)**을 실현하며, 새로운 예외 타입을 추가해도 기존 catch 블록이 계속 작동합니다.

```csharp
try {
    RiskyOperation();
}
catch (ArgumentNullException ex) {
    // 가장 구체적: null 인수 처리
}
catch (ArgumentException ex) {
    // 중간 수준: 일반 인수 오류 처리
}
catch (Exception ex) {
    // 가장 일반적: 모든 예외 처리
}
```

이러한 계층적 처리는 **타입 기반 디스패칭(Type-based Dispatching)**의 예이며, 객체지향 프로그래밍의 **동적 디스패치(Dynamic Dispatch)** 메커니즘을 활용합니다.

### 예외 객체의 주요 속성

모든 예외 객체는 `Exception` 클래스로부터 다음과 같은 유용한 속성을 상속받습니다:

- **Message**: 예외에 대한 설명 메시지
- **StackTrace**: 예외가 발생한 호출 스택 정보
- **InnerException**: 현재 예외를 발생시킨 원인이 되는 내부 예외
- **Source**: 예외를 발생시킨 애플리케이션 또는 객체의 이름
- **HelpLink**: 예외에 대한 도움말 파일의 URL

**예외 정보 확인 예제:**

```csharp
try
{
    int number = int.Parse("abc");
}
catch (Exception ex)
{
    Console.WriteLine($"예외 타입: {ex.GetType().Name}");
    Console.WriteLine($"메시지: {ex.Message}");
    Console.WriteLine($"스택 추적:\n{ex.StackTrace}");
}

// 출력 예시:
// 예외 타입: FormatException
// 메시지: Input string was not in a correct format.
// 스택 추적:
// at System.Int32.Parse(String s)
// at Program.<Main>$(String[] args) in /home/username/Program.cs:line 3
```

---

## 12.2 try-catch-finally

`try-catch-finally` 구문은 C#에서 예외를 처리하는 핵심 메커니즘입니다. 이 구조는 예외가 발생할 수 있는 코드를 격리하고, 예외가 발생했을 때 적절히 대응하며, 예외 발생 여부와 관계없이 반드시 실행해야 하는 정리 코드를 보장합니다.

**구조화된 예외 처리의 이론적 기반:**

try-catch-finally는 **구조화된 프로그래밍(Structured Programming)**의 원칙을 예외 처리에 적용한 것입니다. Edsger Dijkstra가 1968년 발표한 "Go To Statement Considered Harmful"에서 제안한 원칙에 따라, 예외 처리도 명확한 진입점과 탈출점을 가진 구조화된 블록으로 설계되었습니다. 이는 프로그램의 **제어 흐름 추론(Control Flow Reasoning)**을 가능하게 하며, 코드의 **검증 가능성(Verifiability)**을 높입니다.

**CLR의 예외 처리 메커니즘:**

CLR(Common Language Runtime)은 예외 처리를 위해 **Two-Pass 모델**을 사용합니다:

1. **첫 번째 패스(First Pass - Exception Dispatch)**: 예외가 발생하면 CLR은 스택을 따라 올라가면서 적절한 catch 블록을 찾습니다. 이 과정에서 스택 프레임을 실제로 해제하지 않고 검색만 수행합니다. 이는 **예외 필터(when 절)**를 평가하고 **InnerException 체인**을 구축하는 데 필요합니다.

2. **두 번째 패스(Second Pass - Stack Unwinding)**: 적절한 catch 블록을 찾으면, CLR은 스택을 다시 해제하면서 각 스택 프레임의 finally 블록을 실행하고 리소스를 정리합니다. 이는 **RAII(Resource Acquisition Is Initialization)** 패턴의 C# 구현입니다.

이러한 Two-Pass 모델은 성능 비용이 있지만, **결정론적 리소스 해제(Deterministic Resource Disposal)**와 **예외 안전성(Exception Safety)**을 보장합니다. C++의 **RAII**와 Java의 **try-with-resources**, 그리고 C#의 **using 문**은 모두 이 개념의 변형입니다.

**예외 처리의 성능 특성:**

예외 처리는 "제로 비용 추상화(Zero-cost Abstraction)"가 **아닙니다**. 예외가 발생하면:
- 스택 추적(Stack Trace) 캡처: O(n) 시간, n은 호출 스택 깊이
- 스택 해제(Stack Unwinding): O(n) 시간
- 예외 객체 할당: 힙 메모리 할당 비용

따라서 예외는 **진정으로 예외적인 상황**에서만 사용해야 하며, 정상적인 제어 흐름에는 사용하지 않는 것이 원칙입니다. 이는 **"Exceptions for Exceptional Conditions"** 원칙으로 알려져 있습니다.

### 12.2.1 기본 예외 처리

**기본 구조:**

```csharp
try
{
    // 예외가 발생할 수 있는 코드
}
catch (예외타입 변수명)
{
    // 예외 처리 코드
}
```

**실행 흐름과 제어 전달(Control Transfer):**

1. `try` 블록 안의 코드가 순차적으로 실행됩니다.
2. 예외가 발생하지 않으면 `catch` 블록은 건너뛰고 계속 진행됩니다.
3. 예외가 발생하면 즉시 해당하는 `catch` 블록으로 제어가 이동합니다. 이는 **비지역 점프(Non-local Jump)**로, `goto`와 유사하지만 구조화되어 있습니다.
4. `catch` 블록의 코드가 실행된 후 프로그램은 정상적으로 계속됩니다.

**예외 매칭 알고리즘:**

CLR은 예외 타입을 매칭할 때 **공변성(Covariance)** 규칙을 따릅니다. 즉, catch 블록의 예외 타입이 실제 발생한 예외 타입의 기본 클래스이거나 동일한 타입이면 매칭됩니다. 이는 **다형성(Polymorphism)**의 응용이며, 리스코프 치환 원칙(Liskov Substitution Principle)을 따릅니다.

```csharp
try {
    throw new ArgumentNullException();
}
catch (ArgumentException ex) {
    // ArgumentNullException은 ArgumentException의 파생 클래스이므로 매칭됨
    Console.WriteLine("잡혔습니다!");
}
```

**기본 예제:**

```csharp
Console.Write("첫 번째 숫자를 입력하세요: ");
string input1 = Console.ReadLine();

Console.Write("두 번째 숫자를 입력하세요: ");
string input2 = Console.ReadLine();

try
{
    int num1 = int.Parse(input1);
    int num2 = int.Parse(input2);
    int result = num1 / num2;
    
    Console.WriteLine($"결과: {num1} / {num2} = {result}");
}
catch (FormatException)
{
    Console.WriteLine("오류: 숫자 형식이 올바르지 않습니다.");
}
catch (DivideByZeroException)
{
    Console.WriteLine("오류: 0으로 나눌 수 없습니다.");
}

Console.WriteLine("프로그램이 계속 실행됩니다.");

// 잘못된 입력 예시:
// 첫 번째 숫자를 입력하세요: abc
// 두 번째 숫자를 입력하세요: 5
// 오류: 숫자 형식이 올바르지 않습니다.
// 프로그램이 계속 실행됩니다.
```

**예외 정보 활용:**

```csharp
try
{
    int[] numbers = { 1, 2, 3 };
    Console.WriteLine(numbers[10]);
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine("배열 범위를 벗어났습니다.");
    Console.WriteLine($"상세 정보: {ex.Message}");
}

// 출력:
// 배열 범위를 벗어났습니다.
// 상세 정보: Index was outside the bounds of the array.
```

**파일 읽기 예외 처리:**

```csharp
try
{
    string content = File.ReadAllText("data.txt");
    Console.WriteLine($"파일 내용: {content}");
}
catch (FileNotFoundException)
{
    Console.WriteLine("오류: 파일을 찾을 수 없습니다.");
}
catch (UnauthorizedAccessException)
{
    Console.WriteLine("오류: 파일에 접근할 권한이 없습니다.");
}
catch (IOException ex)
{
    Console.WriteLine($"오류: 파일을 읽는 중 문제가 발생했습니다. ({ex.Message})");
}
```

### 12.2.2 여러 catch 블록

하나의 `try` 블록에 여러 개의 `catch` 블록을 연결하여 다양한 유형의 예외를 각각 다르게 처리할 수 있습니다. `catch` 블록은 위에서 아래로 순서대로 평가되며, 첫 번째로 일치하는 예외 타입의 `catch` 블록이 실행됩니다. 이는 **패턴 매칭(Pattern Matching)**의 초기 형태이며, 함수형 프로그래밍의 **가드 절(Guard Clauses)**과 유사합니다.

**타입 계층과 catch 순서의 중요성:**

catch 블록의 순서는 단순한 스타일 문제가 아니라 **타입 안전성(Type Safety)**과 **정확성(Correctness)**의 문제입니다. C# 컴파일러는 도달 불가능한 catch 블록을 **컴파일 오류**로 처리하여, 개발자가 실수로 잘못된 순서를 사용하는 것을 방지합니다. 이는 **정적 분석(Static Analysis)**의 좋은 예입니다.

**중요한 규칙:**

- **더 구체적인(파생된) 예외 타입을 먼저 배치**: 타입 계층에서 하위(자식) 클래스가 상위(부모) 클래스보다 먼저 와야 합니다.
- **더 일반적인(기본) 예외 타입은 나중에 배치**: `Exception` 같은 최상위 클래스는 마지막에 배치합니다.
- **순서 위반 시 컴파일 오류**: C# 컴파일러는 "CS0160: A previous catch clause already catches all exceptions of this or of a super type" 오류를 발생시킵니다.

**컴파일러의 도달성 분석(Reachability Analysis):**

컴파일러는 제어 흐름 그래프(Control Flow Graph)를 분석하여 각 catch 블록에 도달 가능한지 검사합니다. 만약 이전 catch 블록이 더 일반적인 타입을 잡는다면, 이후의 더 구체적인 catch 블록은 절대 실행될 수 없으므로(unreachable code) 컴파일 오류가 발생합니다. 이는 **데드 코드 제거(Dead Code Elimination)**의 일환입니다.

**올바른 순서 예제:**

```csharp
try
{
    Console.Write("숫자를 입력하세요: ");
    string input = Console.ReadLine();
    
    int number = int.Parse(input);
    int result = 100 / number;
    
    Console.WriteLine($"100 / {number} = {result}");
}
catch (FormatException)
{
    // 가장 구체적인 예외
    Console.WriteLine("숫자 형식이 올바르지 않습니다.");
}
catch (DivideByZeroException)
{
    // 구체적인 예외
    Console.WriteLine("0으로 나눌 수 없습니다.");
}
catch (Exception ex)
{
    // 가장 일반적인 예외 (맨 마지막에 배치)
    Console.WriteLine($"예상치 못한 오류가 발생했습니다: {ex.Message}");
}
```

**잘못된 순서 (컴파일 오류):**

```csharp
try
{
    // 코드...
}
catch (Exception ex)
{
    // Exception이 모든 예외의 기본 클래스이므로 먼저 잡으면
    // 아래의 catch 블록에는 절대 도달할 수 없음
    Console.WriteLine("오류 발생");
}
catch (FormatException)  // ❌ 컴파일 오류: Unreachable catch block
{
    Console.WriteLine("형식 오류");
}
```

**Exception 필터링이 없는 catch:**

예외 변수를 선언하지 않고 모든 예외를 잡을 수도 있습니다:

```csharp
try
{
    // 예외 발생 가능 코드
}
catch
{
    // 모든 예외를 잡지만, 예외 객체에 접근할 수 없음
    Console.WriteLine("오류가 발생했습니다.");
}
```

### 12.2.3 finally 블록

`finally` 블록은 예외 발생 여부와 관계없이 **항상 실행**되는 코드 블록입니다. 주로 리소스 정리(파일 닫기, 데이터베이스 연결 해제, 메모리 해제 등)에 사용됩니다. 이는 C++의 **RAII(Resource Acquisition Is Initialization)** 패턴과 **소멸자(Destructor)**의 C# 버전으로 볼 수 있습니다.

**기본 구조:**

```csharp
try
{
    // 예외가 발생할 수 있는 코드
}
catch (Exception ex)
{
    // 예외 처리
}
finally
{
    // 항상 실행되는 코드
}
```

**finally의 보장된 실행(Guaranteed Execution):**

finally 블록은 다음과 같은 모든 상황에서 실행이 보장됩니다:

1. **정상 실행**: 예외가 발생하지 않아도 실행됩니다.
2. **예외 처리 후**: 예외가 발생하고 catch 블록에서 처리되어도 실행됩니다.
3. **return 문 존재**: try나 catch 블록에서 `return` 문이 있어도 실행됩니다.
4. **예외 재발생**: catch 블록에서 `throw`로 예외를 다시 던져도 실행됩니다.
5. **다른 예외 발생**: catch 블록 내에서 새로운 예외가 발생해도 실행됩니다.

**예외: 실행되지 않는 극단적 경우:**

finally가 실행되지 않는 유일한 경우들:
- **StackOverflowException**: 스택이 완전히 고갈되어 finally를 실행할 공간이 없음
- **Environment.FailFast()**: 즉시 프로세스 종료
- **프로세스 강제 종료**: 운영체제 수준에서 프로세스가 강제 종료됨 (kill -9 등)
- **무한 루프**: try 블록 내 무한 루프로 finally에 도달하지 못함
- **전원 차단**: 물리적으로 컴퓨터가 꺼짐

**RAII와 결정론적 리소스 해제:**

C++의 RAII는 객체의 생성자에서 리소스를 획득하고 소멸자에서 해제하는 패턴입니다. C#은 가비지 컬렉션 언어이므로 소멸자 실행 시점이 비결정론적(Non-deterministic)입니다. finally 블록과 `using` 문은 이러한 문제를 해결하여 **결정론적 리소스 해제(Deterministic Resource Disposal)**를 제공합니다.

```csharp
// using 문은 자동으로 try-finally로 변환됨
using (var stream = File.OpenRead("data.txt"))
{
    // 파일 사용
}

// 위 코드는 컴파일러에 의해 아래처럼 변환됨:
FileStream stream = null;
try
{
    stream = File.OpenRead("data.txt");
    // 파일 사용
}
finally
{
    if (stream != null)
        stream.Dispose();
}
```

**finally와 예외 처리의 상호작용:**

finally 블록 내에서 예외가 발생하면 복잡한 상황이 발생합니다. 원래 예외는 **덮어씌워지고(Suppressed)** 새로운 예외가 전파됩니다. 이는 디버깅을 어렵게 만들 수 있으므로, finally 블록에서는 예외를 발생시키지 않도록 주의해야 합니다.

```csharp
try
{
    throw new Exception("원본 예외");
}
finally
{
    throw new Exception("finally에서 발생한 예외");  // 원본 예외를 덮어씀!
}
// 결과: "finally에서 발생한 예외"만 전파됨
```

이는 **예외 안전성(Exception Safety)**의 중요한 고려사항이며, finally에서는 예외를 던지지 않는 것이 **강력한 예외 안전성 보장(Strong Exception Safety Guarantee)**의 일부입니다.

**리소스 정리 예제:**

```csharp
FileStream file = null;

try
{
    Console.WriteLine("파일을 엽니다.");
    file = new FileStream("data.txt", FileMode.Open);
    
    // 파일 작업 수행
    Console.WriteLine("파일 작업 중...");
    
    // 임의로 예외 발생
    throw new Exception("처리 중 오류 발생");
}
catch (Exception ex)
{
    Console.WriteLine($"오류: {ex.Message}");
}
finally
{
    // 예외 발생 여부와 관계없이 파일을 닫음
    if (file != null)
    {
        file.Close();
        Console.WriteLine("파일을 닫았습니다.");
    }
}

Console.WriteLine("프로그램 종료");

// 출력:
// 파일을 엽니다.
// 파일 작업 중...
// 오류: 처리 중 오류 발생
// 파일을 닫았습니다.
// 프로그램 종료
```

**finally와 return:**

```csharp
int Divide(int a, int b)
{
    try
    {
        Console.WriteLine("나눗셈 시도");
        return a / b;
    }
    catch (DivideByZeroException)
    {
        Console.WriteLine("0으로 나눌 수 없습니다.");
        return -1;
    }
    finally
    {
        // return 문이 있어도 finally는 실행됨
        Console.WriteLine("finally 블록 실행");
    }
}

int result = Divide(10, 2);
Console.WriteLine($"결과: {result}");

// 출력:
// 나눗셈 시도
// finally 블록 실행
// 결과: 5
```

**catch 없이 finally만 사용:**

`catch` 블록 없이 `finally`만 사용할 수도 있습니다. 이 경우 예외는 처리되지 않고 상위로 전파되지만, `finally` 블록은 실행됩니다:

```csharp
void ProcessData()
{
    Console.WriteLine("작업 시작");
    
    try
    {
        Console.WriteLine("처리 중...");
        throw new Exception("오류 발생");
    }
    finally
    {
        // 예외가 처리되지 않아도 실행됨
        Console.WriteLine("정리 작업 수행");
    }
    
    // 이 줄은 실행되지 않음 (예외가 전파됨)
    Console.WriteLine("작업 완료");
}

try
{
    ProcessData();
}
catch (Exception ex)
{
    Console.WriteLine($"상위에서 처리: {ex.Message}");
}

// 출력:
// 작업 시작
// 처리 중...
// 정리 작업 수행
// 상위에서 처리: 오류 발생
```

---

## 12.3 예외 던지기 (throw)

`throw` 키워드는 예외를 명시적으로 발생시키는 데 사용됩니다. 예외는 .NET 런타임이 자동으로 발생시키기도 하지만, 개발자가 특정 조건에서 의도적으로 예외를 던질 수도 있습니다. 이는 **방어적 프로그래밍(Defensive Programming)**과 **계약에 의한 설계(Design by Contract)**의 핵심 메커니즘입니다.

**예외 던지기의 철학적 기반:**

Bertrand Meyer의 **Eiffel 언어**에서 시작된 계약에 의한 설계(DbC) 개념에서, 메서드는 **사전조건(Precondition)**, **사후조건(Postcondition)**, **불변조건(Invariant)**을 가집니다. 예외 던지기는 이러한 계약이 위반되었을 때 시스템에 신호를 보내는 메커니즘입니다.

- **사전조건 위반**: 메서드 호출 전에 만족해야 할 조건이 위반됨 → ArgumentException 계열
- **사후조건 위반**: 메서드가 약속한 결과를 만들 수 없음 → InvalidOperationException
- **불변조건 위반**: 객체의 내부 상태가 일관성을 잃음 → InvalidOperationException

**실패 빠르기 원칙(Fail-Fast Principle):**

Martin Fowler와 Kent Beck이 주창한 이 원칙은 "오류를 발견하면 즉시 실패하라"는 것입니다. 오류를 숨기거나 무시하면 **버그 전파(Bug Propagation)**가 발생하여, 실제 원인과 증상이 나타나는 위치가 멀어져 디버깅이 극도로 어려워집니다. 예외를 던지는 것은 fail-fast를 구현하는 주요 방법입니다.

```csharp
// ❌ 나쁜 예: 오류를 숨김
public int Divide(int a, int b)
{
    if (b == 0) return 0;  // 조용히 실패 - 버그 전파
    return a / b;
}

// ✅ 좋은 예: 즉시 실패
public int Divide(int a, int b)
{
    if (b == 0) throw new ArgumentException("0으로 나눌 수 없습니다", nameof(b));
    return a / b;
}
```

### throw의 사용 시나리오

1. **입력 검증**: 잘못된 매개변수나 상태를 발견했을 때
2. **비즈니스 규칙 위반**: 애플리케이션의 비즈니스 로직이 위반되었을 때
3. **복구 불가능한 상황**: 프로그램이 계속 실행될 수 없는 상황일 때
4. **예외 재발생**: catch 블록에서 예외를 처리한 후 다시 상위로 전파할 때

### 새 예외 생성 및 던지기

**기본 문법:**

```csharp
throw new 예외타입("오류 메시지");
```

**간단한 예제:**

```csharp
void CheckAge(int age)
{
    if (age < 0)
    {
        throw new ArgumentException("나이는 음수일 수 없습니다.");
    }
    
    if (age < 18)
    {
        throw new InvalidOperationException("성인만 접근할 수 있습니다.");
    }
    
    Console.WriteLine($"접근 허용: {age}세");
}

try
{
    CheckAge(-5);
}
catch (ArgumentException ex)
{
    Console.WriteLine($"인수 오류: {ex.Message}");
}
catch (InvalidOperationException ex)
{
    Console.WriteLine($"작업 오류: {ex.Message}");
}

// 출력:
// 인수 오류: 나이는 음수일 수 없습니다.
```

### 적절한 예외 타입 선택

.NET은 다양한 표준 예외 타입을 제공하며, 상황에 맞는 예외를 선택하는 것이 중요합니다:

**ArgumentException 계열:**

```csharp
void SetName(string name)
{
    if (name == null)
    {
        throw new ArgumentNullException(nameof(name), "이름은 null일 수 없습니다.");
    }
    
    if (string.IsNullOrWhiteSpace(name))
    {
        throw new ArgumentException("이름은 비어있을 수 없습니다.", nameof(name));
    }
    
    if (name.Length > 50)
    {
        throw new ArgumentOutOfRangeException(nameof(name), "이름은 50자를 초과할 수 없습니다.");
    }
    
    Console.WriteLine($"이름 설정: {name}");
}
```

**InvalidOperationException:**

```csharp
class BankAccount
{
    private decimal balance = 0;
    
    public void Withdraw(decimal amount)
    {
        if (amount > balance)
        {
            throw new InvalidOperationException($"잔액 부족: 현재 잔액은 {balance}원입니다.");
        }
        
        balance -= amount;
        Console.WriteLine($"{amount}원 출금 완료. 잔액: {balance}원");
    }
}

var account = new BankAccount();

try
{
    account.Withdraw(1000);
}
catch (InvalidOperationException ex)
{
    Console.WriteLine($"오류: {ex.Message}");
}

// 출력:
// 오류: 잔액 부족: 현재 잔액은 0원입니다.
```

### 예외 재발생 (Rethrowing)

`catch` 블록에서 예외를 처리한 후 다시 던질 수 있습니다. 이는 로깅이나 부분적 복구를 수행한 후 상위 호출자에게 예외를 알려야 할 때 유용합니다. 예외 재발생은 **횡단 관심사(Cross-cutting Concerns)**인 로깅, 모니터링, 감사(Auditing)를 구현하는 핵심 패턴입니다.

**throw vs throw ex의 근본적 차이:**

이 두 방식의 차이는 **스택 추적 보존(Stack Trace Preservation)**과 관련이 있습니다. 이는 디버깅과 운영 환경에서의 문제 추적에 매우 중요합니다.

```csharp
// 방법 1: throw (권장) - 원본 스택 추적 보존
try
{
    RiskyOperation();
}
catch (Exception ex)
{
    Logger.Log($"오류 발생: {ex.Message}");
    throw;  // 원본 예외를 그대로 다시 던짐
}

// 방법 2: throw ex (비권장) - 스택 추적이 여기서부터 시작됨
try
{
    RiskyOperation();
}
catch (Exception ex)
{
    Logger.Log($"오류 발생: {ex.Message}");
    throw ex;  // 새로운 스택 추적이 생성됨 - 원본 위치 정보 손실!
}
```

**스택 추적의 내부 메커니즘:**

`throw`를 사용하면 CLR은 예외 객체의 내부 스택 추적을 그대로 유지합니다. 반면 `throw ex`를 사용하면 새로운 throw 지점에서 스택 추적을 다시 캡처합니다. 이는 **성능**과 **디버깅 정보** 모두에 영향을 미칩니다.

예외 객체는 내부적으로 `_stackTrace`와 `_remoteStackTrace` 필드를 가지고 있으며, 리플렉션의 `SetStackTrace` 메서드를 통해서만 수정할 수 있습니다. `throw` 문은 이를 보존하지만, `throw ex`는 덮어씁니다.

**ExceptionDispatchInfo를 사용한 고급 재발생:**

.NET 4.5부터 도입된 `ExceptionDispatchInfo`는 예외와 그 컨텍스트를 완전히 캡처하고 나중에 다시 던질 수 있게 합니다. 이는 **비동기 프로그래밍**과 **병렬 처리**에서 매우 유용합니다.

```csharp
using System.Runtime.ExceptionServices;

ExceptionDispatchInfo capturedEx = null;

try
{
    DangerousOperation();
}
catch (Exception ex)
{
    capturedEx = ExceptionDispatchInfo.Capture(ex);
}

if (capturedEx != null)
{
    // 나중에 원본 스택 추적을 완전히 보존하며 재발생
    capturedEx.Throw();
}
```

이는 Task 기반 비동기 패턴(TAP)에서 예외를 여러 스레드 간에 전파할 때 내부적으로 사용되는 메커니즘입니다.

**실용적인 재발생 예제:**

```csharp
void ProcessFile(string filename)
{
    try
    {
        string content = File.ReadAllText(filename);
        // 파일 처리...
    }
    catch (FileNotFoundException ex)
    {
        // 로그 기록
        Console.WriteLine($"[로그] 파일을 찾을 수 없음: {filename}");
        
        // 예외를 상위로 전파
        throw;
    }
    catch (IOException ex)
    {
        Console.WriteLine($"[로그] I/O 오류 발생: {ex.Message}");
        throw;
    }
}

try
{
    ProcessFile("missing.txt");
}
catch (FileNotFoundException)
{
    Console.WriteLine("상위: 파일이 없습니다. 기본 파일을 사용합니다.");
}

// 출력:
// [로그] 파일을 찾을 수 없음: missing.txt
// 상위: 파일이 없습니다. 기본 파일을 사용합니다.
```

### 조건부 throw 표현식

C# 7.0부터 `throw`를 표현식으로 사용할 수 있어, null 병합 연산자나 조건 연산자와 함께 사용할 수 있습니다:

```csharp
class Person
{
    public string Name { get; }
    
    public Person(string name)
    {
        // name이 null이면 예외를 던짐
        Name = name ?? throw new ArgumentNullException(nameof(name));
    }
}

// 조건 연산자와 함께 사용
int GetValue(bool condition)
{
    return condition ? 100 : throw new InvalidOperationException("조건이 거짓입니다.");
}

try
{
    var person = new Person(null);
}
catch (ArgumentNullException ex)
{
    Console.WriteLine($"오류: {ex.Message}");
}

// 출력:
// 오류: Value cannot be null. (Parameter 'name')
```

---

## 12.4 사용자 정의 예외

표준 예외 타입이 애플리케이션의 특정 오류 상황을 충분히 표현하지 못할 때, 사용자 정의 예외(Custom Exception)를 만들 수 있습니다. 사용자 정의 예외는 `Exception` 클래스를 상속하여 생성하며, 애플리케이션의 도메인에 특화된 의미 있는 예외를 정의할 수 있게 합니다.

**도메인 주도 설계와 예외:**

Eric Evans의 **도메인 주도 설계(Domain-Driven Design, DDD)**에서 예외는 도메인 모델의 일부입니다. 사용자 정의 예외는 **유비쿼터스 언어(Ubiquitous Language)**를 코드에 반영하며, 비즈니스 규칙 위반을 명확하게 표현합니다. `InsufficientFundsException`은 단순한 `InvalidOperationException`보다 은행 도메인의 개념을 훨씬 명확히 전달합니다.

**예외 설계의 SOLID 원칙:**

1. **단일 책임 원칙(SRP)**: 각 예외는 하나의 명확한 오류 조건을 나타냅니다.
2. **개방-폐쇄 원칙(OCP)**: 예외 계층을 통해 확장 가능하지만 기존 코드 수정은 불필요합니다.
3. **리스코프 치환 원칙(LSP)**: 파생 예외는 기본 예외로 치환 가능합니다.
4. **인터페이스 분리 원칙(ISP)**: 예외는 필요한 정보만 포함합니다.
5. **의존성 역전 원칙(DIP)**: 상위 계층은 구체적 예외가 아닌 추상 예외에 의존할 수 있습니다.

### 사용자 정의 예외 생성 규칙

**명명 규칙(Naming Conventions):**
- 예외 클래스 이름은 반드시 `Exception`으로 끝나야 합니다.
- 의미 있고 명확한 이름을 사용합니다: `InsufficientFundsException`, `InvalidEmailFormatException`.
- 예외 이름은 **발생한 문제**를 설명해야 하며, 해결 방법이 아닙니다.

**권장 구현 패턴(Best Practice Pattern):**

사용자 정의 예외는 세 가지 생성자를 구현하는 것이 .NET Framework Design Guidelines의 권장사항입니다:

```csharp
[Serializable]
public class CustomException : Exception
{
    // 1. 기본 생성자 - 메시지 없이 예외 생성
    public CustomException()
    {
    }
    
    // 2. 메시지를 받는 생성자 - 가장 많이 사용됨
    public CustomException(string message) : base(message)
    {
    }
    
    // 3. 메시지와 내부 예외를 받는 생성자 - 예외 체이닝용
    public CustomException(string message, Exception innerException) 
        : base(message, innerException)
    {
    }
    
    // 4. 직렬화 생성자 - .NET Remoting/WCF/분산 시스템용 (선택적)
    protected CustomException(SerializationInfo info, StreamingContext context)
        : base(info, context)
    {
    }
}
```

**[Serializable] 특성의 중요성:**

분산 시스템에서 예외는 네트워크를 통해 전송되어야 할 수 있습니다. `[Serializable]` 특성은 예외를 **직렬화(Serialization)** 가능하게 만들어, .NET Remoting, WCF, 또는 다른 분산 통신 메커니즘에서 사용할 수 있게 합니다. 현대의 마이크로서비스 아키텍처에서도 예외 정보를 JSON 등으로 직렬화해야 할 때가 있으므로 중요한 고려사항입니다.

**InnerException과 예외 체이닝:**

`InnerException`은 **예외 체이닝(Exception Chaining)**을 구현합니다. 이는 저수준 예외를 고수준 예외로 래핑할 때 원인을 보존하는 메커니즘입니다. 이는 **추상화 계층(Abstraction Layers)** 간의 예외 변환에서 필수적입니다.

```csharp
try
{
    DatabaseOperation();  // SqlException 발생
}
catch (SqlException ex)
{
    // 저수준 DB 예외를 고수준 비즈니스 예외로 변환
    throw new DataAccessException("데이터 조회 실패", ex);
}
```

이를 통해 상위 계층은 저수준 구현 세부사항(SQL)에 의존하지 않으면서도, 필요시 InnerException을 통해 원인을 추적할 수 있습니다.

### 실용적인 사용자 정의 예외

**은행 계좌 예외:**

```csharp
// 잔액 부족 예외
class InsufficientFundsException : Exception
{
    public decimal CurrentBalance { get; }
    public decimal RequestedAmount { get; }
    
    public InsufficientFundsException(decimal currentBalance, decimal requestedAmount)
        : base($"잔액이 부족합니다. 현재: {currentBalance}원, 요청: {requestedAmount}원")
    {
        CurrentBalance = currentBalance;
        RequestedAmount = requestedAmount;
    }
}

// 은행 계좌 클래스
class BankAccount
{
    public decimal Balance { get; private set; }
    
    public BankAccount(decimal initialBalance)
    {
        Balance = initialBalance;
    }
    
    public void Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("출금 금액은 0보다 커야 합니다.", nameof(amount));
        }
        
        if (amount > Balance)
        {
            throw new InsufficientFundsException(Balance, amount);
        }
        
        Balance -= amount;
        Console.WriteLine($"{amount}원 출금 완료. 잔액: {Balance}원");
    }
}

// 사용 예제
var account = new BankAccount(10000);

try
{
    account.Withdraw(5000);   // 성공
    account.Withdraw(7000);   // 실패
}
catch (InsufficientFundsException ex)
{
    Console.WriteLine($"오류: {ex.Message}");
    Console.WriteLine($"부족한 금액: {ex.RequestedAmount - ex.CurrentBalance}원");
}

// 출력:
// 5000원 출금 완료. 잔액: 5000원
// 오류: 잔액이 부족합니다. 현재: 5000원, 요청: 7000원
// 부족한 금액: 2000원
```

**유효성 검사 예외:**

```csharp
class ValidationException : Exception
{
    public string PropertyName { get; }
    public object InvalidValue { get; }
    
    public ValidationException(string propertyName, object invalidValue, string message)
        : base(message)
    {
        PropertyName = propertyName;
        InvalidValue = invalidValue;
    }
}

class User
{
    private string email;
    
    public string Email
    {
        get => email;
        set
        {
            if (!value.Contains("@"))
            {
                throw new ValidationException(
                    nameof(Email),
                    value,
                    "이메일 주소에 @ 기호가 포함되어야 합니다."
                );
            }
            email = value;
        }
    }
}

var user = new User();

try
{
    user.Email = "invalid-email";
}
catch (ValidationException ex)
{
    Console.WriteLine($"검증 오류: {ex.Message}");
    Console.WriteLine($"속성: {ex.PropertyName}");
    Console.WriteLine($"잘못된 값: {ex.InvalidValue}");
}

// 출력:
// 검증 오류: 이메일 주소에 @ 기호가 포함되어야 합니다.
// 속성: Email
// 잘못된 값: invalid-email
```

**비즈니스 규칙 예외:**

```csharp
class BusinessRuleException : Exception
{
    public string RuleCode { get; }
    
    public BusinessRuleException(string ruleCode, string message) : base(message)
    {
        RuleCode = ruleCode;
    }
}

class OrderProcessor
{
    public void ProcessOrder(int quantity, bool isWeekend)
    {
        if (quantity > 100)
        {
            throw new BusinessRuleException(
                "MAX_QUANTITY",
                "한 번에 최대 100개까지만 주문할 수 있습니다."
            );
        }
        
        if (isWeekend)
        {
            throw new BusinessRuleException(
                "WEEKEND_CLOSED",
                "주말에는 주문을 처리할 수 없습니다."
            );
        }
        
        Console.WriteLine($"{quantity}개 주문이 처리되었습니다.");
    }
}

var processor = new OrderProcessor();

try
{
    processor.ProcessOrder(150, false);
}
catch (BusinessRuleException ex)
{
    Console.WriteLine($"[{ex.RuleCode}] {ex.Message}");
}

// 출력:
// [MAX_QUANTITY] 한 번에 최대 100개까지만 주문할 수 있습니다.
```

---

## 12.5 예외 필터 (Exception Filters)

C# 6.0에서 도입된 **예외 필터(Exception Filters)**는 `when` 키워드를 사용하여 예외를 잡을 때 추가 조건을 지정할 수 있게 해줍니다. 이를 통해 같은 타입의 예외라도 특정 조건을 만족하는 경우에만 처리할 수 있어, 더욱 세밀한 예외 처리가 가능합니다. 이는 함수형 프로그래밍의 **가드 절(Guard Clauses)**와 패턴 매칭의 개념을 예외 처리에 도입한 것입니다.

**예외 필터의 역사적 배경:**

예외 필터는 VB.NET에 먼저 존재했던 기능이 C# 6.0에서 추가된 것입니다. 더 거슬러 올라가면, **Common Intermediate Language (CIL)**는 처음부터 예외 필터를 지원했습니다. C#이 뒤늦게 이 기능을 추가한 것은 Anders Hejlsberg가 "언어를 단순하게 유지하려는" 철학 때문이었으나, 실용성이 입증되면서 결국 채택되었습니다.

**선언적 예외 처리(Declarative Exception Handling):**

예외 필터는 명령형(Imperative) 스타일의 if 문을 선언형(Declarative) 패턴 매칭으로 변환합니다. 이는 코드의 **의도(Intent)**를 더 명확히 표현하며, **관심사의 분리(Separation of Concerns)**를 개선합니다.

```csharp
// 명령형 스타일 (예외 필터 없이)
try
{
    MakeHttpRequest();
}
catch (HttpException ex)
{
    if (ex.StatusCode == 404)
    {
        Console.WriteLine("404 처리");
    }
    else if (ex.StatusCode >= 500)
    {
        Console.WriteLine("서버 오류 처리");
    }
    else
    {
        throw;  // 다른 상태 코드는 재발생
    }
}

// 선언형 스타일 (예외 필터 사용)
try
{
    MakeHttpRequest();
}
catch (HttpException ex) when (ex.StatusCode == 404)
{
    Console.WriteLine("404 처리");
}
catch (HttpException ex) when (ex.StatusCode >= 500)
{
    Console.WriteLine("서버 오류 처리");
}
// 다른 경우는 자동으로 전파됨
```

### 기본 문법

```csharp
try
{
    // 코드
}
catch (예외타입 변수명) when (조건식)
{
    // 조건이 true일 때만 실행
}
```

조건식은 `bool`을 반환해야 하며, 조건이 `false`이면 해당 `catch` 블록은 건너뛰고 다음 `catch` 블록을 검사합니다. 이는 **단락 평가(Short-circuit Evaluation)**와 유사한 메커니즘입니다.

**예외 필터의 평가 시점과 Two-Pass Exception Handling:**

중요한 점은 예외 필터가 **스택 해제 전에** 평가된다는 것입니다. CLR의 Two-Pass 예외 처리 모델에서:

1. **First Pass**: 적절한 catch 블록을 찾기 위해 스택을 순회하며 when 절을 평가합니다.
2. **Second Pass**: 매칭되는 catch 블록을 찾으면 스택을 실제로 해제합니다.

이는 디버깅 시 매우 중요한 특성입니다. 예외 필터가 실행되는 순간에는 **호출 스택이 아직 보존**되어 있어, 디버거로 전체 스택을 검사할 수 있습니다. 반면 catch 블록 내부의 if 문은 스택이 이미 해제된 후에 실행되므로 원본 컨텍스트를 잃습니다.

**예외 필터와 부작용(Side Effects):**

예외 필터 조건식 내에서 부작용을 발생시킬 수 있습니다. 이는 **로깅(Logging)**이나 **원격 측정(Telemetry)** 수집에 매우 유용합니다. 그러나 부작용이 있는 조건식은 신중하게 사용해야 하며, **멱등성(Idempotency)**을 유지해야 합니다.

```csharp
bool LogAndReturnFalse(Exception ex)
{
    Logger.Log($"예외 필터에서 로깅: {ex.Message}");
    Telemetry.TrackException(ex);
    return false;  // false를 반환하여 이 catch 블록은 실행하지 않음
}

try
{
    DangerousOperation();
}
catch (Exception ex) when (LogAndReturnFalse(ex))
{
    // 절대 실행되지 않지만, 모든 예외가 로깅됨
}
catch (Exception ex)
{
    // 실제 처리
}
```

이 패턴은 **관측성(Observability)** 패턴의 일부로, 예외를 처리하지 않으면서도 모든 예외를 기록할 수 있게 합니다.

### 기본 예제

**특정 오류 코드만 처리:**

```csharp
void ProcessData(int code)
{
    if (code < 0)
    {
        throw new ArgumentException($"잘못된 코드: {code}");
    }
}

try
{
    ProcessData(-5);
}
catch (ArgumentException ex) when (ex.Message.Contains("코드: -5"))
{
    Console.WriteLine("특정 오류 코드 -5가 감지되었습니다.");
}
catch (ArgumentException ex)
{
    Console.WriteLine($"일반 인수 오류: {ex.Message}");
}

// 출력:
// 특정 오류 코드 -5가 감지되었습니다.
```

### HTTP 상태 코드 필터링

```csharp
class HttpException : Exception
{
    public int StatusCode { get; }
    
    public HttpException(int statusCode, string message) : base(message)
    {
        StatusCode = statusCode;
    }
}

void MakeHttpRequest(int statusCode)
{
    throw new HttpException(statusCode, $"HTTP 오류 {statusCode}");
}

try
{
    MakeHttpRequest(404);
}
catch (HttpException ex) when (ex.StatusCode == 404)
{
    Console.WriteLine("404 오류: 리소스를 찾을 수 없습니다.");
}
catch (HttpException ex) when (ex.StatusCode >= 500)
{
    Console.WriteLine($"서버 오류 {ex.StatusCode}: 서버에 문제가 있습니다.");
}
catch (HttpException ex)
{
    Console.WriteLine($"HTTP 오류 {ex.StatusCode}: {ex.Message}");
}

// 출력:
// 404 오류: 리소스를 찾을 수 없습니다.
```

### 재시도 로직과 함께 사용

```csharp
int retryCount = 0;
int maxRetries = 3;

while (retryCount < maxRetries)
{
    try
    {
        Console.WriteLine($"시도 {retryCount + 1}");
        
        // 네트워크 작업 시뮬레이션
        Random random = new Random();
        if (random.Next(3) != 0)
        {
            throw new InvalidOperationException("네트워크 오류");
        }
        
        Console.WriteLine("작업 성공!");
        break;
    }
    catch (InvalidOperationException ex) when (retryCount < maxRetries - 1)
    {
        retryCount++;
        Console.WriteLine($"재시도 중... ({ex.Message})");
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine($"최대 재시도 횟수 초과: {ex.Message}");
        break;
    }
}
```

### 로깅과 예외 필터

예외 필터의 강력한 기능 중 하나는 조건식 내에서 부작용(side effect)을 발생시킬 수 있다는 점입니다. 이를 활용하여 로깅을 수행할 수 있습니다:

```csharp
bool LogException(Exception ex)
{
    Console.WriteLine($"[로그] 예외 발생: {ex.GetType().Name} - {ex.Message}");
    return false;  // false를 반환하여 이 catch 블록을 건너뜀
}

try
{
    throw new InvalidOperationException("작업 실패");
}
catch (Exception ex) when (LogException(ex))
{
    // LogException이 false를 반환하므로 실행되지 않음
}
catch (Exception ex)
{
    Console.WriteLine("예외 처리");
}

// 출력:
// [로그] 예외 발생: InvalidOperationException - 작업 실패
// 예외 처리
```

### 속성 기반 필터링

```csharp
class DatabaseException : Exception
{
    public string TableName { get; }
    public bool IsTransient { get; }
    
    public DatabaseException(string tableName, bool isTransient, string message)
        : base(message)
    {
        TableName = tableName;
        IsTransient = isTransient;
    }
}

try
{
    throw new DatabaseException("Users", true, "연결 시간 초과");
}
catch (DatabaseException ex) when (ex.IsTransient)
{
    Console.WriteLine($"일시적 오류 발생 ({ex.TableName} 테이블). 재시도 가능합니다.");
}
catch (DatabaseException ex)
{
    Console.WriteLine($"영구적 오류 발생 ({ex.TableName} 테이블). 수동 개입이 필요합니다.");
}

// 출력:
// 일시적 오류 발생 (Users 테이블). 재시도 가능합니다.
```

### 복잡한 조건 결합

```csharp
class PaymentException : Exception
{
    public decimal Amount { get; }
    public string PaymentMethod { get; }
    
    public PaymentException(decimal amount, string paymentMethod, string message)
        : base(message)
    {
        Amount = amount;
        PaymentMethod = paymentMethod;
    }
}

try
{
    throw new PaymentException(1500, "신용카드", "결제 거부됨");
}
catch (PaymentException ex) when (ex.Amount < 1000 && ex.PaymentMethod == "신용카드")
{
    Console.WriteLine("소액 신용카드 결제 실패");
}
catch (PaymentException ex) when (ex.Amount >= 1000 && ex.PaymentMethod == "신용카드")
{
    Console.WriteLine($"고액 신용카드 결제 실패: {ex.Amount}원");
    Console.WriteLine("대체 결제 수단을 제안합니다.");
}
catch (PaymentException ex)
{
    Console.WriteLine($"{ex.PaymentMethod} 결제 오류: {ex.Message}");
}

// 출력:
// 고액 신용카드 결제 실패: 1500원
// 대체 결제 수단을 제안합니다.
```

---

## 마무리

이 장에서는 C#의 예외 처리 메커니즘을 깊이 있게 학습했습니다. 예외 처리는 단순한 오류 보고 수단을 넘어서, 소프트웨어의 **견고성(Robustness)**, **신뢰성(Reliability)**, **유지보수성(Maintainability)**을 결정하는 핵심 요소입니다.

**학습 정리:**

✅ **예외의 이론적 기반**: 예외가 무엇이며, 역사적 발전 과정과 소프트웨어 공학 원칙(관심사의 분리, 방어적 프로그래밍, 계약에 의한 설계)을 통해 그 필요성을 이해했습니다.

✅ **try-catch-finally의 메커니즘**: CLR의 Two-Pass 예외 처리 모델, 스택 해제(Stack Unwinding), RAII 패턴, 그리고 결정론적 리소스 해제 방법을 학습했습니다.

✅ **예외 던지기의 전략**: 실패 빠르기(Fail-Fast) 원칙, 적절한 예외 타입 선택, throw vs throw ex의 스택 추적 보존 메커니즘, 그리고 ExceptionDispatchInfo를 통한 고급 재발생 기법을 익혔습니다.

✅ **사용자 정의 예외 설계**: 도메인 주도 설계(DDD), SOLID 원칙, 예외 체이닝(Exception Chaining), 직렬화 지원을 포함한 포괄적인 예외 클래스 설계 방법을 배웠습니다.

✅ **예외 필터의 고급 활용**: VB.NET과 CIL의 역사적 배경, Two-Pass 모델과의 상호작용, 선언적 예외 처리, 로깅과 원격 측정을 위한 부작용 활용 패턴을 학습했습니다.

**예외 처리의 모범 사례 (Best Practices):**

**1. 예외는 예외적인 상황에서만 사용 (Exceptions for Exceptional Conditions)**

예외는 정상적인 제어 흐름에 사용하지 않습니다. 예외 처리는 성능 비용이 있으며(스택 추적 캡처, 스택 해제), 이는 O(n) 시간 복잡도를 가집니다. 예상 가능한 조건은 if 문이나 TryParse 같은 메서드를 사용합니다.

```csharp
// ❌ 나쁜 예: 예외를 제어 흐름으로 사용
try {
    int value = int.Parse(input);
} catch (FormatException) {
    value = 0;  // 기본값 사용
}

// ✅ 좋은 예: TryParse 사용
if (!int.TryParse(input, out int value)) {
    value = 0;  // 기본값 사용
}
```

**2. 구체적인 예외 타입 사용 (Specific Exception Types)**

`Exception`을 직접 잡지 말고, 처리할 수 있는 구체적인 예외만 잡습니다. 이는 **리스코프 치환 원칙(LSP)**과 **타입 안전성(Type Safety)**을 실현합니다.

```csharp
// ❌ 나쁜 예: 모든 예외를 무차별적으로 잡음
try {
    ProcessData();
} catch (Exception) {
    // OutOfMemoryException, StackOverflowException도 잡아버림
}

// ✅ 좋은 예: 복구 가능한 예외만 잡음
try {
    ProcessData();
} catch (IOException ex) {
    // I/O 오류만 처리
} catch (ArgumentException ex) {
    // 인수 오류만 처리
}
```

**3. 예외를 숨기지 말것 (Don't Swallow Exceptions)**

빈 catch 블록은 **Silent Failure**를 야기하며, 디버깅을 극도로 어렵게 만듭니다. 최소한 로깅은 수행해야 합니다.

```csharp
// ❌ 최악: 예외를 완전히 숨김
try {
    DangerousOperation();
} catch {
    // 아무것도 하지 않음 - 버그의 온상
}

// ⚠️ 여전히 문제: 로깅하지만 재발생하지 않음
try {
    DangerousOperation();
} catch (Exception ex) {
    Console.WriteLine(ex.Message);  // 콘솔은 프로덕션에서 볼 수 없음
}

// ✅ 좋은 예: 적절한 로깅과 재발생
try {
    DangerousOperation();
} catch (Exception ex) {
    Logger.Error("작업 실패", ex);
    throw;  // 상위 계층이 처리하도록 재발생
}
```

**4. finally나 using으로 리소스 정리 (Resource Cleanup)**

**RAII 패턴**을 구현하여 리소스 누수를 방지합니다. `using` 문은 컴파일러가 자동으로 try-finally로 변환하므로 권장됩니다.

```csharp
// ✅ using 문 사용 (권장)
using (var connection = new SqlConnection(connectionString))
{
    connection.Open();
    // 사용
}  // 자동으로 Dispose() 호출

// 또는 C# 8.0+ using 선언
using var connection = new SqlConnection(connectionString);
connection.Open();
// 스코프 끝에서 자동 정리
```

**5. throw vs throw ex 구분 (Preserve Stack Traces)**

스택 추적 보존을 위해 재발생 시 `throw`를 사용합니다. 이는 **디버깅 가능성(Debuggability)**과 **관측성(Observability)**에 필수적입니다.

**6. 예외 체이닝 사용 (Exception Chaining)**

`InnerException`을 통해 원인을 보존하며, 추상화 계층 간 예외를 변환합니다. 이는 **계층화된 아키텍처(Layered Architecture)**에서 중요합니다.

```csharp
try {
    LowLevelDatabaseOperation();
} catch (SqlException ex) {
    throw new DataAccessException("사용자 데이터 조회 실패", ex);
}
```

**예외 처리와 소프트웨어 아키텍처:**

**1. 회복력 패턴(Resilience Patterns):**
- **Retry Pattern**: 일시적 오류 재시도 (네트워크, DB 연결)
- **Circuit Breaker Pattern**: 반복적 실패 시 요청 차단
- **Timeout Pattern**: 무한 대기 방지
- **Fallback Pattern**: 실패 시 대안 제공

**2. 관측성(Observability):**
- 구조화된 로깅(Structured Logging)
- 분산 추적(Distributed Tracing)과 Correlation ID
- 메트릭 수집(Metrics Collection)
- Application Insights, Serilog, OpenTelemetry 활용

**3. 예외 안전성 보장(Exception Safety Guarantees):**
- **Basic Guarantee**: 리소스 누수 없음, 불변조건 유지
- **Strong Guarantee**: 연산 실패 시 상태 변경 없음 (원자성)
- **No-throw Guarantee**: 예외를 절대 던지지 않음 (소멸자, Dispose)

**다음 단계:**

13장에서는 파일 입출력을 다루며, **파일 시스템 I/O의 예외 처리**, **비동기 파일 작업의 예외 전파**, **using 문과 IAsyncDisposable**, 그리고 **JSON 직렬화 오류 처리**를 학습합니다. 파일 I/O는 예외 처리가 특히 중요한 영역으로, 권한 문제, 디스크 용량, 네트워크 공유 등 다양한 실패 시나리오를 다뤄야 합니다.

또한 14장 LINQ에서는 **지연 실행(Deferred Execution)과 예외**, **쿼리 체인의 예외 전파**, 그리고 17장 비동기 프로그래밍에서는 **Task의 예외 래핑**, **async/await의 예외 처리**, **AggregateException과 병렬 처리** 등 고급 예외 처리 패턴을 배우게 됩니다.

예외 처리는 모든 C# 프로그래밍의 기반이 되므로, 이 장의 내용을 철저히 이해하고 실습하기 바랍니다. 특히 **프로덕션 환경**에서 예외 처리는 시스템의 안정성과 직결되므로, 방어적 프로그래밍과 회복력 패턴을 항상 염두에 두어야 합니다.
