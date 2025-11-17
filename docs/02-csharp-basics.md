# 2장. C# 기초 문법

1장에서 개발 환경을 구축하고 첫 번째 프로그램을 실행해보았습니다. 이제 본격적으로 C# 언어의 기본 문법을 배울 차례입니다. 이 장에서는 변수와 데이터 타입, 연산자, 입출력 등 프로그래밍의 핵심 구성 요소를 학습합니다.

## 이 장에서 배울 내용

- **변수와 데이터 타입**: 프로그램에서 정보를 저장하고 관리하는 방법
- **연산자**: 데이터를 계산하고 비교하는 다양한 도구들
- **콘솔 입출력**: 사용자와 프로그램이 상호작용하는 방법
- **주석**: 코드를 설명하고 문서화하는 기법

---

## 2.1 변수와 데이터 타입

프로그래밍의 세계에서 변수(Variable)는 정보를 담는 그릇과 같은 역할을 합니다. 현실 세계에서 우리가 물건을 보관하기 위해 상자에 라벨을 붙이듯이, 프로그램에서는 데이터를 저장하기 위해 메모리 공간에 이름을 부여합니다. 이 이름이 바로 변수입니다. 변수를 사용함으로써 프로그램은 실행 중에 동적으로 값을 저장하고, 필요할 때 읽어오며, 상황에 따라 수정할 수 있는 능력을 갖추게 됩니다.

C#은 **정적 타입 언어(Statically Typed Language)**로 분류됩니다. 이는 변수를 선언하는 시점에 해당 변수가 저장할 수 있는 데이터의 종류(타입)를 명시적으로 지정해야 함을 의미합니다. 이러한 특성은 런타임 오류를 컴파일 시점에 미리 발견할 수 있게 해주어, 더욱 안정적이고 예측 가능한 코드 작성을 가능하게 합니다. Python이나 JavaScript와 같은 동적 타입 언어에서는 변수의 타입이 실행 중에 자동으로 결정되지만, C#에서는 개발자가 명시적으로 타입을 선언함으로써 타입 안정성(Type Safety)을 확보합니다.

### 2.1.1 기본 데이터 타입 (int, string, bool, double)

C#은 다양한 상황에서 사용할 수 있는 풍부한 내장 데이터 타입을 제공합니다. 이 중에서도 가장 기본이 되고 자주 사용되는 네 가지 타입에 대해 상세히 알아보겠습니다. 각 타입은 저장할 수 있는 데이터의 종류와 메모리 사용 방식, 그리고 수행할 수 있는 연산이 다르므로, 상황에 맞는 적절한 타입을 선택하는 것이 중요합니다.

#### int (정수형)

`int`는 Integer(정수)의 약자로, 소수점이 없는 정수를 저장하는 32비트 데이터 타입입니다. .NET의 공통 타입 시스템(Common Type System, CTS)에서는 `System.Int32`로 정의되며, 약 -21억부터 +21억까지의 범위를 표현할 수 있습니다. 일상적인 계수, 인덱스, 수량 등을 나타낼 때 가장 널리 사용되는 타입입니다.

**메모리 구조와 범위:**
- 크기: 32비트 (4바이트)
- 범위: -2,147,483,648 ~ 2,147,483,647
- 부호 있음(Signed): 음수와 양수 모두 표현 가능
- 기본값: 0

정수형은 컴퓨터의 산술 논리 장치(ALU, Arithmetic Logic Unit)에서 가장 효율적으로 처리되는 데이터 타입으로, 연산 속도가 매우 빠릅니다. 이는 CPU의 레지스터 크기와 직접적으로 대응되기 때문입니다.

```csharp
// 정수형 변수 선언과 초기화
int age = 25;
int year = 2024;
int temperature = -5;

Console.WriteLine($"나이: {age}");
Console.WriteLine($"연도: {year}");
Console.WriteLine($"온도: {temperature}도");

// 출력:
// 나이: 25
// 연도: 2024
// 온도: -5도
```

**주요 특징:**
- **정수 전용 표현**: 소수점이 없는 온전한 정수만 저장 가능하며, 소수점 값을 할당하면 자동으로 절삭(truncation)됩니다.
- **명확한 범위**: -2,147,483,648부터 2,147,483,647까지로, 대부분의 일상적인 계수 작업에 충분한 범위를 제공합니다.
- **효율적인 연산**: 사칙연산(`+`, `-`, `*`, `/`, `%`)이 매우 빠르게 처리되며, CPU 레지스터와 직접 매핑됩니다.
- **오버플로우 주의**: 범위를 초과하는 연산 시 오버플로우가 발생하며, 기본적으로는 예외를 발생시키지 않고 값이 순환됩니다 (`checked` 컨텍스트에서는 예외 발생).

**관련 정수 타입 비교:**

| 타입 | 크기 | 범위 | 용도 |
|------|------|------|------|
| `sbyte` | 8비트 | -128 ~ 127 | 매우 작은 정수, 메모리 절약 |
| `byte` | 8비트 | 0 ~ 255 | 부호 없는 작은 정수, 바이너리 데이터 |
| `short` | 16비트 | -32,768 ~ 32,767 | 작은 범위의 정수 |
| `ushort` | 16비트 | 0 ~ 65,535 | 부호 없는 작은 정수 |
| `int` | 32비트 | -2,147,483,648 ~ 2,147,483,647 | 일반적인 정수 (가장 많이 사용) |
| `uint` | 32비트 | 0 ~ 4,294,967,295 | 부호 없는 정수 |
| `long` | 64비트 | ±9,223,372,036,854,775,807 | 매우 큰 정수 |
| `ulong` | 64비트 | 0 ~ 18,446,744,073,709,551,615 | 부호 없는 큰 정수 |

#### string (문자열)

`string`은 텍스트 데이터를 저장하는 참조 타입(Reference Type)입니다. .NET의 공통 타입 시스템에서는 `System.String`으로 정의되며, Unicode 문자의 시퀀스로 구현되어 있어 전 세계 모든 언어의 문자를 표현할 수 있습니다. 사용자 이름, 메시지, 파일 경로 등 프로그램에서 다루는 대부분의 텍스트 정보는 문자열로 표현됩니다.

**문자열의 근본적 특성:**

C#의 문자열은 **불변(Immutable)** 객체입니다. 이는 일단 생성된 문자열 객체의 내용은 절대 변경될 수 없다는 의미입니다. 문자열을 "수정"하는 것처럼 보이는 연산(예: 연결, 치환)은 실제로는 새로운 문자열 객체를 생성하여 반환합니다. 이러한 설계는 다음과 같은 이점을 제공합니다:

1. **스레드 안전성(Thread Safety)**: 여러 스레드가 동시에 같은 문자열을 읽어도 안전합니다.
2. **메모리 최적화**: 동일한 내용의 문자열은 메모리를 공유할 수 있습니다 (String Interning).
3. **예측 가능성**: 함수에 문자열을 전달해도 원본이 변경될 걱정이 없습니다.

단, 빈번한 문자열 조작이 필요한 경우에는 `StringBuilder` 클래스를 사용하는 것이 성능상 유리합니다.

텍스트 데이터를 저장하는 타입입니다. 큰따옴표(`"`)로 감싸서 표현합니다.

```csharp
// 문자열 변수 선언
string name = "김철수";
string greeting = "안녕하세요!";
string empty = "";  // 빈 문자열

Console.WriteLine(name);
Console.WriteLine(greeting);

// 문자열 연결
string fullGreeting = greeting + " " + name + "님";
Console.WriteLine(fullGreeting);

// 출력:
// 김철수
// 안녕하세요!
// 안녕하세요! 김철수님
```

**주요 특징:**
- **전역 문자 지원**: Unicode 표준을 완벽하게 지원하여 한글, 영어, 중국어, 아랍어, 이모지 등 모든 문자를 표현할 수 있습니다.
- **불변성(Immutability)**: 한 번 생성된 문자열의 내용은 변경할 수 없으며, 수정 연산은 새로운 문자열 객체를 생성합니다.
- **참조 타입**: 힙(Heap) 메모리에 저장되며, 변수는 실제 데이터가 아닌 데이터의 주소(참조)를 저장합니다.
- **연산자 오버로딩**: `+` 연산자를 사용한 문자열 연결, `==` 연산자를 사용한 내용 비교가 직관적으로 가능합니다.
- **리터럴 표현**: 큰따옴표(`"`)로 감싸서 표현하며, 작은따옴표(`'`)는 단일 문자(`char`) 타입에 사용됩니다.
- **이스케이프 시퀀스**: `\n`(줄바꿈), `\t`(탭), `\\`(백슬래시), `\"`(큰따옴표) 등 특수 문자를 표현할 수 있습니다.

