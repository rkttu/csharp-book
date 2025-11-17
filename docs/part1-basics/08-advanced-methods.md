# 8장. 고급 메서드 활용

프로그래밍에서 메서드는 단순히 코드를 재사용하는 수단을 넘어, 로직을 추상화하고 표현력을 높이는 강력한 도구입니다. 7장에서 메서드의 기초를 다루었다면, 이 장에서는 C#의 고급 메서드 기능들을 학습하여 더욱 간결하고 표현력 있는 코드를 작성하는 방법을 익히게 됩니다. 이러한 고급 기능들은 현대 소프트웨어 개발의 핵심 패러다임인 함수형 프로그래밍(Functional Programming)과 선언적 프로그래밍(Declarative Programming)의 개념을 C#에 도입한 것으로, 코드의 간결성과 유지보수성을 획기적으로 향상시킵니다.

람다 식(Lambda Expressions)은 익명 함수를 간결하게 표현하는 현대적인 문법으로, LINQ와 함수형 프로그래밍의 핵심입니다. 1930년대 수학자 알론조 처치(Alonzo Church)의 람다 계산법(Lambda Calculus)에서 유래한 이 개념은, 21세기 프로그래밍 언어들의 필수 요소가 되었습니다. 확장 메서드(Extension Methods)는 기존 타입에 새로운 메서드를 추가하는 것처럼 보이게 하는 강력한 기능으로, 개방-폐쇄 원칙(Open-Closed Principle)을 우아하게 구현하여 코드의 확장성과 가독성을 크게 향상시킵니다. 재귀 함수(Recursive Functions)는 자기 자신을 호출하는 메서드로, 수학적 귀납법과 분할 정복(Divide and Conquer) 알고리즘의 프로그래밍적 구현이며, 복잡한 문제를 우아하게 해결하는 강력한 기법입니다.

이 장에서 다룰 내용들은 단순한 문법적 편의성을 넘어서, 프로그래밍 패러다임의 변화를 반영합니다. 명령형 프로그래밍(Imperative Programming)에서 선언적 프로그래밍으로, 절차 중심에서 데이터 변환 중심으로의 사고 전환을 가능하게 하는 도구들입니다. 이러한 기법들을 익히면, 더 적은 코드로 더 명확한 의도를 표현할 수 있으며, 버그 발생 가능성을 줄이고 테스트 가능성을 높일 수 있습니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 다음과 같은 C# 고급 메서드 활용 기법을 체계적으로 학습하게 됩니다:

- **람다 식의 이해와 활용**: 간결한 익명 함수 표현 방식인 람다 식의 문법을 익히고, `Func`와 `Action` 델리게이트를 통해 메서드를 일급 객체(First-class Citizen)로 다루는 함수형 프로그래밍의 핵심 개념을 이해합니다. 람다 계산법(Lambda Calculus)의 이론적 배경부터 실무에서의 활용까지, LINQ와 결합된 강력한 데이터 처리 기법을 습득합니다.

- **확장 메서드를 통한 코드 확장성**: 개방-폐쇄 원칙(Open-Closed Principle)을 구현하는 확장 메서드의 작성법과 활용 패턴을 학습합니다. 기존 타입을 수정하지 않고도 새로운 기능을 추가하는 방법을 익히며, sealed 클래스나 외부 라이브러리의 타입도 확장할 수 있는 강력한 메커니즘을 이해합니다. 메서드 체이닝(Method Chaining)과 Fluent API 패턴을 통해 가독성 높은 코드를 작성하는 기법을 배웁니다.

- **재귀 함수의 원리와 적용**: 수학적 귀납법(Mathematical Induction)에 기반한 재귀적 사고방식을 익히고, 복잡한 문제를 더 작은 하위 문제로 분할하여 해결하는 분할 정복(Divide and Conquer) 전략을 이해합니다. 기저 조건(Base Case)과 재귀 호출(Recursive Call)의 설계 원칙, 꼬리 재귀 최적화(Tail Recursion Optimization), 그리고 스택 오버플로우를 방지하는 실용적인 재귀 함수 작성법을 배웁니다.

**학습 목표:**
- 함수형 프로그래밍 패러다임의 핵심 개념 이해
- 람다 식과 델리게이트를 활용한 고차 함수(Higher-order Functions) 작성
- 확장 메서드를 통한 타입 시스템의 안전한 확장
- 재귀적 문제 해결 능력 배양과 알고리즘적 사고 향상
- LINQ와 결합된 선언적 프로그래밍 스타일 습득

---

## 8.1 람다 식 (Lambda Expressions)

람다 식은 익명 메서드를 간결하게 표현하는 현대적인 문법으로, C# 3.0에서 LINQ(Language Integrated Query)와 함께 2007년에 도입되었습니다. "람다(Lambda)"라는 이름은 1930년대 수학자 알론조 처치(Alonzo Church)가 개발한 람다 계산법(Lambda Calculus, λ-calculus)에서 유래했으며, 이는 현대 함수형 프로그래밍의 이론적 기반이 되었습니다. 람다 계산법은 튜링 머신(Turing Machine)과 함께 계산 가능성(Computability)을 정의하는 두 가지 동등한 모델 중 하나로, 컴퓨터 과학의 근간을 이룹니다.

**함수형 프로그래밍과 람다 식의 철학:**

람다 식은 메서드를 값처럼 다룰 수 있게 해주어, 메서드를 다른 메서드의 인자로 전달하거나 변수에 할당할 수 있게 합니다. 이는 함수형 프로그래밍(Functional Programming)의 핵심 개념인 "일급 함수(First-class Functions)"를 C#에서 구현하는 방법입니다. 일급 함수란 함수를 다른 데이터 타입처럼 자유롭게 다룰 수 있는 것을 의미하며, 이를 통해 고차 함수(Higher-order Functions), 커링(Currying), 부분 적용(Partial Application) 같은 강력한 프로그래밍 기법이 가능해집니다.

함수형 프로그래밍의 주요 원칙들은 다음과 같습니다:
1. **불변성(Immutability)**: 데이터를 변경하지 않고 새로운 데이터를 생성
2. **순수 함수(Pure Functions)**: 부작용(Side Effects)이 없는 함수
3. **선언적 프로그래밍(Declarative Programming)**: "무엇을" 할지 명시하고, "어떻게"는 추상화
4. **함수 합성(Function Composition)**: 작은 함수들을 조합하여 복잡한 기능 구현

람다 식은 이러한 함수형 프로그래밍 원칙을 C#에서 실현하는 핵심 도구입니다.

**람다 식의 주요 활용 사례:**
- **LINQ 쿼리**: 컬렉션을 필터링, 변환, 정렬할 때의 핵심 메커니즘
- **이벤트 핸들러**: UI 이벤트 처리를 간결하게 표현하여 코드 가독성 향상
- **비동기 프로그래밍**: Task 기반 비동기 패턴(TAP)에서 콜백 함수를 간단히 작성
- **함수형 프로그래밍**: 고차 함수(Higher-order Functions)를 활용한 추상화 수준 향상
- **전략 패턴(Strategy Pattern)**: 알고리즘을 런타임에 선택하여 실행

**역사적 맥락과 발전:**

C#의 람다 식은 다음과 같은 진화 과정을 거쳤습니다:
- **C# 1.0 (2002)**: 명명된 메서드만 사용 가능, 델리게이트에 메서드 할당
- **C# 2.0 (2005)**: 익명 메서드(Anonymous Methods) 도입, `delegate` 키워드 사용
- **C# 3.0 (2007)**: 람다 식 도입, `=>` 연산자, LINQ와의 완벽한 통합
- **C# 7.0 (2017)**: 표현식 본문 멤버(Expression-bodied Members) 확장
- **C# 9.0 (2020)**: 자연 타입(Natural Type) 람다, 반환 타입 추론 개선
- **C# 10.0 (2021)**: 람다 개선(Lambda Improvements), 특성(Attributes) 지원

이러한 발전은 C#이 명령형 언어에서 다중 패러다임 언어(Multi-paradigm Language)로 진화하는 과정을 보여줍니다.

### 8.1.1 람다 식 기본 문법

람다 식은 `=>` 연산자(람다 연산자 또는 "goes to" 연산자)를 사용하여 매개변수와 식을 구분합니다. 왼쪽에는 입력 매개변수를, 오른쪽에는 실행할 식이나 문장 블록을 작성합니다.

**기본 구조:**

```
(매개변수) => 식
(매개변수) => { 문장들; return 값; }
```

**람다 식의 역사적 발전:**