**문자열 특수 표기법:**

```csharp
// 일반 문자열
string normal = "Hello, World!";

// 이스케이프 시퀀스 사용
string withNewline = "첫 번째 줄\n두 번째 줄";
string withTab = "이름:\t김철수";
string withQuote = "그는 \"안녕\"이라고 말했다.";

// 축자 문자열(Verbatim String) - @ 접두사
string path = @"C:\Users\Username\Documents";  // 백슬래시를 이스케이프할 필요 없음
string multiline = @"첫 번째 줄
두 번째 줄
세 번째 줄";  // 줄바꿈을 그대로 포함

// 보간 문자열(Interpolated String) - $ 접두사
string name = "홍길동";
int age = 30;
string message = $"{name}님의 나이는 {age}세입니다.";
```

#### bool (불린형)

`bool`은 Boolean(불린)의 약자로, 논리적 참(true) 또는 거짓(false) 두 가지 값만을 가질 수 있는 논리 타입입니다. 영국의 수학자 조지 불(George Boole)의 불 대수(Boolean Algebra)에서 유래한 이름으로, 프로그래밍에서 의사결정과 흐름 제어의 핵심적인 역할을 담당합니다.

**논리 타입의 중요성:**

불린 타입은 크기는 작지만(단 1비트의 정보만 표현), 프로그램의 흐름을 제어하는 데 있어 가장 중요한 타입입니다. 모든 조건문(`if`, `while`, `for`)과 논리 연산의 기반이 되며, 프로그램이 상황에 따라 다르게 동작할 수 있도록 만드는 핵심 요소입니다.

**메모리와 표현:**
- 크기: 1바이트 (실제로는 1비트 정보만 필요하지만, 메모리 정렬을 위해 1바이트 할당)
- 값: `true` 또는 `false` (대소문자 구분 필수)
- 기본값: `false`
- .NET 타입: `System.Boolean`

참(true) 또는 거짓(false) 두 가지 값만 가질 수 있는 논리 타입입니다.

```csharp
// 불린형 변수
bool isStudent = true;
bool isRaining = false;
bool hasLicense = true;

Console.WriteLine($"학생입니까? {isStudent}");
Console.WriteLine($"비가 오나요? {isRaining}");

// 조건에 따른 출력
if (isStudent)
{
    Console.WriteLine("학생 할인이 적용됩니다.");
}

// 출력:
// 학생입니까? True
// 비가 오나요? False
// 학생 할인이 적용됩니다.
```

**주요 특징:**
- **이진 값 전용**: `true` 또는 `false`만 가능하며, C/C++와 달리 숫자(0, 1)와 자동 변환되지 않습니다.
- **논리 연산의 기초**: 조건문, 반복문, 논리 연산(`&&`, `||`, `!`)의 핵심 데이터 타입입니다.
- **비교 연산의 결과**: 모든 비교 연산(`==`, `!=`, `>`, `<`, `>=`, `<=`)의 결과는 bool 타입입니다.
- **타입 안전성**: C#에서는 정수를 불린으로 암시적 변환할 수 없어, `if (1)` 같은 코드는 컴파일 오류가 발생합니다.
- **조건 표현식**: 삼항 연산자(`? :`)와 null 병합 연산자(`??`)의 조건부로 사용됩니다.

**불린 값의 생성 방법:**

```csharp
// 직접 리터럴 할당
bool isReady = true;
bool isCompleted = false;

// 비교 연산의 결과
bool isAdult = age >= 18;
bool isPositive = number > 0;
bool isEqual = value1 == value2;

// 논리 연산의 결과
bool canProceed = isReady && !isCompleted;
bool shouldWait = !isReady || isProcessing;

// 메서드 반환값 (관례적으로 Is, Has, Can 등으로 시작)
bool isValid = ValidateInput(input);
bool hasPermission = CheckPermission(user);
bool canAccess = IsAuthorized() && HasValidToken();
```

**불린 명명 규칙:**

불린 변수는 일반적으로 질문에 대한 답변 형태로 명명합니다:
- `is + 형용사`: `isReady`, `isVisible`, `isEmpty`
- `has + 명사`: `hasChildren`, `hasPermission`, `hasError`
- `can + 동사`: `canRead`, `canWrite`, `canDelete`
- `should + 동사`: `shouldUpdate`, `shouldRetry`, `shouldNotify`
- `true` 또는 `false` 값만 가능
- 조건문, 반복문에서 주로 사용
- 비교 연산의 결과값

#### double (실수형)

`double`은 Double-Precision Floating-Point의 약자로, 소수점을 포함하는 실수를 저장하는 64비트 부동소수점(Floating-Point) 타입입니다. IEEE 754 표준을 따르며, 매우 큰 수부터 매우 작은 수까지 넓은 범위의 값을 근사적으로 표현할 수 있습니다.

**부동소수점의 이해:**

컴퓨터에서 실수를 표현하는 방식은 과학적 표기법과 유사합니다. 예를 들어, 숫자 123.45는 1.2345 × 10²로 표현할 수 있습니다. 부동소수점은 이처럼 가수(Mantissa)와 지수(Exponent)를 분리하여 저장함으로써, 제한된 비트 수로 매우 넓은 범위의 수를 표현합니다.

**메모리 구조:**
- 크기: 64비트 (8바이트)
- 정밀도: 약 15-17자리의 10진수
- 범위: ±5.0 × 10⁻³²⁴ ~ ±1.7 × 10³⁰⁸
- 특수 값: `NaN` (Not a Number), `PositiveInfinity`, `NegativeInfinity`
- .NET 타입: `System.Double`

**부동소수점의 한계:**

부동소수점 연산은 본질적으로 근사치이므로, 정확한 값이 아닌 "충분히 가까운" 값을 표현합니다. 이는 다음과 같은 중요한 함의를 가집니다:

```csharp
double a = 0.1 + 0.2;
double b = 0.3;
Console.WriteLine(a == b);  // False! (예상과 다름)
Console.WriteLine(a);        // 0.30000000000000004

// 부동소수점 비교 시에는 오차 범위를 고려해야 함
const double EPSILON = 1e-10;
bool isEqual = Math.Abs(a - b) < EPSILON;
```

이러한 특성 때문에, 금융 계산이나 정확한 10진수 연산이 필요한 경우에는 `decimal` 타입을 사용해야 합니다.

소수점을 포함하는 숫자를 저장하는 64비트 부동소수점 타입입니다.

```csharp
// 실수형 변수
double pi = 3.14159;
double price = 1500.50;
double temperature = 36.5;

Console.WriteLine($"원주율: {pi}");
Console.WriteLine($"가격: {price}원");
Console.WriteLine($"체온: {temperature}도");

// 계산
double radius = 5.0;
double area = pi * radius * radius;
Console.WriteLine($"반지름 {radius}인 원의 넓이: {area}");

// 출력:
// 원주율: 3.14159
// 가격: 1500.5원
// 체온: 36.5도
// 반지름 5인 원의 넓이: 78.53975
```

**주요 특징:**
- **넓은 표현 범위**: 과학적 계산이 필요한 매우 크거나 작은 수를 표현할 수 있습니다.
- **정밀도 한계**: 약 15-17자리의 10진수 정밀도를 가지며, 이를 초과하면 정확도가 떨어집니다.
- **근사치 표현**: 모든 10진 소수를 정확히 표현할 수 없으며, 이진 근사치로 저장됩니다.
- **과학적 표기법**: `1.5e3` (1.5 × 10³ = 1500)과 같은 표기법을 지원합니다.
- **특수 값 지원**: `double.NaN`, `double.PositiveInfinity`, `double.NegativeInfinity`, `double.Epsilon` 등의 특수 값이 정의되어 있습니다.

**실수 타입 비교:**

| 타입 | 크기 | 정밀도 | 범위 | 용도 |
|------|------|--------|------|------|
| `float` | 32비트 | 7자리 | ±1.5 × 10⁻⁴⁵ ~ ±3.4 × 10³⁸ | 단정밀도, 그래픽, 게임 |
| `double` | 64비트 | 15-17자리 | ±5.0 × 10⁻³²⁴ ~ ±1.7 × 10³⁰⁸ | 일반 과학 계산 (기본값) |
| `decimal` | 128비트 | 28-29자리 | ±1.0 × 10⁻²⁸ ~ ±7.9 × 10²⁸ | 금융, 정확한 10진수 계산 |

**사용 가이드:**
- **float**: 메모리가 중요하고 정밀도가 덜 중요한 경우 (3D 그래픽, 대량 데이터)
- **double**: 일반적인 과학/공학 계산, 기본 실수 타입
- **decimal**: 금융 계산, 정확한 10진수가 필요한 경우 (통화, 세금, 이자)

### 2.1.2 var 키워드와 타입 추론

C# 3.0에서 도입된 `var` 키워드는 컴파일러의 타입 추론(Type Inference) 기능을 활용하여 개발자가 명시적으로 타입을 지정하지 않아도 변수를 선언할 수 있게 해줍니다. 이는 코드의 간결성을 높이면서도, C#의 강력한 정적 타입 시스템의 안전성을 그대로 유지하는 현대적인 프로그래밍 패러다임입니다.

**타입 추론의 메커니즘:**

`var`를 사용하면 컴파일러는 할당된 값(또는 표현식)의 타입을 분석하여, 컴파일 시점에 변수의 타입을 자동으로 결정합니다. 이는 단순히 타입 표기를 생략하는 것이 아니라, 컴파일러가 정확한 타입을 추론하여 내부적으로는 명시적 타입 선언과 동일하게 처리합니다. 따라서 런타임 성능에는 전혀 영향이 없으며, 완전히 타입 안전합니다.

```csharp
// 다음 두 선언은 완전히 동일합니다 (컴파일된 코드는 identical)
int number = 42;
var number = 42;  // 컴파일러가 int로 추론

// 복잡한 타입의 경우 가독성이 향상됩니다
Dictionary<string, List<int>> explicitType = new Dictionary<string, List<int>>();
var implicitType = new Dictionary<string, List<int>>();  // 더 간결
```

**타입 추론의 이점:**

1. **코드 간결성**: 긴 타입 이름을 반복하지 않아 코드가 더 읽기 쉬워집니다.
2. **리팩토링 용이성**: 타입을 변경할 때 한 곳만 수정하면 됩니다.
3. **LINQ와의 시너지**: 익명 타입이나 복잡한 쿼리 결과를 다룰 때 필수적입니다.
4. **타입 안전성 유지**: 컴파일 시점에 타입이 결정되므로, 동적 타입 언어와 달리 타입 오류를 미리 발견할 수 있습니다.

`var` 키워드를 사용하면 컴파일러가 할당된 값을 보고 자동으로 변수의 타입을 추론합니다.

```csharp
// var를 사용한 타입 추론
var number = 10;           // int로 추론
var message = "Hello";     // string으로 추론
var isValid = true;        // bool로 추론
var price = 99.99;         // double로 추론

Console.WriteLine($"number의 타입: {number.GetType()}");
Console.WriteLine($"message의 타입: {message.GetType()}");

// 출력:
// number의 타입: System.Int32
// message의 타입: System.String
```

**var 사용 규칙:**
- **지역 변수 전용**: 메서드 내부의 지역 변수에서만 사용 가능하며, 필드, 속성, 매개변수에는 사용할 수 없습니다.
- **즉시 초기화 필수**: 선언과 동시에 값을 할당해야 하며, `var x;`와 같은 선언만으로는 타입을 추론할 수 없습니다.
- **null 리터럴 불가**: `var x = null;`은 타입을 결정할 수 없으므로 컴파일 오류가 발생합니다.
- **컴파일 타임 결정**: 한 번 추론된 타입은 변경할 수 없으며, 다른 타입의 값을 할당하면 오류가 발생합니다.
- **명시성 우선**: 타입이 명확하지 않거나, 가독성을 해칠 우려가 있다면 명시적 타입 선언을 사용하는 것이 좋습니다.

```csharp
// 올바른 사용
var name = "홍길동";

// 잘못된 사용
// var age;              // ❌ 초기화 없이 선언 불가
// var value = null;     // ❌ null로는 타입 추론 불가
```

**언제 var를 사용할까?**

`var` 사용 여부는 종종 논쟁의 대상이 되며, 팀마다 코딩 스타일이 다를 수 있습니다. 다음은 일반적으로 받아들여지는 가이드라인입니다:

**var 사용이 권장되는 경우:**
- 오른쪽 할당문에서 타입이 명확한 경우
  ```csharp
  var customer = new Customer();  // 명확함
  var count = GetItemCount();     // 메서드 이름에서 int임을 유추 가능
  ```
- 익명 타입을 다룰 때 (필수)
  ```csharp
  var person = new { Name = "홍길동", Age = 30 };
  ```
- LINQ 쿼리 결과를 다룰 때
  ```csharp
  var results = from item in items
                where item.Price > 100
                select item;
  ```
- 긴 제네릭 타입을 반복하고 싶지 않을 때
  ```csharp
  var dictionary = new Dictionary<string, List<int>>();
  ```

**명시적 타입이 권장되는 경우:**
- 오른쪽 할당문만으로는 타입이 불분명한 경우
  ```csharp
  int count = GetCount();  // GetCount()의 반환 타입이 명확하지 않을 수 있음
  ```
- 기본 타입을 다룰 때 (의견이 갈림)
  ```csharp
  int age = 25;      // 일부는 이렇게 선호
  var age = 25;      // 일부는 이렇게 선호
  ```
- 인터페이스나 기본 클래스 타입으로 받고 싶을 때
  ```csharp
  IEnumerable<int> numbers = GetNumbers();  // 특정 인터페이스로 받음
  ```

Microsoft의 공식 C# 코딩 규칙은 타입이 명확한 경우 `var` 사용을 권장하며, 타입을 명시하는 것이 가독성에 도움이 될 때는 명시적 타입을 사용하라고 조언합니다.

### 2.1.3 상수 (const)

상수(Constant)는 프로그램 실행 중에 절대 변경되지 않는 값을 저장하는 특별한 종류의 변수입니다. `const` 키워드로 선언되며, 컴파일 타임에 값이 결정되어 IL(Intermediate Language) 코드에 직접 임베딩됩니다. 이는 프로그램의 의미를 명확하게 하고, 실수로 인한 값 변경을 방지하며, 컴파일러 최적화를 가능하게 하는 중요한 프로그래밍 기법입니다.

**상수의 근본적 특성:**

상수는 단순히 "변경되지 않는 변수"가 아닙니다. `const`로 선언된 값은 컴파일 시점에 완전히 해결되어, 실행 파일 내에 리터럴로 대체됩니다. 이는 다음과 같은 중요한 의미를 가집니다:

1. **컴파일 타임 상수**: 값은 컴파일 시점에 알려져야 하며, 런타임에 계산될 수 없습니다.
2. **인라인 최적화**: 상수를 사용하는 모든 곳에서 실제 값으로 직접 대체됩니다.
3. **크로스 어셈블리 주의**: 다른 어셈블리(DLL)에서 상수를 참조하면, 그 어셈블리를 재컴파일하지 않는 한 변경사항이 반영되지 않습니다.

상수는 프로그램 실행 중 값이 변경되지 않는 변수입니다. `const` 키워드를 사용하여 선언합니다.

```csharp
// 상수 선언
const double PI = 3.14159;
const int MAX_STUDENTS = 30;
const string COMPANY_NAME = "ABC Corporation";

Console.WriteLine($"원주율: {PI}");
Console.WriteLine($"최대 학생 수: {MAX_STUDENTS}");
Console.WriteLine($"회사명: {COMPANY_NAME}");

// PI = 3.14;  // ❌ 컴파일 오류: 상수는 값을 변경할 수 없음

// 출력:
// 원주율: 3.14159
// 최대 학생 수: 30
// 회사명: ABC Corporation
```