람다 식 이전에는 익명 메서드(Anonymous Methods)를 사용했습니다. 다음 예제는 같은 기능을 시대별로 어떻게 표현했는지 보여줍니다:

```csharp
// C# 1.0: 명명된 메서드
int Add(int x, int y)
{
    return x + y;
}

// C# 2.0: 익명 메서드
Func<int, int, int> add1 = delegate(int x, int y)
{
    return x + y;
};

// C# 3.0: 람다 식 (문장 블록)
Func<int, int, int> add2 = (int x, int y) =>
{
    return x + y;
};

// C# 3.0: 람다 식 (식 본문) - 가장 간결
Func<int, int, int> add3 = (x, y) => x + y;

// 사용
Console.WriteLine(add3(5, 3));  // 출력: 8
```

**단순한 람다 식 예제:**

```csharp
// 매개변수가 하나인 람다 식
Func<int, int> square = x => x * x;
Console.WriteLine(square(5));  // 출력: 25

// 매개변수가 두 개인 람다 식
Func<int, int, int> multiply = (a, b) => a * b;
Console.WriteLine(multiply(3, 4));  // 출력: 12

// 매개변수가 없는 람다 식
Func<string> getMessage = () => "Hello, Lambda!";
Console.WriteLine(getMessage());  // 출력: Hello, Lambda!
```

**타입 추론과 명시적 타입:**

C# 컴파일러는 문맥에서 매개변수의 타입을 추론할 수 있지만, 명시적으로 지정할 수도 있습니다:

```csharp
// 타입 추론 (권장)
Func<int, bool> isEven = x => x % 2 == 0;

// 명시적 타입 지정
Func<int, bool> isEven2 = (int x) => x % 2 == 0;

Console.WriteLine(isEven(4));   // 출력: True
Console.WriteLine(isEven(7));   // 출력: False
```

**문장 블록이 있는 람다 식:**

복잡한 로직이 필요한 경우 중괄호를 사용하여 여러 문장을 작성할 수 있습니다. 이 경우 반환값이 있다면 명시적으로 `return` 문을 사용해야 합니다. 이를 **문장 람다(Statement Lambda)**라고 하며, 단일 식으로 표현되는 **식 람다(Expression Lambda)**와 구분됩니다.

**식 람다 vs 문장 람다의 근본적 차이:**

식 람다는 단일 표현식으로 구성되어 컴파일러가 자동으로 반환값을 처리하지만, 문장 람다는 명시적인 `return` 문이 필요합니다. 이는 메서드 본문의 복잡도에 따라 선택할 수 있으며, 간단한 변환이나 조건은 식 람다로, 복잡한 로직은 문장 람다로 작성하는 것이 일반적입니다.

```csharp
Func<int, int, string> compareNumbers = (a, b) =>
{
    if (a > b)
        return $"{a}가 더 큽니다";
    else if (a < b)
        return $"{b}가 더 큽니다";
    else
        return "두 수가 같습니다";
};

Console.WriteLine(compareNumbers(10, 5));   // 출력: 10가 더 큽니다
Console.WriteLine(compareNumbers(3, 8));    // 출력: 8가 더 큽니다
Console.WriteLine(compareNumbers(7, 7));    // 출력: 두 수가 같습니다
```

**람다 식의 컴파일 메커니즘:**

흥미롭게도, 람다 식은 컴파일 시점에 두 가지 방식 중 하나로 변환될 수 있습니다:

1. **델리게이트로 변환**: 메서드 포인터로 컴파일되어 직접 호출 가능
2. **표현식 트리로 변환**: 코드 자체가 데이터 구조로 표현되어 런타임에 분석/변환 가능 (LINQ to SQL 등에서 사용)

```csharp
// 델리게이트로 컴파일
Func<int, int> square = x => x * x;

// 표현식 트리로 컴파일 (LINQ에서 SQL로 변환 가능)
Expression<Func<int, int>> squareExpression = x => x * x;
```

이러한 이중성이 LINQ가 메모리 상의 객체(LINQ to Objects)뿐만 아니라 데이터베이스 쿼리(LINQ to SQL, Entity Framework)에서도 동작할 수 있게 하는 핵심 메커니즘입니다.

**배열과 리스트에서의 람다 식 활용:**

람다 식은 컬렉션을 다룰 때 가장 빛을 발합니다. LINQ(Language Integrated Query) 메서드와 함께 사용하면 매우 강력한 데이터 처리가 가능하며, 이는 SQL의 선언적 쿼리 스타일을 C# 코드에 적용한 것입니다.

**LINQ 메서드 체이닝의 지연 실행(Lazy Evaluation):**

LINQ의 중요한 특성 중 하나는 대부분의 쿼리 연산자가 지연 실행(Deferred Execution)을 사용한다는 점입니다. 이는 쿼리를 정의하는 시점이 아닌, 실제로 결과를 사용하는 시점에 실행된다는 의미입니다. 이러한 특성 덕분에:
1. 불필요한 계산을 피할 수 있어 성능이 향상됩니다
2. 쿼리를 조합하여 복잡한 데이터 변환 파이프라인을 구축할 수 있습니다
3. 데이터베이스 쿼리의 경우, 여러 연산을 하나의 최적화된 SQL 쿼리로 변환할 수 있습니다

단, `ToList()`, `ToArray()`, `Count()`, `Sum()` 같은 집계 연산은 즉시 실행(Immediate Execution)되어 결과를 반환합니다.

```csharp
// 숫자 리스트
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Where: 짝수만 필터링 (지연 실행)
var evenNumbers = numbers.Where(n => n % 2 == 0);
Console.WriteLine("짝수: " + string.Join(", ", evenNumbers));
// 출력: 짝수: 2, 4, 6, 8, 10

// Select: 각 숫자를 제곱 (지연 실행)
var squares = numbers.Select(n => n * n);
Console.WriteLine("제곱: " + string.Join(", ", squares));
// 출력: 제곱: 1, 4, 9, 16, 25, 36, 49, 64, 81, 100

// Where + Select 조합: 홀수만 선택하여 제곱 (메서드 체이닝)
var oddSquares = numbers.Where(n => n % 2 != 0).Select(n => n * n);
Console.WriteLine("홀수의 제곱: " + string.Join(", ", oddSquares));
// 출력: 홀수의 제곱: 1, 9, 25, 49, 81
```

**LINQ의 함수형 프로그래밍 원칙:**

LINQ는 함수형 프로그래밍의 핵심 개념들을 구현합니다:
- **Map**: `Select` - 각 요소를 변환
- **Filter**: `Where` - 조건에 맞는 요소만 선택
- **Reduce**: `Aggregate` - 요소들을 단일 값으로 축약
- **Fold**: `Sum`, `Average`, `Max`, `Min` - 특정 집계 연산

이러한 연산들은 부작용(Side Effects)이 없으며, 원본 컬렉션을 변경하지 않고 새로운 시퀀스를 생성합니다(불변성 원칙).

**문자열 리스트 처리:**

```csharp
List<string> names = new List<string> { "Alice", "Bob", "Charlie", "David", "Eve" };

// 이름 길이가 4자 이상인 것만 필터링
var longNames = names.Where(name => name.Length >= 4);
Console.WriteLine("긴 이름: " + string.Join(", ", longNames));
// 출력: 긴 이름: Alice, Charlie, David

// 모든 이름을 대문자로 변환
var upperNames = names.Select(name => name.ToUpper());
Console.WriteLine("대문자: " + string.Join(", ", upperNames));
// 출력: 대문자: ALICE, BOB, CHARLIE, DAVID, EVE

// 'e'를 포함하는 이름만 선택
var namesWithE = names.Where(name => name.Contains('e', StringComparison.OrdinalIgnoreCase));
Console.WriteLine("'e' 포함: " + string.Join(", ", namesWithE));
// 출력: 'e' 포함: Alice, Charlie, Eve
```

**정렬에서의 람다 식:**

```csharp
List<int> numbers = new List<int> { 5, 2, 8, 1, 9, 3 };

// OrderBy: 오름차순 정렬 (사실 여기서는 람다가 단순히 값 자체를 반환)
var ascending = numbers.OrderBy(n => n);
Console.WriteLine("오름차순: " + string.Join(", ", ascending));
// 출력: 오름차순: 1, 2, 3, 5, 8, 9

// OrderByDescending: 내림차순 정렬
var descending = numbers.OrderByDescending(n => n);
Console.WriteLine("내림차순: " + string.Join(", ", descending));
// 출력: 내림차순: 9, 8, 5, 3, 2, 1

// 복잡한 객체 정렬
var people = new[]
{
    new { Name = "Alice", Age = 30 },
    new { Name = "Bob", Age = 25 },
    new { Name = "Charlie", Age = 35 }
};

var sortedByAge = people.OrderBy(p => p.Age);
foreach (var person in sortedByAge)
{
    Console.WriteLine($"{person.Name}: {person.Age}세");
}
// 출력:
// Bob: 25세
// Alice: 30세
// Charlie: 35세
```