**상수의 특징:**
- **불변성 보장**: 선언 후 어떤 방법으로도 값을 변경할 수 없으며, 시도 시 컴파일 오류가 발생합니다.
- **컴파일 타임 초기화**: 선언과 동시에 초기화해야 하며, 값은 컴파일 시점에 알려진 리터럴이어야 합니다.
- **컴파일 타임 평가**: 상수 표현식은 컴파일 시점에 평가되므로, 런타임 계산이 없어 성능이 최적화됩니다.
- **정적 특성**: `const` 필드는 암묵적으로 `static`이며, 타입 차원에서 공유됩니다.
- **제한된 타입**: 기본 타입(`int`, `bool`, `double`, `string` 등)과 `enum`, `null` 참조만 `const`로 선언할 수 있습니다.
- **범위 규칙**: 지역 상수(메서드 내부)와 클래스 수준 상수 모두 선언 가능합니다.

**상수 vs 변수:**

```csharp
// 변수 - 값 변경 가능
var count = 10;
count = 20;  // ✅ 가능
count++;     // ✅ 가능

// 상수 - 값 변경 불가
const int MAX_COUNT = 100;
// MAX_COUNT = 200;  // ❌ 컴파일 오류: "상수에 할당할 수 없습니다"
// MAX_COUNT++;      // ❌ 컴파일 오류
```

**상수 vs readonly:**

C#에서는 변경 불가능한 값을 표현하는 두 가지 주요 키워드가 있습니다:

```csharp
// const - 컴파일 타임 상수
public const double PI = 3.14159;  // 값이 코드에 직접 삽입됨

// readonly - 런타임 상수
public readonly DateTime CreatedAt = DateTime.Now;  // 런타임에 초기화

// const는 static이 암시됨
Console.WriteLine(MyClass.PI);  // 타입 이름으로 접근

// readonly는 인스턴스마다 다를 수 있음
var obj1 = new MyClass();  // obj1.CreatedAt
var obj2 = new MyClass();  // obj2.CreatedAt (다른 시간)
```

| 특성 | `const` | `readonly` |
|------|---------|------------|
| 초기화 시점 | 컴파일 타임 | 런타임 (생성자에서 가능) |
| 값 결정 | 리터럴만 가능 | 모든 표현식 가능 |
| 범위 | 암묵적 static | 인스턴스 또는 static |
| 타입 제한 | 기본 타입, string, null | 모든 타입 |
| 성능 | 인라인 대체 (빠름) | 메모리 참조 (약간 느림) |
| 버전 관리 | 재컴파일 필요 | 재컴파일 불필요 |

**상수 명명 규칙:**

C# 커뮤니티에서는 상수 명명에 대해 두 가지 주요 스타일이 사용됩니다:

1. **PascalCase** (Microsoft 공식 가이드라인)
   ```csharp
   public const int MaxValue = 100;
   public const string ApplicationName = "MyApp";
   ```

2. **UPPER_SNAKE_CASE** (C/C++ 스타일, 가독성 강조)
   ```csharp
   public const int MAX_VALUE = 100;
   public const string APPLICATION_NAME = "MyApp";
   ```

Microsoft의 공식 C# 코딩 규칙은 **PascalCase**를 권장하지만, 많은 개발자들은 상수를 일반 변수와 시각적으로 구분하기 위해 **UPPER_SNAKE_CASE**를 선호합니다. 중요한 것은 프로젝트 내에서 일관성을 유지하는 것입니다.

**상수 사용 예제:**

```csharp
// 수학 상수
public const double PI = 3.14159265359;
public const double E = 2.71828182846;

// 설정 값
public const int MAX_LOGIN_ATTEMPTS = 3;
public const int SESSION_TIMEOUT_MINUTES = 30;
public const string DEFAULT_LANGUAGE = "ko-KR";

// 상태 코드
public const int HTTP_OK = 200;
public const int HTTP_NOT_FOUND = 404;
public const int HTTP_SERVER_ERROR = 500;

// 유효성 검증
public const int MIN_PASSWORD_LENGTH = 8;
public const int MAX_USERNAME_LENGTH = 20;

// 상수 표현식 (다른 상수를 사용한 계산)
public const int SECONDS_PER_MINUTE = 60;
public const int SECONDS_PER_HOUR = SECONDS_PER_MINUTE * 60;  // ✅ 컴파일 타임에 계산
public const int SECONDS_PER_DAY = SECONDS_PER_HOUR * 24;
```

---

## 2.2 연산자

연산자(Operator)는 프로그래밍 언어의 핵심 구성 요소로, 데이터에 대해 특정 작업을 수행하는 기호 또는 키워드입니다. 수학에서 `+`, `-`, `×`, `÷`와 같은 기호들이 숫자 간의 연산을 표현하듯이, 프로그래밍 언어의 연산자는 변수와 값들에 대한 계산, 비교, 논리 판단 등 다양한 조작을 가능하게 합니다.

C#은 풍부하고 직관적인 연산자 집합을 제공하여, 복잡한 로직을 간결하고 가독성 있게 표현할 수 있게 해줍니다. 연산자는 크게 **산술 연산자**(수학적 계산), **비교 연산자**(값의 대소 비교), **논리 연산자**(조건의 결합), **대입 연산자**(값의 할당) 등으로 분류되며, 각각은 프로그램 로직 구성에 필수적인 역할을 담당합니다.

**연산자의 핵심 개념:**

- **피연산자(Operand)**: 연산자가 작용하는 대상(변수, 리터럴, 표현식)
- **항수(Arity)**: 연산자가 취하는 피연산자의 개수
  - 단항 연산자(Unary): `!`, `++`, `--`, `-` (부호)
  - 이항 연산자(Binary): `+`, `-`, `*`, `/`, `==`, `&&` (가장 일반적)
  - 삼항 연산자(Ternary): `? :` (조건 연산자)
- **우선순위(Precedence)**: 여러 연산자가 함께 사용될 때의 실행 순서
- **결합성(Associativity)**: 같은 우선순위의 연산자들이 나열될 때의 평가 방향

연산자는 변수나 값에 대해 연산을 수행하는 기호입니다. C#은 다양한 종류의 연산자를 제공합니다.

### 2.2.1 산술 연산자

산술 연산자(Arithmetic Operators)는 수학적 계산을 수행하는 연산자로, 프로그래밍에서 가장 기본이 되고 직관적인 연산자 군입니다. 덧셈, 뺄셈, 곱셈, 나눗셈과 같은 사칙연산부터 나머지 연산까지, 일상적인 수학 계산을 코드로 표현할 수 있게 해줍니다.

**기본 산술 연산자의 동작:**

C#의 산술 연산자는 수학의 표준 표기법을 최대한 따르면서도, 프로그래밍 특유의 특성을 반영합니다. 특히 정수 나눗셈과 실수 나눗셈의 차이, 나머지 연산의 활용 등은 프로그래밍에서 매우 중요한 개념입니다.

수학적 계산을 수행하는 연산자입니다.

```csharp
// 산술 연산자
int a = 10;
int b = 3;

Console.WriteLine($"a + b = {a + b}");  // 덧셈: 13
Console.WriteLine($"a - b = {a - b}");  // 뺄셈: 7
Console.WriteLine($"a * b = {a * b}");  // 곱셈: 30
Console.WriteLine($"a / b = {a / b}");  // 나눗셈(몫): 3
Console.WriteLine($"a % b = {a % b}");  // 나머지: 1

// 실수 나눗셈
double x = 10.0;
double y = 3.0;
Console.WriteLine($"x / y = {x / y}");  // 3.3333333333333335
```

**주요 산술 연산자:**

| 연산자 | 의미 | 예제 | 결과 | 설명 |
|--------|------|------|------|------|
| `+` | 덧셈 (Addition) | `5 + 3` | `8` | 두 피연산자의 합 |
| `-` | 뺄셈 (Subtraction) | `5 - 3` | `2` | 첫 번째에서 두 번째를 뺌 |
| `*` | 곱셈 (Multiplication) | `5 * 3` | `15` | 두 피연산자의 곱 |
| `/` | 나눗셈 (Division) | `5 / 3` | `1` (int), `1.666...` (double) | 나눗셈의 몫 또는 결과 |
| `%` | 나머지 (Modulus) | `5 % 3` | `2` | 나눗셈의 나머지 |
| `-` | 부호 반전 (Unary Minus) | `-5` | `-5` | 단항 연산자, 부호 변경 |
| `+` | 부호 유지 (Unary Plus) | `+5` | `5` | 단항 연산자, 보통 생략 |

**정수 나눗셈 vs 실수 나눗셈:**

C#에서 나눗셈 연산의 결과는 피연산자의 타입에 따라 달라집니다. 이는 초보자들이 자주 실수하는 부분이므로 명확히 이해해야 합니다.

```csharp
// 정수 나눗셈 - 소수점 이하 절삭(truncation)
int a = 10;
int b = 3;
int result1 = a / b;  // 3 (10 ÷ 3 = 3.333... → 3)

// 실수 나눗셈 - 정확한 결과
double x = 10.0;
double y = 3.0;
double result2 = x / y;  // 3.3333333333333335

// 혼합 타입 - 하나라도 실수면 실수 나눗셈
double result3 = 10 / 3.0;  // 3.3333333333333335
double result4 = 10.0 / 3;  // 3.3333333333333335

// 명시적 캐스팅
double result5 = (double)10 / 3;  // 3.3333333333333335
```

**나머지 연산자의 활용:**

나머지 연산자(`%`, Modulus)는 나눗셈 후 남은 나머지를 반환합니다. 이는 주기적인 패턴, 짝수/홀수 판별, 범위 제한 등 다양한 상황에서 유용하게 사용됩니다.

```csharp
// 짝수/홀수 판별
int number = 7;
bool isEven = (number % 2 == 0);  // false (홀수)
bool isOdd = (number % 2 != 0);   // true

// 주기적 패턴 (0, 1, 2, 3, 0, 1, 2, 3, ...)
for (int i = 0; i < 10; i++)
{
    int pattern = i % 4;  // 4로 나눈 나머지 → 0~3 반복
}

// 배열 인덱스를 순환시키기
int[] colors = { "Red", "Green", "Blue" };
int index = userInput % colors.Length;  // 0~2 범위로 제한

// 마지막 자릿수 추출
int lastDigit = 12345 % 10;  // 5
```

**복합 대입 연산자:**

복합 대입 연산자(Compound Assignment Operators)는 산술 연산과 대입을 하나로 결합한 연산자로, 코드를 더 간결하고 읽기 쉽게 만들어줍니다.

```csharp
int num = 10;

num += 5;  // num = num + 5;  결과: 15
num -= 3;  // num = num - 3;  결과: 12
num *= 2;  // num = num * 2;  결과: 24
num /= 4;  // num = num / 4;  결과: 6
num %= 4;  // num = num % 4;  결과: 2

Console.WriteLine($"최종 값: {num}");  // 2
```

**완전한 복합 대입 연산자 목록:**

| 연산자 | 동일 표현 | 예제 | 설명 |
|--------|----------|------|------|
| `+=` | `x = x + y` | `x += 5` | 덧셈 후 대입 |
| `-=` | `x = x - y` | `x -= 3` | 뺄셈 후 대입 |
| `*=` | `x = x * y` | `x *= 2` | 곱셈 후 대입 |
| `/=` | `x = x / y` | `x /= 4` | 나눗셈 후 대입 |
| `%=` | `x = x % y` | `x %= 3` | 나머지 후 대입 |

복합 대입 연산자는 다음과 같은 이점을 제공합니다:
- **간결성**: 변수 이름을 반복하지 않아도 됨
- **가독성**: 의도가 명확하게 드러남
- **오류 감소**: 변수 이름을 잘못 타이핑할 가능성이 줄어듦
- **최적화 가능성**: 컴파일러가 더 효율적인 코드를 생성할 수 있음

**증감 연산자:**

증감 연산자(Increment/Decrement Operators)는 변수의 값을 1씩 증가시키거나 감소시키는 특수한 단항 연산자입니다. 반복문의 카운터, 인덱스 조작 등에서 매우 빈번하게 사용됩니다.

```csharp
int count = 5;

count++;  // 후위 증가(Post-increment): count = count + 1
Console.WriteLine(count);  // 6

++count;  // 전위 증가(Pre-increment): count = count + 1
Console.WriteLine(count);  // 7

count--;  // 후위 감소(Post-decrement): count = count - 1
Console.WriteLine(count);  // 6

--count;  // 전위 감소(Pre-decrement): count = count - 1
Console.WriteLine(count);  // 5
```

**전위 vs 후위 연산자의 중요한 차이:**

증감 연산자의 위치(변수 앞 또는 뒤)에 따라 동작 순서가 달라집니다:

```csharp
// 후위 증가 (Post-increment): 먼저 값을 사용하고, 그 다음에 증가
int x = 5;
int result1 = x++;  // result1 = 5, x = 6 (값을 먼저 반환한 후 증가)
Console.WriteLine($"result1: {result1}, x: {x}");  // result1: 5, x: 6

// 전위 증가 (Pre-increment): 먼저 증가하고, 그 다음에 값을 사용
int y = 5;
int result2 = ++y;  // result2 = 6, y = 6 (먼저 증가한 후 값을 반환)
Console.WriteLine($"result2: {result2}, y: {y}");  // result2: 6, y: 6

// 독립적으로 사용할 때는 차이 없음
int a = 5;
a++;     // a는 6
++a;     // a는 7 (둘 다 동일한 최종 결과)
```

**실용적인 예제:**

```csharp
// 배열 순회 시 인덱스 증가
int[] numbers = { 10, 20, 30, 40, 50 };
int index = 0;
Console.WriteLine(numbers[index++]);  // 10 출력, 그 다음 index는 1
Console.WriteLine(numbers[index++]);  // 20 출력, 그 다음 index는 2

// while 루프의 카운터
int counter = 0;
while (counter < 5)
{
    Console.WriteLine($"반복 {++counter}");  // 1, 2, 3, 4, 5
}

// 카운트다운
int countdown = 10;
while (countdown > 0)
{
    Console.WriteLine(countdown--);  // 10, 9, 8, ..., 1
}
```

**주의사항:**

증감 연산자를 복잡하게 사용하면 코드의 가독성이 떨어질 수 있습니다:

```csharp
// 나쁜 예 - 혼란스러움
int n = 5;
int result = n++ + ++n;  // ❌ 동작은 하지만 이해하기 어려움

// 좋은 예 - 명확함
int n = 5;
int first = n;
n++;
n++;
int result = first + n;  // ✅ 의도가 명확함
```

### 2.2.2 비교 연산자

비교 연산자(Comparison Operators 또는 Relational Operators)는 두 값의 크기나 동등성을 비교하여 불린(bool) 값(`true` 또는 `false`)을 반환하는 이항 연산자입니다. 조건문(`if`, `while`)과 함께 사용되어 프로그램의 흐름을 제어하는 핵심적인 역할을 담당하며, 의사결정 로직의 기반이 됩니다.

**비교 연산자의 본질:**

비교 연산은 프로그래밍에서 "조건"을 표현하는 가장 기본적인 방법입니다. "나이가 18세 이상인가?", "재고가 충분한가?", "비밀번호가 일치하는가?"와 같은 질문을 코드로 표현할 때 비교 연산자를 사용합니다. 모든 비교 연산의 결과는 반드시 `bool` 타입이며, 이는 곧바로 조건문의 조건식으로 사용될 수 있습니다.

두 값을 비교하여 `true` 또는 `false`를 반환하는 연산자입니다.

```csharp
int x = 10;
int y = 20;

Console.WriteLine($"x == y: {x == y}");  // 같음: False
Console.WriteLine($"x != y: {x != y}");  // 같지 않음: True
Console.WriteLine($"x > y: {x > y}");    // 크다: False
Console.WriteLine($"x < y: {x < y}");    // 작다: True
Console.WriteLine($"x >= y: {x >= y}");  // 크거나 같다: False
Console.WriteLine($"x <= y: {x <= y}");  // 작거나 같다: True
```

**주요 비교 연산자:**

| 연산자 | 의미 | 예제 | 결과 | 설명 |
|--------|------|------|------|------|
| `==` | 같다 (Equal to) | `5 == 5` | `true` | 두 값이 같으면 true |
| `!=` | 같지 않다 (Not equal to) | `5 != 3` | `true` | 두 값이 다르면 true |
| `>` | 크다 (Greater than) | `5 > 3` | `true` | 왼쪽이 오른쪽보다 크면 true |
| `<` | 작다 (Less than) | `5 < 3` | `false` | 왼쪽이 오른쪽보다 작으면 true |
| `>=` | 크거나 같다 (Greater than or equal to) | `5 >= 5` | `true` | 왼쪽이 오른쪽보다 크거나 같으면 true |
| `<=` | 작거나 같다 (Less than or equal to) | `5 <= 3` | `false` | 왼쪽이 오른쪽보다 작거나 같으면 true |