**집계 함수와 람다 식:**

```csharp
List<int> scores = new List<int> { 85, 92, 78, 95, 88 };

// Sum: 합계
int total = scores.Sum(s => s);
Console.WriteLine($"총점: {total}");  // 출력: 총점: 438

// Average: 평균
double average = scores.Average(s => s);
Console.WriteLine($"평균: {average}");  // 출력: 평균: 87.6

// Max, Min: 최대값, 최소값
int maxScore = scores.Max(s => s);
int minScore = scores.Min(s => s);
Console.WriteLine($"최고점: {maxScore}, 최저점: {minScore}");
// 출력: 최고점: 95, 최저점: 78

// Count: 조건을 만족하는 개수
int highScoresCount = scores.Count(s => s >= 90);
Console.WriteLine($"90점 이상: {highScoresCount}개");
// 출력: 90점 이상: 2개
```

### 8.1.2 Func와 Action 델리게이트

델리게이트(Delegate)는 메서드에 대한 참조를 저장하는 타입으로, C#에서 메서드를 일급 객체(First-class Citizen)로 다룰 수 있게 해주는 핵심 메커니즘입니다. `Func`와 `Action`은 .NET Framework에서 제공하는 제네릭 델리게이트로, 대부분의 상황에서 사용자 정의 델리게이트 없이도 메서드 참조를 다룰 수 있게 해줍니다.

**델리게이트의 이론적 배경과 역사:**

델리게이트는 C# 1.0(2002년)부터 존재했지만, 초기에는 각 메서드 시그니처마다 별도의 델리게이트 타입을 선언해야 했습니다. 이는 다음과 같은 번거로움이 있었습니다:

```csharp
// C# 1.0 스타일 - 매번 델리게이트 타입을 선언해야 함
public delegate int MathOperation(int a, int b);
public delegate bool Predicate(int value);
public delegate void Printer(string message);
```

C# 2.0(2005년)에서 제네릭(Generics)이 도입되면서, .NET Framework 3.5(2007년)부터 `Func`와 `Action` 같은 범용 제네릭 델리게이트가 도입되었고, 이는 코드의 재사용성과 간결성을 획기적으로 향상시켰습니다.

**델리게이트의 내부 구조:**

델리게이트는 다음 세 가지 핵심 정보를 캡슐화합니다:
1. **Target**: 호출할 메서드가 속한 객체 인스턴스 (정적 메서드의 경우 null)
2. **Method**: 호출할 메서드의 메타데이터 정보
3. **Invocation List**: 멀티캐스트 델리게이트의 경우, 여러 메서드의 목록

이러한 구조 덕분에 델리게이트는 타입 안전한(Type-safe) 함수 포인터로 동작하며, C/C++의 함수 포인터보다 훨씬 안전하고 강력합니다.

**Func 델리게이트:**

`Func`는 **반환값이 있는** 메서드를 나타내는 델리게이트입니다. 제네릭 타입 매개변수 중 마지막 타입은 반환 타입이고, 그 앞의 타입들은 입력 매개변수입니다. .NET Framework는 `Func<TResult>`부터 `Func<T1, T2, ..., T16, TResult>`까지 17가지 오버로드를 제공합니다.

**Func의 타입 시그니처:**
```
Func<TResult>                              // () => TResult
Func<T, TResult>                           // (T) => TResult  
Func<T1, T2, TResult>                      // (T1, T2) => TResult
...
Func<T1, T2, ..., T16, TResult>            // (T1, ..., T16) => TResult
```

```csharp
// Func의 기본 형식
// Func<반환타입>                    - 매개변수 없음
// Func<T1, 반환타입>                - 매개변수 1개
// Func<T1, T2, 반환타입>            - 매개변수 2개
// ... 최대 16개의 매개변수까지 지원

// 매개변수 없이 string 반환
Func<string> getGreeting = () => "안녕하세요!";
Console.WriteLine(getGreeting());  // 출력: 안녕하세요!

// int를 받아서 bool 반환
Func<int, bool> isPositive = x => x > 0;
Console.WriteLine(isPositive(5));   // 출력: True
Console.WriteLine(isPositive(-3));  // 출력: False

// 두 int를 받아서 int 반환
Func<int, int, int> add = (a, b) => a + b;
Console.WriteLine(add(10, 20));  // 출력: 30

// string과 int를 받아서 string 반환
Func<string, int, string> repeat = (str, count) =>
{
    string result = "";
    for (int i = 0; i < count; i++)
    {
        result += str;
    }
    return result;
};
Console.WriteLine(repeat("Hello ", 3));  // 출력: Hello Hello Hello
```

**Func 델리게이트의 공변성(Covariance):**

Func 델리게이트는 반환 타입에 대해 공변성(Covariance)을 지원합니다. 이는 더 파생된 타입을 반환하는 함수를 기본 타입을 반환하는 Func로 할당할 수 있음을 의미합니다:

```csharp
// 공변성 예제
Func<object> getObject = () => "문자열도 object입니다";  // string은 object의 하위 타입
object result = getObject();
```

**Action 델리게이트:**

`Action`은 **반환값이 없는(void)** 메서드를 나타내는 델리게이트입니다. 반환 타입이 없으므로 모든 타입 매개변수는 입력 매개변수입니다. .NET Framework는 `Action`부터 `Action<T1, T2, ..., T16>`까지 17가지 오버로드를 제공합니다.

**Action의 타입 시그니처:**
```
Action                                     // () => void
Action<T>                                  // (T) => void
Action<T1, T2>                             // (T1, T2) => void
...
Action<T1, T2, ..., T16>                   // (T1, ..., T16) => void
```

```csharp
// Action의 기본 형식
// Action                           - 매개변수 없음
// Action<T1>                       - 매개변수 1개
// Action<T1, T2>                   - 매개변수 2개
// ... 최대 16개의 매개변수까지 지원

// 매개변수 없음
Action greet = () => Console.WriteLine("안녕하세요!");
greet();  // 출력: 안녕하세요!

// string 매개변수 하나
Action<string> printMessage = message => Console.WriteLine($"메시지: {message}");
printMessage("테스트");  // 출력: 메시지: 테스트

// 두 개의 매개변수
Action<string, int> printInfo = (name, age) =>
{
    Console.WriteLine($"이름: {name}");
    Console.WriteLine($"나이: {age}세");
};
printInfo("김철수", 25);
// 출력:
// 이름: 김철수
// 나이: 25세
```

**Func와 Action의 실용적 활용:**

델리게이트를 사용하면 메서드를 매개변수로 전달하거나, 메서드를 동적으로 선택하여 실행할 수 있습니다:

```csharp
// 메서드를 매개변수로 받는 함수
void ProcessNumbers(List<int> numbers, Func<int, int> operation)
{
    foreach (int num in numbers)
    {
        int result = operation(num);
        Console.WriteLine($"{num} -> {result}");
    }
}

List<int> data = new List<int> { 1, 2, 3, 4, 5 };

// 제곱 연산
Console.WriteLine("제곱:");
ProcessNumbers(data, x => x * x);
// 출력:
// 1 -> 1
// 2 -> 4
// 3 -> 9
// 4 -> 16
// 5 -> 25

// 두 배 연산
Console.WriteLine("\n두 배:");
ProcessNumbers(data, x => x * 2);
// 출력:
// 1 -> 2
// 2 -> 4
// 3 -> 6
// 4 -> 8
// 5 -> 10
```

**조건부 실행:**

```csharp
// 사용자 입력에 따라 다른 연산 수행
void Calculate(int a, int b, string operation)
{
    Func<int, int, int> calculator = operation switch
    {
        "+" => (x, y) => x + y,
        "-" => (x, y) => x - y,
        "*" => (x, y) => x * y,
        "/" => (x, y) => x / y,
        _ => (x, y) => 0
    };
    
    int result = calculator(a, b);
    Console.WriteLine($"{a} {operation} {b} = {result}");
}

Calculate(10, 5, "+");  // 출력: 10 + 5 = 15
Calculate(10, 5, "-");  // 출력: 10 - 5 = 5
Calculate(10, 5, "*");  // 출력: 10 * 5 = 50
Calculate(10, 5, "/");  // 출력: 10 / 5 = 2
```