**중요한 개념: `==` vs `=`**

초보자들이 가장 흔하게 실수하는 부분 중 하나는 동등 비교 연산자(`==`)와 대입 연산자(`=`)를 혼동하는 것입니다:

```csharp
int x = 5;  // ✅ 대입: x에 5를 할당

// 비교
if (x == 5)  // ✅ 비교: x가 5와 같은지 확인
{
    Console.WriteLine("x는 5입니다");
}

// 흔한 실수
// if (x = 5)  // ❌ 컴파일 오류: bool이 필요한데 int가 옴
```

다행히 C#은 타입 안전성 덕분에 이러한 실수를 컴파일 시점에 잡아냅니다. C/C++에서는 `if (x = 5)`가 유효한 코드로 컴파일되어 심각한 버그를 유발할 수 있지만, C#에서는 컴파일 오류가 발생합니다.

**다양한 타입의 비교:**

```csharp
// 정수 비교
int age = 20;
bool isAdult = age >= 18;  // true

// 실수 비교 (주의: 부동소수점 오차)
double price1 = 10.0;
double price2 = 10.0;
bool isSame = price1 == price2;  // true (하지만 계산된 값의 경우 주의)

// 문자 비교
char letter = 'B';
bool isA = letter == 'A';  // false
bool isAfterA = letter > 'A';  // true (ASCII/Unicode 값 비교)
```

**문자열 비교:**

C#에서 문자열 비교는 내용(content) 기반으로 이루어지며, 대소문자를 구분합니다:

```csharp
string name1 = "Alice";
string name2 = "Bob";
string name3 = "Alice";
string name4 = "alice";  // 소문자

Console.WriteLine(name1 == name2);  // False (다른 문자열)
Console.WriteLine(name1 == name3);  // True (같은 내용)
Console.WriteLine(name1 != name2);  // True (다르므로)
Console.WriteLine(name1 == name4);  // False (대소문자 다름)

// 대소문자 무시 비교
bool isSameIgnoreCase = name1.Equals(name4, StringComparison.OrdinalIgnoreCase);
Console.WriteLine(isSameIgnoreCase);  // True

// 문자열의 사전식 순서 비교
Console.WriteLine("Apple".CompareTo("Banana"));  // 음수 (Apple이 먼저)
Console.WriteLine("Zebra".CompareTo("Apple"));   // 양수 (Zebra가 뒤)
```

**참조 타입 비교의 미묘함:**

문자열은 참조 타입이지만, `==` 연산자가 **오버로드(overloaded)**되어 참조가 아닌 내용을 비교합니다. 다른 참조 타입에서는 `==`이 기본적으로 참조를 비교하지만, 문자열은 예외입니다:

```csharp
// 문자열: 내용 비교
string str1 = "Hello";
string str2 = "Hello";
Console.WriteLine(str1 == str2);  // True (내용이 같음)

// 일반 객체: 참조 비교 (특별한 경우)
object obj1 = new object();
object obj2 = new object();
Console.WriteLine(obj1 == obj2);  // False (다른 객체)
```

### 2.2.3 논리 연산자

논리 연산자(Logical Operators)는 불린(bool) 값들을 결합하거나 반전시켜 복합적인 조건을 표현하는 연산자입니다. 조지 불(George Boole)의 불 대수(Boolean Algebra)에 기반을 두며, "그리고(AND)", "또는(OR)", "아니다(NOT)"와 같은 논리적 관계를 코드로 표현할 수 있게 해줍니다.

**논리 연산의 중요성:**

실제 프로그래밍에서는 단순한 조건보다는 여러 조건을 조합한 복합 조건이 필요한 경우가 훨씬 많습니다. "18세 이상이면서 학생증이 있는 경우", "관리자이거나 소유자인 경우", "로그인하지 않은 경우" 등의 조건을 표현하기 위해 논리 연산자는 필수적입니다.

불린(bool) 값을 대상으로 논리 연산을 수행합니다.

```csharp
bool isStudent = true;
bool hasID = false;

// AND 연산자 (&&): 둘 다 true일 때만 true
Console.WriteLine($"isStudent && hasID: {isStudent && hasID}");  // False

// OR 연산자 (||): 하나라도 true면 true
Console.WriteLine($"isStudent || hasID: {isStudent || hasID}");  // True

// NOT 연산자 (!): 반대 값
Console.WriteLine($"!isStudent: {!isStudent}");  // False
Console.WriteLine($"!hasID: {!hasID}");          // True
```

**주요 논리 연산자:**

| 연산자 | 의미 | 예제 | 결과 | 설명 |
|--------|------|------|------|------|
| `&&` | AND (논리곱, 그리고) | `true && false` | `false` | 둘 다 true일 때만 true |
| `||` | OR (논리합, 또는) | `true || false` | `true` | 하나라도 true면 true |
| `!` | NOT (논리 부정, 아니다) | `!true` | `false` | 불린 값을 반전 |

**AND 연산자 (`&&`) - 논리곱:**

AND 연산자는 **모든** 피연산자가 `true`일 때만 `true`를 반환합니다. 하나라도 `false`이면 결과는 `false`입니다.

**진리표 (Truth Table):**
| A | B | A && B |
|---|---|--------|
| false | false | false |
| false | true | false |
| true | false | false |
| true | true | true |

```csharp
bool isStudent = true;
bool hasID = false;

// 둘 다 true여야 함
Console.WriteLine($"isStudent && hasID: {isStudent && hasID}");  // False

// 실용 예제: 입장 가능 여부
int age = 20;
bool hasTicket = true;
bool canEnter = age >= 18 && hasTicket;  // true (18세 이상이면서 티켓이 있음)
```

**OR 연산자 (`||`) - 논리합:**

OR 연산자는 **하나라도** `true`이면 `true`를 반환합니다. 모두 `false`일 때만 결과가 `false`입니다.

**진리표:**
| A | B | A \|\| B |
|---|---|--------|
| false | false | false |
| false | true | true |
| true | false | true |
| true | true | true |

```csharp
bool isStudent = true;
bool hasID = false;

// 하나라도 true면 됨
Console.WriteLine($"isStudent || hasID: {isStudent || hasID}");  // True

// 실용 예제: 할인 적용 여부
bool isStudent = false;
int age = 70;
bool getDiscount = isStudent || age >= 65;  // true (학생이거나 65세 이상)
```

**NOT 연산자 (`!`) - 논리 부정:**

NOT 연산자는 불린 값을 반대로 바꾸는 단항 연산자입니다.

**진리표:**
| A | !A |
|---|-----|
| false | true |
| true | false |

```csharp
bool isReady = true;
bool isNotReady = !isReady;  // false

// 실용 예제
bool isLoggedIn = false;
if (!isLoggedIn)
{
    Console.WriteLine("로그인이 필요합니다.");
}

// 이중 부정 (가독성 저하, 피할 것)
bool result = !!true;  // true (하지만 혼란스러움)
```

**실용 예제:**

```csharp
int age = 20;
bool hasTicket = true;

// 18세 이상이고 티켓이 있으면 입장 가능
bool canEnter = age >= 18 && hasTicket;
Console.WriteLine($"입장 가능: {canEnter}");  // True

// 학생이거나 65세 이상이면 할인
bool isStudent = false;
int customerAge = 70;
bool getDiscount = isStudent || customerAge >= 65;
Console.WriteLine($"할인 적용: {getDiscount}");  // True
```

**단축 평가 (Short-circuit Evaluation):**

C#의 논리 연산자 `&&`와 `||`는 **단축 평가(Short-circuit Evaluation)** 또는 **최소 평가(Minimal Evaluation)**라는 최적화 기법을 사용합니다. 이는 결과를 결정하는 데 충분한 정보가 있으면 나머지 피연산자를 평가하지 않는 것을 의미하며, 성능 최적화와 안전한 코드 작성에 매우 유용합니다.

**AND 연산자 (`&&`)의 단축 평가:**
- 왼쪽 피연산자가 `false`이면 전체 결과는 무조건 `false`
- 따라서 오른쪽 피연산자를 평가할 필요가 없음 (실행하지 않음)