**콜백 패턴:**

델리게이트는 비동기 작업이나 이벤트 처리에서 콜백(callback) 패턴을 구현하는 데 자주 사용됩니다:

```csharp
void DownloadFile(string url, Action<string> onComplete, Action<string> onError)
{
    // 실제로는 비동기 다운로드가 일어남
    Console.WriteLine($"다운로드 중: {url}");
    
    // 성공 시뮬레이션
    if (url.StartsWith("http"))
    {
        onComplete("파일 다운로드 완료!");
    }
    else
    {
        onError("잘못된 URL입니다.");
    }
}

// 사용 예
DownloadFile(
    "https://example.com/file.zip",
    successMessage => Console.WriteLine($"성공: {successMessage}"),
    errorMessage => Console.WriteLine($"오류: {errorMessage}")
);
// 출력:
// 다운로드 중: https://example.com/file.zip
// 성공: 파일 다운로드 완료!
```

**메서드 체이닝과 Fluent API:**

`Func`와 `Action`을 활용하면 유창한(fluent) API 스타일의 코드를 작성할 수 있습니다:

```csharp
class Calculator
{
    private int value;
    
    public Calculator(int initialValue)
    {
        value = initialValue;
    }
    
    public Calculator Apply(Func<int, int> operation)
    {
        value = operation(value);
        return this;
    }
    
    public void Display()
    {
        Console.WriteLine($"결과: {value}");
    }
}

// 메서드 체이닝을 통한 연속 연산
var calc = new Calculator(5);
calc.Apply(x => x * 2)      // 10
    .Apply(x => x + 3)      // 13
    .Apply(x => x * x)      // 169
    .Display();             // 출력: 결과: 169
```

**Predicate 델리게이트:**

`Func<T, bool>`과 동일한 역할을 하는 `Predicate<T>`도 있습니다. 주로 조건 검사에 사용됩니다:

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Predicate 사용
Predicate<int> isEven = x => x % 2 == 0;
List<int> evenNumbers = numbers.FindAll(isEven);
Console.WriteLine("짝수: " + string.Join(", ", evenNumbers));
// 출력: 짝수: 2, 4, 6, 8, 10

// Func<int, bool>로도 같은 작업 가능
Func<int, bool> isOdd = x => x % 2 != 0;
var oddNumbers = numbers.Where(isOdd);
Console.WriteLine("홀수: " + string.Join(", ", oddNumbers));
// 출력: 홀수: 1, 3, 5, 7, 9
```

---

## 8.2 확장 메서드 (Extension Methods)

확장 메서드는 기존 타입에 새로운 메서드를 추가하는 것처럼 보이게 하는 C# 3.0의 혁신적인 기능입니다. 실제로는 정적 메서드(Static Method)이지만, 인스턴스 메서드처럼 호출할 수 있어 코드의 가독성과 표현력을 크게 향상시킵니다. 이는 "문법적 설탕(Syntactic Sugar)"의 우아한 예로, 컴파일 시점에 일반적인 정적 메서드 호출로 변환되지만, 개발자에게는 훨씬 자연스러운 API를 제공합니다.

**확장 메서드의 설계 철학과 객체지향 원칙:**

확장 메서드는 SOLID 원칙 중 **개방-폐쇄 원칙(Open-Closed Principle, OCP)**을 구현하는 우아한 방법입니다. Robert C. Martin이 제시한 이 원칙은 "소프트웨어 엔터티(클래스, 모듈, 함수 등)는 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다"는 것입니다. 확장 메서드를 통해 기존 클래스의 소스 코드를 수정하지 않고(폐쇄), 새로운 기능을 추가할 수 있어(개방) 코드의 안정성을 유지하면서도 확장성을 제공합니다.

**확장 메서드가 해결하는 문제들:**

1. **sealed 클래스의 확장**: `string`, `DateTime` 같은 sealed 클래스는 상속할 수 없지만, 확장 메서드로 기능 추가 가능
2. **외부 라이브러리의 타입 확장**: 소스 코드를 수정할 수 없는 제3자 라이브러리의 타입에 메서드 추가
3. **인터페이스에 구현 추가**: C# 8.0 이전에는 인터페이스에 기본 구현을 추가할 수 없었지만, 확장 메서드로 우회 가능
4. **기본 타입의 도메인 특화**: `int`, `double` 같은 기본 타입에 도메인 특화된 연산 추가
5. **Fluent API 구축**: 메서드 체이닝을 통한 가독성 높은 API 설계

**확장 메서드의 실제 활용 사례:**

가장 대표적인 예는 **LINQ(Language Integrated Query)**입니다. LINQ의 모든 메서드(`Where`, `Select`, `OrderBy`, `GroupBy` 등)는 `IEnumerable<T>` 인터페이스에 대한 확장 메서드로 구현되어 있습니다. 이를 통해 모든 컬렉션 타입에서 일관된 쿼리 API를 사용할 수 있습니다:

```csharp
// LINQ 메서드는 모두 확장 메서드
public static class Enumerable
{
    public static IEnumerable<TSource> Where<TSource>(
        this IEnumerable<TSource> source, 
        Func<TSource, bool> predicate)
    {
        // 구현...
    }
}
```

**확장 메서드의 컴파일 메커니즘:**

확장 메서드는 컴파일 타임에 다음과 같이 변환됩니다:

```csharp
// 소스 코드 (확장 메서드 호출)
string text = "Hello";
bool result = text.IsLongerThan(3);

// 컴파일 후 (정적 메서드 호출로 변환)
string text = "Hello";
bool result = StringExtensions.IsLongerThan(text, 3);
```

이러한 변환은 IL(Intermediate Language) 수준에서 완전히 동일하므로, 성능상 차이가 전혀 없습니다. 순전히 코드 가독성과 표현력을 위한 문법적 기능입니다.

**확장 메서드의 정의 규칙:**

확장 메서드를 정의하기 위해서는 다음 규칙을 반드시 따라야 합니다:

1. **정적 클래스(Static Class)**에 정의해야 합니다
2. **정적 메서드(Static Method)**여야 합니다
3. 첫 번째 매개변수에 **`this` 키워드**를 붙여야 합니다 (이것이 핵심)
4. 첫 번째 매개변수의 타입이 확장할 대상 타입입니다
5. **네임스페이스 임포트**: 확장 메서드를 사용하려면 해당 정적 클래스가 속한 네임스페이스를 `using`으로 임포트해야 합니다

**기본 구조:**

```csharp
// 네임스페이스 선언 (중요!)
namespace MyExtensions
{
    // 1. 반드시 정적 클래스
    public static class 확장메서드클래스명
    {
        // 2. 반드시 정적 메서드
        // 3. 첫 번째 매개변수에 this 키워드 (이것이 확장 메서드의 핵심!)
        public static 반환타입 메서드명(this 확장할타입 매개변수, 추가매개변수들...)
        {
            // 메서드 구현
        }
    }
}

// 사용 시
using MyExtensions;  // 네임스페이스 임포트 필수!
```

**메모리와 성능 고려사항:**

- **인라인 최적화**: JIT 컴파일러는 간단한 확장 메서드를 인라인화하여 성능 최적화
- **가상 디스패치 없음**: 정적 메서드이므로 가상 메서드 호출(Virtual Method Dispatch)의 오버헤드 없음
- **박싱 없음**: 값 타입에 대한 확장 메서드는 박싱(Boxing)을 유발하지 않음

**문자열 확장 메서드 예제:**

```csharp
// 확장 메서드를 담을 정적 클래스
public static class StringExtensions
{
    // 문자열이 특정 길이 이상인지 확인
    public static bool IsLongerThan(this string str, int length)
    {
        return str != null && str.Length > length;
    }
    
    // 문자열을 지정된 횟수만큼 반복
    public static string Repeat(this string str, int count)
    {
        if (str == null || count <= 0)
            return string.Empty;
            
        return string.Concat(Enumerable.Repeat(str, count));
    }
    
    // 문자열의 단어 개수 세기
    public static int WordCount(this string str)
    {
        if (string.IsNullOrWhiteSpace(str))
            return 0;
            
        return str.Split(new[] { ' ', '\t', '\n' }, 
                        StringSplitOptions.RemoveEmptyEntries).Length;
    }
}

// 사용 예제
string text = "Hello";