```csharp
// 첫 번째 조건이 false이므로 두 번째는 확인하지 않음
bool result1 = false && ExpensiveOperation();  // ExpensiveOperation() 호출 안 됨

// 실용 예제: null 체크
string text = null;
if (text != null && text.Length > 0)  // text가 null이면 text.Length는 평가하지 않음
{
    Console.WriteLine("텍스트가 있습니다");
}
// null일 때 text.Length를 평가하면 NullReferenceException 발생하지만, 단축 평가로 안전함
```

**OR 연산자 (`||`)의 단축 평가:**
- 왼쪽 피연산자가 `true`이면 전체 결과는 무조건 `true`
- 따라서 오른쪽 피연산자를 평가할 필요가 없음 (실행하지 않음)

```csharp
// 첫 번째 조건이 true이므로 두 번째는 확인하지 않음
bool result2 = true || ExpensiveOperation();  // ExpensiveOperation() 호출 안 됨

// 실용 예제: 기본값 설정
string GetUserName()
{
    string cachedName = cache.Get("userName");
    // 캐시에 있으면 캐시 사용, 없으면 데이터베이스 조회 (단축 평가 활용)
    return cachedName ?? database.GetUserName();
}
```

**단축 평가의 활용:**

```csharp
// 1. 안전한 null 체크
User user = GetUser();
if (user != null && user.IsActive && user.HasPermission("read"))
{
    // user가 null이면 뒤의 조건들은 평가되지 않아 안전함
}

// 2. 성능 최적화 - 저렴한 조건을 먼저 배치
if (isLocalCacheAvailable || QueryDatabase())
{
    // 로컬 캐시가 있으면 데이터베이스 쿼리를 실행하지 않음
}

// 3. 배열/리스트 범위 체크
int[] numbers = { 1, 2, 3 };
int index = 5;
if (index >= 0 && index < numbers.Length && numbers[index] > 0)
{
    // index가 유효하지 않으면 numbers[index] 접근을 시도하지 않음
}
```

**주의사항:**

단축 평가가 항상 발생하므로, 오른쪽 피연산자에 부작용(side effect)이 있는 코드를 넣으면 예상치 못한 동작이 발생할 수 있습니다:

```csharp
int count = 0;

// 나쁜 예 - count++가 실행될지 안 될지 불명확
if (SomeCondition() && count++ > 0)  // ❌ 부작용에 의존하는 코드
{
    // ...
}

// 좋은 예 - 부작용을 명시적으로 분리
count++;
if (SomeCondition() && count > 0)  // ✅ 명확함
{
    // ...
}
```

**비단축 연산자 (`&`, `|`):**

C#은 단축 평가를 하지 않는 논리 연산자도 제공합니다:
- `&` (비단축 AND): 양쪽 모두 항상 평가
- `|` (비단축 OR): 양쪽 모두 항상 평가

```csharp
// 양쪽 모두 실행됨
bool result = false & Method1();  // Method1()이 호출됨

// 일반적으로는 단축 평가 연산자(&&, ||)를 사용하는 것이 권장됨
```

### 2.2.4 대입 연산자

대입 연산자(Assignment Operators)는 변수에 값을 할당하는 연산자로, 프로그래밍에서 가장 기본적이면서도 빈번하게 사용되는 연산자입니다. 단순한 값 할당부터 산술 연산과 결합된 복합 대입까지, 다양한 형태로 코드의 간결성과 가독성을 높이는 데 기여합니다.

**기본 대입 연산자 (`=`):**

가장 기본적인 대입 연산자는 `=`로, 오른쪽의 값(또는 표현식의 결과)을 왼쪽 변수에 할당합니다. 수학의 등호와 달리, 프로그래밍에서는 "오른쪽 값을 왼쪽에 저장하라"는 명령을 의미합니다.

변수에 값을 할당하는 연산자입니다.

```csharp
// 기본 대입
int x = 10;
string name = "홍길동";

// 복합 대입 연산자
int a = 5;
a += 3;   // a = a + 3;   결과: 8
a -= 2;   // a = a - 2;   결과: 6
a *= 4;   // a = a * 4;   결과: 24
a /= 3;   // a = a / 3;   결과: 8
a %= 5;   // a = a % 5;   결과: 3

Console.WriteLine($"최종 a 값: {a}");
```

**모든 복합 대입 연산자:**

| 연산자 | 동일 표현 | 예제 |
|--------|----------|------|
| `+=` | `x = x + y` | `x += 5` |
| `-=` | `x = x - y` | `x -= 3` |
| `*=` | `x = x * y` | `x *= 2` |
| `/=` | `x = x / y` | `x /= 4` |
| `%=` | `x = x % y` | `x %= 3` |

**연쇄 대입:**

```csharp
int a, b, c;
a = b = c = 10;  // 모든 변수에 10 할당

Console.WriteLine($"a={a}, b={b}, c={c}");  // a=10, b=10, c=10
```

---

## 2.3 콘솔 입출력

콘솔 입출력은 사용자와 프로그램이 상호작용하는 가장 기본적인 방법입니다.

### 2.3.1 Console.WriteLine()

콘솔에 텍스트를 출력하고 자동으로 줄을 바꿉니다.

```csharp
// 기본 출력
Console.WriteLine("Hello, World!");
Console.WriteLine("안녕하세요!");

// 여러 줄 출력
Console.WriteLine("첫 번째 줄");
Console.WriteLine("두 번째 줄");
Console.WriteLine("세 번째 줄");

// 출력:
// Hello, World!
// 안녕하세요!
// 첫 번째 줄
// 두 번째 줄
// 세 번째 줄
```

**변수 출력:**

```csharp
int age = 25;
string name = "김철수";

Console.WriteLine(name);
Console.WriteLine(age);

// 문자열 연결로 출력
Console.WriteLine("이름: " + name);
Console.WriteLine("나이: " + age);

// 출력:
// 김철수
// 25
// 이름: 김철수
// 나이: 25
```

**Console.Write() vs Console.WriteLine():**

```csharp
// WriteLine - 줄바꿈 포함
Console.WriteLine("첫 번째");
Console.WriteLine("두 번째");

// Write - 줄바꿈 없음
Console.Write("첫 번째 ");
Console.Write("두 번째");
Console.WriteLine();  // 수동 줄바꿈

// 출력:
// 첫 번째
// 두 번째
// 첫 번째 두 번째
```

### 2.3.2 Console.ReadLine()

사용자로부터 입력을 받는 메서드입니다. 항상 `string` 타입으로 반환합니다.

```csharp
// 사용자 입력 받기
Console.Write("이름을 입력하세요: ");
string name = Console.ReadLine();

Console.Write("나이를 입력하세요: ");
string ageInput = Console.ReadLine();

Console.WriteLine($"안녕하세요, {name}님!");
Console.WriteLine($"입력하신 나이: {ageInput}");

// 실행 예:
// 이름을 입력하세요: 홍길동
// 나이를 입력하세요: 30
// 안녕하세요, 홍길동님!
// 입력하신 나이: 30
```

**타입 변환:**

`ReadLine()`은 항상 문자열을 반환하므로, 숫자로 사용하려면 변환이 필요합니다.

```csharp
// 문자열을 정수로 변환
Console.Write("숫자를 입력하세요: ");
string input = Console.ReadLine();
int number = int.Parse(input);

Console.WriteLine($"입력한 숫자 + 10 = {number + 10}");

// 더 안전한 변환 (TryParse)
Console.Write("나이를 입력하세요: ");
string ageInput = Console.ReadLine();

if (int.TryParse(ageInput, out int age))
{
    Console.WriteLine($"내년 나이: {age + 1}");
}
else
{
    Console.WriteLine("올바른 숫자를 입력해주세요.");
}
```

**다양한 타입 변환:**

```csharp
// 정수 변환
int num = int.Parse("123");

// 실수 변환
double price = double.Parse("99.99");

// 불린 변환
bool flag = bool.Parse("true");

Console.WriteLine($"숫자: {num}");
Console.WriteLine($"가격: {price}");
Console.WriteLine($"플래그: {flag}");
```

### 2.3.3 문자열 보간 (String Interpolation)

문자열 보간은 문자열 내에 변수나 표현식을 직접 삽입하는 현대적이고 가독성 높은 방법입니다.

```csharp
string name = "이영희";
int age = 28;
string city = "서울";

// 문자열 보간 사용 ($ 기호)
Console.WriteLine($"이름: {name}");
Console.WriteLine($"나이: {age}");
Console.WriteLine($"도시: {city}");

// 복잡한 문장
Console.WriteLine($"{name}님은 {age}살이며, {city}에 거주합니다.");

// 출력:
// 이름: 이영희
// 나이: 28
// 도시: 서울
// 이영희님은 28살이며, 서울에 거주합니다.
```

**표현식 사용:**

```csharp
int a = 10;
int b = 20;

// 중괄호 안에 표현식 사용 가능
Console.WriteLine($"a + b = {a + b}");
Console.WriteLine($"a * b = {a * b}");
Console.WriteLine($"a > b: {a > b}");

// 메서드 호출
string text = "hello";
Console.WriteLine($"대문자: {text.ToUpper()}");

// 출력:
// a + b = 30
// a * b = 200
// a > b: False
// 대문자: HELLO
```

**서식 지정:**

```csharp
double pi = 3.14159265;
int number = 42;

// 소수점 자릿수 지정
Console.WriteLine($"PI (소수점 2자리): {pi:F2}");

// 통화 형식
double price = 1234.56;
Console.WriteLine($"가격: {price:C}");

// 퍼센트 형식
double rate = 0.856;
Console.WriteLine($"비율: {rate:P1}");

// 출력:
// PI (소수점 2자리): 3.14
// 가격: ₩1,234.56
// 비율: 85.6%
```

**기존 방식과 비교:**

```csharp
string name = "김민수";
int age = 25;

// 방법 1: 문자열 연결 (오래된 방식)
Console.WriteLine("이름: " + name + ", 나이: " + age);

// 방법 2: String.Format (중간 방식)
Console.WriteLine(String.Format("이름: {0}, 나이: {1}", name, age));

// 방법 3: 문자열 보간 (현대적 방식, 권장)
Console.WriteLine($"이름: {name}, 나이: {age}");

// 모두 동일한 출력:
// 이름: 김민수, 나이: 25
```

---

## 2.4 주석 작성하기

주석(Comment)은 코드에 설명을 추가하는 방법입니다. 주석은 컴파일러가 무시하므로 프로그램 실행에 영향을 주지 않습니다.

### 단일 행 주석

`//`로 시작하는 한 줄 주석입니다.

```csharp
// 이것은 주석입니다.
Console.WriteLine("Hello");  // 코드 옆에도 주석 가능

// 변수 선언
int age = 25;

// 여러 줄을 주석 처리할 때
// 각 줄마다 // 를 붙입니다.
// 이렇게 말이죠.
```

### 여러 줄 주석

`/*`로 시작해서 `*/`로 끝나는 여러 줄 주석입니다.

```csharp
/*
   이것은 여러 줄 주석입니다.
   여러 줄에 걸쳐 설명을 작성할 수 있습니다.
   코드 블록을 임시로 비활성화할 때도 유용합니다.
*/

Console.WriteLine("Hello");

/*
Console.WriteLine("이 코드는 실행되지 않습니다");
Console.WriteLine("주석 처리되었기 때문입니다");
*/
```

### XML 문서 주석

`///`로 시작하는 문서화 주석으로, 코드의 API 문서를 자동 생성할 때 사용됩니다.

```csharp
/// <summary>
/// 두 정수의 합을 계산합니다.
/// </summary>
/// <param name="a">첫 번째 정수</param>
/// <param name="param name="b">두 번째 정수</param>
/// <returns>두 정수의 합</returns>
int Add(int a, int b)
{
    return a + b;
}
```

### 주석 작성 모범 사례

**좋은 주석:**

```csharp
// 사용자 나이를 검증 (18세 이상만 허용)
if (age >= 18)
{
    Console.WriteLine("입장 가능");
}

// TODO: 추후 이메일 검증 로직 추가 필요
string email = Console.ReadLine();

// HACK: 임시 해결책 - 추후 리팩토링 필요
const int TEMP_LIMIT = 100;
```

**피해야 할 주석:**

```csharp
// 나쁜 예: 코드를 그대로 설명
int x = 10;  // x에 10을 할당

// 나쁜 예: 명백한 내용 반복
Console.WriteLine("Hello");  // Hello 출력

// 좋은 예: 왜 그렇게 했는지 설명
int timeout = 30;  // API 응답 대기 시간 (초단위)
```

### 주석 사용 가이드

1. **코드 자체가 설명이 되도록 작성** - 명확한 변수명과 함수명 사용
2. **"왜"를 설명** - "무엇을" 하는지는 코드가 보여줌, "왜" 하는지를 주석으로 설명
3. **TODO, FIXME, HACK 등의 태그 활용** - 추후 작업이 필요한 부분 표시
4. **오래된 주석 제거** - 코드가 변경되면 관련 주석도 업데이트하거나 제거
5. **과도한 주석 지양** - 코드가 복잡하면 리팩토링 고려

```csharp
// 좋은 주석 예제
var maxRetries = 3;  // 네트워크 불안정을 고려한 재시도 횟수

// 나쁜 주석 예제
var x = 3;  // x는 3입니다
```

---

## 2장 정리 및 요약

이 장에서는 C# 프로그래밍의 기초 문법을 학습했습니다.

### 핵심 개념 정리

1. **변수와 데이터 타입**
   - 기본 타입: `int`, `string`, `bool`, `double`
   - `var` 키워드를 통한 타입 추론
   - `const`를 사용한 상수 선언

2. **연산자**
   - 산술 연산자: `+`, `-`, `*`, `/`, `%`
   - 비교 연산자: `==`, `!=`, `>`, `<`, `>=`, `<=`
   - 논리 연산자: `&&`, `||`, `!`
   - 대입 연산자: `=`, `+=`, `-=`, `*=`, `/=`, `%=`

3. **콘솔 입출력**
   - `Console.WriteLine()`: 출력 후 줄바꿈
   - `Console.ReadLine()`: 사용자 입력 받기
   - 문자열 보간: `$"text {variable}"`

4. **주석**
   - 단일 행: `//`
   - 여러 줄: `/* */`
   - XML 문서: `///`

### 실습 문제

#### 문제 1: 개인 정보 입력 및 출력

사용자로부터 이름, 나이, 도시를 입력받아 출력하는 프로그램을 작성하세요.

```csharp
Console.Write("이름을 입력하세요: ");
var name = Console.ReadLine();

Console.Write("나이를 입력하세요: ");
var ageInput = Console.ReadLine();
var age = int.Parse(ageInput);

Console.Write("거주 도시를 입력하세요: ");
var city = Console.ReadLine();

Console.WriteLine($"\n=== 입력하신 정보 ===");
Console.WriteLine($"이름: {name}");
Console.WriteLine($"나이: {age}세");
Console.WriteLine($"도시: {city}");
```

#### 문제 2: 간단한 계산기

두 숫자를 입력받아 사칙연산 결과를 모두 출력하는 프로그램을 작성하세요.

```csharp
Console.Write("첫 번째 숫자: ");
var num1 = double.Parse(Console.ReadLine());

Console.Write("두 번째 숫자: ");
var num2 = double.Parse(Console.ReadLine());

Console.WriteLine($"\n=== 연산 결과 ===");
Console.WriteLine($"{num1} + {num2} = {num1 + num2}");
Console.WriteLine($"{num1} - {num2} = {num1 - num2}");
Console.WriteLine($"{num1} × {num2} = {num1 * num2}");
Console.WriteLine($"{num1} ÷ {num2} = {num1 / num2}");
```

#### 문제 3: 성인 판별 프로그램

나이를 입력받아 성인인지 미성년자인지 판별하는 프로그램을 작성하세요.

```csharp
Console.Write("나이를 입력하세요: ");
var age = int.Parse(Console.ReadLine());

var isAdult = age >= 18;

Console.WriteLine($"입력 나이: {age}세");
Console.WriteLine($"성인 여부: {isAdult}");

if (isAdult)
{
    Console.WriteLine("성인입니다.");
}
else
{
    Console.WriteLine("미성년자입니다.");
}
```

### 다음 장 예고

3장 "제어문"에서는 프로그램의 흐름을 제어하는 방법을 학습합니다:
- 조건문 (`if`, `else`, `switch`)
- 반복문 (`for`, `while`, `foreach`)
- 분기문 (`break`, `continue`, `return`)

이를 통해 더욱 복잡하고 동적인 프로그램을 작성할 수 있게 됩니다!