// 확장 메서드를 인스턴스 메서드처럼 호출
bool isLong = text.IsLongerThan(3);
Console.WriteLine($"'{text}'는 3자보다 길까요? {isLong}");
// 출력: 'Hello'는 3자보다 길까요? True

string repeated = text.Repeat(3);
Console.WriteLine(repeated);
// 출력: HelloHelloHello

string sentence = "This is a sample sentence";
int words = sentence.WordCount();
Console.WriteLine($"단어 개수: {words}");
// 출력: 단어 개수: 5
```

**정수 확장 메서드:**

```csharp
public static class IntExtensions
{
    // 숫자가 짝수인지 확인
    public static bool IsEven(this int number)
    {
        return number % 2 == 0;
    }
    
    // 숫자가 홀수인지 확인
    public static bool IsOdd(this int number)
    {
        return number % 2 != 0;
    }
    
    // 숫자의 팩토리얼 계산
    public static long Factorial(this int number)
    {
        if (number < 0)
            throw new ArgumentException("음수는 팩토리얼을 계산할 수 없습니다.");
        if (number == 0 || number == 1)
            return 1;
            
        long result = 1;
        for (int i = 2; i <= number; i++)
        {
            result *= i;
        }
        return result;
    }
}

// 사용 예제
int num = 5;

Console.WriteLine($"{num}은 짝수? {num.IsEven()}");
Console.WriteLine($"{num}은 홀수? {num.IsOdd()}");
// 출력:
// 5은 짝수? False
// 5은 홀수? True

Console.WriteLine($"{num}! = {num.Factorial()}");
// 출력: 5! = 120
```

**컬렉션 확장 메서드:**

```csharp
public static class CollectionExtensions
{
    // 리스트의 각 요소를 출력
    public static void Print<T>(this IEnumerable<T> collection)
    {
        foreach (var item in collection)
        {
            Console.WriteLine(item);
        }
    }
    
    // 리스트가 비어있거나 null인지 확인
    public static bool IsNullOrEmpty<T>(this IEnumerable<T> collection)
    {
        return collection == null || !collection.Any();
    }
    
    // 리스트의 모든 요소를 구분자로 연결
    public static string JoinString<T>(this IEnumerable<T> collection, string separator)
    {
        return string.Join(separator, collection);
    }
}

// 사용 예제
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

Console.WriteLine("숫자 출력:");
numbers.Print();
// 출력:
// 1
// 2
// 3
// 4
// 5

bool isEmpty = numbers.IsNullOrEmpty();
Console.WriteLine($"리스트가 비어있나요? {isEmpty}");
// 출력: 리스트가 비어있나요? False

string joined = numbers.JoinString(" -> ");
Console.WriteLine(joined);
// 출력: 1 -> 2 -> 3 -> 4 -> 5
```

**확장 메서드 호출 방식:**

확장 메서드는 두 가지 방식으로 호출할 수 있습니다:

```csharp
string text = "Hello";

// 1. 인스턴스 메서드 스타일 (권장)
bool result1 = text.IsLongerThan(3);

// 2. 정적 메서드 스타일
bool result2 = StringExtensions.IsLongerThan(text, 3);

// 두 방식은 완전히 동일한 결과를 생성
Console.WriteLine($"방법 1: {result1}");  // 출력: 방법 1: True
Console.WriteLine($"방법 2: {result2}");  // 출력: 방법 2: True
```

**확장 메서드의 제약과 주의사항:**

1. **우선순위**: 확장 메서드는 인스턴스 메서드보다 우선순위가 낮습니다. 동일한 시그니처의 인스턴스 메서드가 있으면 그것이 호출됩니다.

2. **접근 제한**: 확장 메서드는 `public` 멤버만 접근할 수 있습니다. `private`이나 `protected` 멤버는 접근할 수 없습니다.

3. **네임스페이스**: 확장 메서드를 사용하려면 해당 확장 메서드가 정의된 네임스페이스를 `using`으로 포함해야 합니다.

```csharp
// 확장 메서드 정의 (MyExtensions 네임스페이스)
namespace MyExtensions
{
    public static class IntExtensions
    {
        public static bool IsPositive(this int number)
        {
            return number > 0;
        }
    }
}

// 다른 파일에서 사용
using MyExtensions;  // 이 using이 필요함

int num = 5;
bool positive = num.IsPositive();  // 이제 사용 가능
```

**실용적인 확장 메서드 예제 - 날짜 처리:**

```csharp
public static class DateTimeExtensions
{
    public static bool IsWeekend(this DateTime date)
    {
        return date.DayOfWeek == DayOfWeek.Saturday 
            || date.DayOfWeek == DayOfWeek.Sunday;
    }
    
    public static bool IsWeekday(this DateTime date)
    {
        return !date.IsWeekend();
    }
    
    public static string ToKoreanDateString(this DateTime date)
    {
        string[] dayNames = { "일", "월", "화", "수", "목", "금", "토" };
        return $"{date.Year}년 {date.Month}월 {date.Day}일 ({dayNames[(int)date.DayOfWeek]}요일)";
    }
}

// 사용 예제
DateTime today = DateTime.Now;

Console.WriteLine(today.ToKoreanDateString());
Console.WriteLine($"주말인가요? {today.IsWeekend()}");
Console.WriteLine($"평일인가요? {today.IsWeekday()}");
```

**메서드 체이닝:**

확장 메서드는 메서드 체이닝을 통해 더욱 가독성 높은 코드를 작성할 수 있게 해줍니다:

```csharp
public static class StringChainExtensions
{
    public static string RemoveSpaces(this string str)
    {
        return str.Replace(" ", "");
    }
    
    public static string Capitalize(this string str)
    {
        if (string.IsNullOrEmpty(str))
            return str;
        return char.ToUpper(str[0]) + str.Substring(1).ToLower();
    }
    
    public static string AddQuotes(this string str)
    {
        return $"\"{str}\"";
    }
}

// 메서드 체이닝 사용
string text = "  hello world  ";
string result = text.Trim()
                   .RemoveSpaces()
                   .Capitalize()
                   .AddQuotes();

Console.WriteLine(result);
// 출력: "Helloworld"
```

---

## 8.3 재귀 함수

재귀 함수(Recursive Function)는 자기 자신을 호출하는 함수로, 복잡한 문제를 더 작고 단순한 하위 문제로 분할하여 해결하는 강력한 프로그래밍 기법입니다. 재귀는 **분할 정복(Divide and Conquer)** 알고리즘의 핵심 원리이며, 트리(Tree), 그래프(Graph), 연결 리스트(Linked List) 같은 재귀적 자료구조를 다루는 자연스러운 방법입니다.

**재귀의 수학적 기반과 귀납법:**

재귀는 수학의 **귀납법(Mathematical Induction)**과 밀접한 관련이 있습니다. 귀납법은 다음 두 단계로 구성됩니다:
1. **기초 단계(Base Case)**: 가장 작은 경우에 대해 명제가 참임을 증명
2. **귀납 단계(Inductive Step)**: n=k일 때 참이면, n=k+1일 때도 참임을 증명

재귀 함수는 이와 정확히 대응됩니다:
1. **기저 조건(Base Case)**: 재귀를 종료하는 가장 간단한 경우
2. **재귀 호출(Recursive Call)**: 더 작은 문제로 축소하여 자기 자신을 호출

예를 들어, 팩토리얼(Factorial)은 수학적으로 다음과 같이 재귀적으로 정의됩니다:
```
n! = n × (n-1)!  (n > 0)
0! = 1           (기저 조건)
```

이러한 수학적 정의를 프로그래밍 코드로 거의 직접적으로 옮길 수 있다는 점이 재귀의 아름다움입니다. 코드와 수학적 정의 사이의 이러한 일대일 대응은 프로그램의 정확성(Correctness)을 증명하기 쉽게 만들어줍니다.

**재귀의 이론적 배경:**

재귀는 컴퓨터 과학의 근본적인 개념들과 깊이 연결되어 있습니다:

1. **튜링 머신과 계산 가능성**: 모든 재귀 함수는 반복문(Loop)으로 변환 가능하며, 이는 튜링 완전성(Turing Completeness)과 관련됩니다
2. **호출 스택(Call Stack)**: 재귀는 프로그램의 호출 스택 메커니즘을 직접적으로 활용합니다
3. **함수형 프로그래밍**: Lisp, Haskell 같은 함수형 언어에서 재귀는 반복의 주된 방법입니다
4. **분할 정복**: 병합 정렬(Merge Sort), 퀵 정렬(Quick Sort) 등 많은 효율적인 알고리즘의 기반

**재귀 함수의 필수 요소:**

모든 올바른 재귀 함수는 다음 두 가지 요소를 반드시 포함해야 합니다. 이 중 하나라도 빠지면 프로그램은 제대로 작동하지 않습니다:

1. **기저 조건(Base Case, Termination Condition)**: 
   - 재귀를 멈추는 종료 조건
   - 이것이 없으면 무한 재귀에 빠져 **스택 오버플로우(Stack Overflow)** 발생
   - 가장 작고 단순한 문제의 해를 직접 반환
   - 예: 0! = 1, 빈 리스트의 길이 = 0

2. **재귀 호출(Recursive Call, Reduction Step)**:
   - 문제를 더 작은 하위 문제로 분할
   - 자기 자신을 호출하되, 반드시 기저 조건에 가까워지는 방향으로 진행
   - 예: n! = n × (n-1)!, 리스트의 길이 = 1 + (나머지 리스트의 길이)

**재귀의 실행 메커니즘 - 호출 스택:**

재귀 함수가 호출될 때마다 다음 정보가 **호출 스택(Call Stack)**에 저장됩니다:
- 함수의 매개변수 값
- 지역 변수
- 반환 주소 (재귀 호출 후 돌아올 위치)

이러한 정보를 저장하는 메모리 공간을 **활성화 레코드(Activation Record)** 또는 **스택 프레임(Stack Frame)**이라고 합니다. 재귀 깊이가 깊어질수록 스택 메모리 사용량이 증가하며, 스택 크기 한계를 초과하면 **스택 오버플로우(Stack Overflow Error)**가 발생합니다.

```
Factorial(3) 호출 시 스택 상태:

[Factorial(3)]                    // 3 * Factorial(2) 대기
[Factorial(3)]
[Factorial(2)]                    // 2 * Factorial(1) 대기
[Factorial(3)]
[Factorial(2)]
[Factorial(1)]                    // 1 반환 (기저 조건)
[Factorial(3)]
[Factorial(2)]                    // 2 * 1 = 2 반환
[Factorial(3)]                    // 3 * 2 = 6 반환
```

**기본 예제 - 팩토리얼:**

팩토리얼(n!)은 1부터 n까지의 모든 양의 정수를 곱한 값입니다. 예를 들어 5! = 5 × 4 × 3 × 2 × 1 = 120입니다. 이는 재귀를 설명하는 가장 고전적인 예제이며, 수학적 정의와 코드의 대응이 명확합니다.

```csharp
// 재귀적 팩토리얼
int Factorial(int n)
{
    // 기저 조건: 0! = 1, 1! = 1
    if (n <= 1)
    {
        return 1;
    }
    
    // 재귀 호출: n! = n × (n-1)!
    return n * Factorial(n - 1);
}

// 사용 예제
Console.WriteLine($"5! = {Factorial(5)}");   // 출력: 5! = 120
Console.WriteLine($"10! = {Factorial(10)}"); // 출력: 10! = 3628800

// 재귀 과정 시각화:
// Factorial(5)
//   = 5 * Factorial(4)
//   = 5 * (4 * Factorial(3))
//   = 5 * (4 * (3 * Factorial(2)))
//   = 5 * (4 * (3 * (2 * Factorial(1))))
//   = 5 * (4 * (3 * (2 * 1)))
//   = 5 * (4 * (3 * 2))
//   = 5 * (4 * 6)
//   = 5 * 24
//   = 120
```

**재귀 vs 반복문 비교:**

같은 문제를 재귀와 반복문 두 가지 방식으로 해결할 수 있습니다. 이 두 방식은 이론적으로 동등한 표현력을 가지며(튜링 완전성), 서로 변환이 가능합니다.

```csharp
// 재귀 방식 - 수학적 정의에 가까움
int FactorialRecursive(int n)
{
    if (n <= 1)
        return 1;
    return n * FactorialRecursive(n - 1);
}

// 반복문 방식 - 명시적 상태 관리
int FactorialIterative(int n)
{
    int result = 1;
    for (int i = 2; i <= n; i++)
    {
        result *= i;
    }
    return result;
}

// 두 방식 모두 같은 결과
Console.WriteLine(FactorialRecursive(5));  // 출력: 120
Console.WriteLine(FactorialIterative(5));  // 출력: 120
```

**재귀의 장단점과 실무 적용 지침:**

**장점:**
1. **코드 간결성과 우아함**: 복잡한 문제를 매우 간결하게 표현 가능. 특히 트리, 그래프 같은 재귀적 자료구조에서 자연스러움
2. **수학적 정의와의 일치**: 수학적 정의나 자료구조의 정의를 거의 그대로 코드로 변환 가능
3. **분할 정복의 자연스러운 구현**: 큰 문제를 작은 문제로 나누는 사고방식을 직접적으로 표현
4. **증명 가능성**: 귀납법을 통해 프로그램의 정확성을 수학적으로 증명하기 쉬움

**단점:**
1. **스택 메모리 사용**: 각 재귀 호출마다 O(1) 스택 공간 사용. 깊이가 n이면 O(n) 공간 복잡도
2. **스택 오버플로우 위험**: C#의 기본 스택 크기는 약 1MB로, 깊은 재귀(수천~수만 레벨)는 오버플로우 유발
3. **함수 호출 오버헤드**: 함수 호출/반환에 따른 성능 비용 (현대 컴파일러는 최적화하지만)
4. **중복 계산 가능성**: 피보나치 같은 경우 지수 시간 복잡도 발생 (메모이제이션으로 해결)
5. **디버깅 복잡도**: 깊은 호출 스택 추적의 어려움

**재귀 사용 가이드라인:**

| 상황 | 재귀 추천 | 반복문 추천 |
|------|-----------|-------------|
| 트리/그래프 순회 | ✅ | ❌ |
| 분할 정복 알고리즘 | ✅ | ❌ |
| 백트래킹 문제 | ✅ | ❌ |
| 단순 반복 | ❌ | ✅ |
| 깊이 >1000 | ❌ | ✅ |
| 성능 최우선 | ❌ | ✅ |

**재귀의 시간 복잡도 분석:**

```
T(n) = a·T(n/b) + f(n)

여기서:
- a: 하위 문제의 개수
- n/b: 하위 문제의 크기
- f(n): 분할/결합에 드는 비용

예시:
- 팩토리얼: T(n) = T(n-1) + O(1) → O(n) 시간, O(n) 공간
- 피보나치(단순): T(n) = T(n-1) + T(n-2) + O(1) → O(2^n) 시간
- 이진 탐색: T(n) = T(n/2) + O(1) → O(log n) 시간
```

**피보나치 수열 - 재귀의 비효율성 예제:**

피보나치 수열은 재귀의 고전적인 예제이면서, 동시에 순수 재귀의 비효율성을 보여주는 완벽한 예입니다. 각 항은 앞의 두 항의 합입니다: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...

**중복 계산 문제:**
```
Fib(5) 계산 시 중복:
                Fib(5)
               /      \
           Fib(4)      Fib(3)
          /     \      /    \
      Fib(3)  Fib(2) Fib(2) Fib(1)
      /   \    /  \    /  \
  Fib(2) Fib(1) ...  ...  ...

Fib(2)가 3번, Fib(3)가 2번 계산됨 → 지수적 증가
```

```csharp
// 재귀적 피보나치 (비효율적 - O(2^n) 시간 복잡도)
int FibonacciRecursive(int n)
{
    // 기저 조건
    if (n <= 1)
        return n;
    
    // 재귀 호출 - 중복 계산 발생!
    return FibonacciRecursive(n - 1) + FibonacciRecursive(n - 2);
}

// 반복문 피보나치 (효율적 - O(n) 시간, O(1) 공간)
int FibonacciIterative(int n)
{
    if (n <= 1)
        return n;
    
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++)
    {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}

// 사용 예제
Console.WriteLine("피보나치 수열 (재귀):");
for (int i = 0; i <= 10; i++)
{
    Console.Write($"{FibonacciRecursive(i)} ");
}
Console.WriteLine();
// 출력: 0 1 1 2 3 5 8 13 21 34 55

Console.WriteLine("\n피보나치 수열 (반복문):");
for (int i = 0; i <= 10; i++)
{
    Console.Write($"{FibonacciIterative(i)} ");
}
Console.WriteLine();
// 출력: 0 1 1 2 3 5 8 13 21 34 55
```

**주의:** 단순 재귀 피보나치는 O(2^n)의 시간 복잡도를 가져 매우 비효율적입니다. 큰 n 값에는 사용하지 않는 것이 좋습니다.

**배열의 합 구하기:**

```csharp
// 재귀로 배열의 합 구하기
int SumArray(int[] array, int index)
{
    // 기저 조건: 배열의 끝에 도달
    if (index >= array.Length)
        return 0;
    
    // 재귀 호출: 현재 요소 + 나머지 배열의 합
    return array[index] + SumArray(array, index + 1);
}

int[] numbers = { 1, 2, 3, 4, 5 };
int total = SumArray(numbers, 0);
Console.WriteLine($"배열의 합: {total}");
// 출력: 배열의 합: 15
```

**문자열 뒤집기:**

```csharp
// 재귀로 문자열 뒤집기
string ReverseString(string str)
{
    // 기저 조건: 빈 문자열이거나 한 글자
    if (str.Length <= 1)
        return str;
    
    // 재귀 호출: 첫 글자를 제외한 나머지를 뒤집고, 첫 글자를 뒤에 추가
    return ReverseString(str.Substring(1)) + str[0];
}

string original = "Hello";
string reversed = ReverseString(original);
Console.WriteLine($"원본: {original}");
Console.WriteLine($"뒤집기: {reversed}");
// 출력:
// 원본: Hello
// 뒤집기: olleH
```

**최대공약수 (유클리드 알고리즘):**

유클리드 알고리즘은 재귀의 우아함을 보여주는 훌륭한 예입니다:

```csharp
// 최대공약수 (GCD) - 재귀
int GCD(int a, int b)
{
    // 기저 조건: b가 0이면 a가 최대공약수
    if (b == 0)
        return a;
    
    // 재귀 호출: GCD(b, a % b)
    return GCD(b, a % b);
}

Console.WriteLine($"GCD(48, 18) = {GCD(48, 18)}");  // 출력: GCD(48, 18) = 6
Console.WriteLine($"GCD(100, 35) = {GCD(100, 35)}"); // 출력: GCD(100, 35) = 5

// 재귀 과정:
// GCD(48, 18)
//   = GCD(18, 48 % 18)
//   = GCD(18, 12)
//   = GCD(12, 18 % 12)
//   = GCD(12, 6)
//   = GCD(6, 12 % 6)
//   = GCD(6, 0)
//   = 6
```

**거듭제곱 계산:**

```csharp
// x의 n제곱을 재귀로 계산
double Power(double x, int n)
{
    // 기저 조건: x^0 = 1
    if (n == 0)
        return 1;
    
    // 음수 지수 처리
    if (n < 0)
        return 1.0 / Power(x, -n);
    
    // 재귀 호출
    return x * Power(x, n - 1);
}

Console.WriteLine($"2^5 = {Power(2, 5)}");    // 출력: 2^5 = 32
Console.WriteLine($"3^4 = {Power(3, 4)}");    // 출력: 3^4 = 81
Console.WriteLine($"2^-3 = {Power(2, -3)}");  // 출력: 2^-3 = 0.125
```

**재귀의 실용적 활용 - 파일 시스템 탐색:**

재귀는 트리 구조(디렉토리, XML, JSON 등)를 다룰 때 자연스러운 해법을 제공합니다:

```csharp
void PrintDirectory(string path, int depth = 0)
{
    try
    {
        // 들여쓰기
        string indent = new string(' ', depth * 2);
        
        // 현재 디렉토리 출력
        string dirName = Path.GetFileName(path);
        Console.WriteLine($"{indent}[폴더] {dirName}");
        
        // 파일들 출력
        foreach (string file in Directory.GetFiles(path))
        {
            string fileName = Path.GetFileName(file);
            Console.WriteLine($"{indent}  - {fileName}");
        }
        
        // 하위 디렉토리를 재귀적으로 탐색
        foreach (string subDir in Directory.GetDirectories(path))
        {
            PrintDirectory(subDir, depth + 1);
        }
    }
    catch (UnauthorizedAccessException)
    {
        // 접근 권한이 없는 디렉토리는 건너뜀
    }
}

// 사용 예제 (주의: 실제 경로로 테스트)
// PrintDirectory("/home/username/MyFolder");
```

**꼬리 재귀 최적화 (Tail Call Optimization):**

꼬리 재귀(Tail Recursion)는 재귀 호출이 함수의 **마지막 연산**인 경우를 말합니다. 이는 컴파일러 최적화의 중요한 대상이며, 함수형 프로그래밍 언어(Scheme, Scala 등)에서는 필수적인 최적화 기법입니다.

**꼬리 재귀의 정의와 중요성:**

일반 재귀에서는 재귀 호출 후에 추가 연산(예: 곱셈, 덧셈)이 필요하므로, 호출 스택에 반환 주소와 중간 결과를 저장해야 합니다. 하지만 꼬리 재귀에서는 재귀 호출의 결과가 그대로 함수의 결과이므로, 이론적으로 현재 스택 프레임을 재사용할 수 있습니다. 이를 **꼬리 호출 최적화(Tail Call Optimization, TCO)** 또는 **꼬리 호출 제거(Tail Call Elimination)**라고 합니다.

**주의:** C#의 JIT 컴파일러는 현재 꼬리 재귀 최적화를 보장하지 않습니다. 일부 경우에 최적화되기도 하지만, 일관성이 없습니다. F# 같은 함수형 언어는 항상 TCO를 보장합니다.

```csharp
// 일반 재귀 (꼬리 재귀 아님)
int FactorialNormal(int n)
{
    if (n <= 1)
        return 1;
    return n * FactorialNormal(n - 1);  // ❌ 곱셈 연산이 재귀 호출 후에 수행됨
}
// 실행 흐름: FactorialNormal(5) = 5 * [FactorialNormal(4) = 4 * [FactorialNormal(3) = ...]]
// 스택 필요: 각 호출의 n 값과 곱셈 대기 상태 저장

// 꼬리 재귀 (Tail Recursive)
int FactorialTail(int n, int accumulator = 1)
{
    if (n <= 1)
        return accumulator;
    return FactorialTail(n - 1, n * accumulator);  // ✅ 재귀 호출이 마지막 연산
}
// 실행 흐름: FactorialTail(5, 1) → FactorialTail(4, 5) → FactorialTail(3, 20) → ...
// 스택 불필요: 누산기(accumulator)에 중간 결과 저장, 스택 프레임 재사용 가능

Console.WriteLine(FactorialNormal(5));  // 출력: 120
Console.WriteLine(FactorialTail(5));    // 출력: 120
```

**누산기(Accumulator) 패턴:**

꼬리 재귀를 구현하는 핵심 기법은 **누산기(Accumulator)** 매개변수를 사용하는 것입니다. 중간 계산 결과를 누산기에 저장하여 전달함으로써, 재귀 호출 후 추가 연산이 불필요하게 만듭니다.

```csharp
// 리스트 합계 - 꼬리 재귀 버전
int SumListTail(List<int> list, int index = 0, int accumulator = 0)
{
    if (index >= list.Count)
        return accumulator;
    return SumListTail(list, index + 1, accumulator + list[index]);
}
```

**재귀 사용 시 주의사항과 실무 가이드:**

1. **항상 기저 조건 확인**: 기저 조건이 없거나 도달 불가능하면 무한 재귀로 인한 스택 오버플로우 발생
   ```csharp
   // ❌ 잘못된 예 - 기저 조건 누락
   int BadRecursion(int n)
   {
       return n + BadRecursion(n - 1);  // StackOverflowException!
   }
   ```

2. **재귀 깊이 고려**: C#의 기본 스택 크기는 약 1MB (x86: 1MB, x64: 4MB). 각 프레임이 약 100바이트라면 약 10,000 레벨까지 가능하지만, 안전 마진을 고려해야 함
   ```csharp
   // 재귀 깊이 제한 예제
   int SafeRecursive(int n, int maxDepth = 1000)
   {
       if (n <= 1 || maxDepth <= 0)
           return 1;
       return n * SafeRecursive(n - 1, maxDepth - 1);
   }
   ```

3. **성능 고려**: 중복 계산이 많은 경우 메모이제이션(Memoization) 또는 동적 계획법(Dynamic Programming) 사용
   ```csharp
   // 메모이제이션을 사용한 피보나치 (O(n) 시간 복잡도)
   Dictionary<int, int> memo = new Dictionary<int, int>();
   int FibonacciMemo(int n)
   {
       if (n <= 1) return n;
       if (memo.ContainsKey(n)) return memo[n];
       
       int result = FibonacciMemo(n - 1) + FibonacciMemo(n - 2);
       memo[n] = result;
       return result;
   }
   ```

4. **디버깅과 로깅**: 재귀 호출의 흐름을 추적할 수 있도록 적절한 로깅 추가
   ```csharp
   int FactorialDebug(int n, int depth = 0)
   {
       string indent = new string(' ', depth * 2);
       Console.WriteLine($"{indent}Factorial({n}) 호출");
       
       if (n <= 1)
       {
           Console.WriteLine($"{indent}기저 조건: 1 반환");
           return 1;
       }
       
       int result = n * FactorialDebug(n - 1, depth + 1);
       Console.WriteLine($"{indent}Factorial({n}) = {result}");
       return result;
   }
   ```

5. **재귀 vs 반복 선택 기준**:
   - **재귀 선택**: 트리/그래프 순회, 분할 정복, 백트래킹, 수학적 정의가 재귀적인 경우
   - **반복 선택**: 단순 반복, 깊이 > 1000, 성능이 매우 중요한 경우, 스택 공간 제약이 있는 환경

---

## 마무리

이 장에서는 C#의 고급 메서드 활용 기법을 체계적으로 학습했습니다. 람다 식을 통해 간결하고 표현력 있는 함수형 코드를 작성하는 방법을 익혔고, `Func`와 `Action` 델리게이트를 사용하여 메서드를 일급 객체(First-class Citizen)로 다루는 방법을 배웠습니다. 확장 메서드를 통해 개방-폐쇄 원칙(Open-Closed Principle)을 구현하며 기존 타입에 새로운 기능을 추가하는 우아한 방법을 이해했으며, 재귀 함수를 활용하여 복잡한 문제를 분할 정복(Divide and Conquer)하는 기법을 학습했습니다.

**프로그래밍 패러다임의 진화:**

이러한 고급 기법들은 단순히 코드를 짧게 만드는 "문법적 설탕(Syntactic Sugar)"을 넘어서, 프로그래밍 패러다임의 근본적인 변화를 반영합니다. C#은 명령형(Imperative), 객체지향(Object-Oriented), 함수형(Functional), 그리고 선언적(Declarative) 프로그래밍 스타일을 모두 지원하는 다중 패러다임 언어(Multi-paradigm Language)로 진화했습니다.

**람다 식과 LINQ**는 SQL의 선언적 쿼리 스타일을 C#에 도입하여, "어떻게(How)"보다 "무엇을(What)"에 집중할 수 있게 했습니다. 이는 코드의 의도를 더 명확하게 표현하고, 버그 발생 가능성을 줄이며, 코드 리뷰와 유지보수를 더 쉽게 만듭니다.

**확장 메서드**는 SOLID 원칙 중 개방-폐쇄 원칙을 실현하며, 기존 코드의 안정성을 해치지 않으면서도 새로운 기능을 추가할 수 있게 합니다. 이는 특히 레거시 코드베이스를 다룰 때나, 제3자 라이브러리를 확장할 때 매우 유용합니다.

**재귀 함수**는 수학적 귀납법의 프로그래밍적 구현으로, 복잡한 문제를 단순한 하위 문제로 분해하는 강력한 사고 도구입니다. 비록 성능과 스택 메모리 제약이 있지만, 트리와 그래프 같은 재귀적 자료구조를 다룰 때는 가장 자연스럽고 우아한 해결책을 제공합니다.

**실무 적용과 모범 사례:**

1. **람다 식 활용**:
   - LINQ 쿼리에서 복잡한 데이터 변환 파이프라인 구축
   - 이벤트 핸들러를 간결하게 작성하여 코드 가독성 향상
   - 전략 패턴(Strategy Pattern)을 람다로 구현하여 유연성 확보

2. **델리게이트 활용**:
   - 고차 함수(Higher-order Functions)를 통한 코드 재사용성 극대화
   - 콜백 메커니즘으로 비동기 작업 처리
   - 의존성 주입(Dependency Injection)에서 팩토리 함수로 활용

3. **확장 메서드 설계**:
   - 도메인 특화 언어(DSL, Domain-Specific Language) 구축
   - Fluent API를 통한 가독성 높은 인터페이스 설계
   - 유틸리티 메서드를 적절한 네임스페이스로 구성

4. **재귀 함수 적용**:
   - 파일 시스템, XML/JSON 트리 순회
   - 분할 정복 알고리즘 (병합 정렬, 퀵 정렬)
   - 백트래킹 문제 (N-Queens, 미로 찾기)
   - 깊이가 제한적이고 구조가 재귀적인 경우

**성능 최적화 고려사항:**

- **람다와 델리게이트**: 핫 패스(Hot Path)에서는 인라인 코드보다 약간 느릴 수 있으나, 현대 JIT 컴파일러는 대부분 최적화
- **확장 메서드**: 정적 메서드 호출로 컴파일되므로 성능 오버헤드 없음
- **재귀**: 깊은 재귀는 스택 오버플로우 위험. 꼬리 재귀나 반복문으로 변환 고려
- **LINQ**: 지연 실행으로 불필요한 계산 방지, 하지만 복잡한 쿼리는 프로파일링 필요

**학습 정리:**

✅ **람다 식 마스터**: 익명 함수를 간결하게 표현하는 람다 식 문법과 활용법, 람다 계산법의 이론적 배경
✅ **델리게이트 이해**: `Func`와 `Action`을 통한 메서드의 일급 객체화, 델리게이트 내부 구조와 공변성
✅ **확장 메서드 활용**: 기존 타입을 확장하는 우아한 방법, 개방-폐쇄 원칙의 실현, LINQ의 구현 메커니즘
✅ **재귀 함수 구현**: 자기 자신을 호출하여 문제를 해결하는 재귀적 사고, 수학적 귀납법과의 관계, 꼬리 재귀 최적화

**다음 단계:**

9장에서는 객체지향 프로그래밍의 핵심인 클래스와 객체에 대해 학습하게 됩니다. 지금까지 배운 메서드 활용 기법들은 클래스 설계와 결합되어 더욱 강력한 추상화를 가능하게 합니다. 특히 람다 식과 델리게이트는 이벤트 기반 프로그래밍과 디자인 패턴의 구현에 필수적이며, 확장 메서드는 Fluent API와 Builder 패턴의 기반이 됩니다.

**실습 과제:**

1. **람다와 LINQ**: 문자열 리스트에서 길이가 5 이상인 단어만 필터링하고 대문자로 변환한 후, 알파벳 순으로 정렬하는 LINQ 쿼리 작성
2. **확장 메서드**: DateTime 타입에 한국어 형식으로 날짜를 출력하는 확장 메서드와, 두 날짜 사이의 근무일(주말 제외)을 계산하는 확장 메서드 추가
3. **재귀 함수**: 정렬된 배열에서 특정 값을 찾는 이진 탐색(Binary Search) 알고리즘을 재귀 함수로 구현하고, 반복문 버전과 성능 비교
4. **종합 문제**: 사용자 정의 컬렉션에 LINQ 스타일의 확장 메서드(`MyWhere`, `MySelect`, `MyAggregate`)를 구현하고, 람다 식으로 다양한 쿼리 작성

**권장 학습 자료:**

- "C# in Depth" by Jon Skeet - 람다와 LINQ의 심화 내용
- "Functional Programming in C#" by Enrico Buonanno - 함수형 프로그래밍 패러다임
- "The Art of Computer Programming" by Donald Knuth - 재귀와 알고리즘 이론
- Microsoft Docs: LINQ (Language Integrated Query) - 공식 문서와 예제

특히 LINQ와 함께 사용될 때 이러한 기법들의 진가가 발휘되므로, 14장과 15장에서 다룰 LINQ 관련 내용을 학습할 때 이 장의 내용을 다시 복습하시기 바랍니다.

**다음 단계:**

9장에서는 객체지향 프로그래밍의 핵심인 클래스와 객체에 대해 학습하게 됩니다. 지금까지 배운 메서드 활용 기법들은 클래스 설계와 결합되어 더욱 강력한 추상화를 가능하게 합니다.

**실습 과제:**

1. 문자열 리스트에서 길이가 5 이상인 단어만 필터링하고 대문자로 변환하는 람다 식 작성
2. DateTime 타입에 한국어 형식으로 날짜를 출력하는 확장 메서드 추가
3. 이진 탐색(Binary Search) 알고리즘을 재귀 함수로 구현
4. 사용자 정의 컬렉션에 LINQ 스타일의 확장 메서드 추가
