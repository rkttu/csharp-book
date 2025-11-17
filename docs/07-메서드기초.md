# 7장. 메서드 기초

6장까지 배운 내용으로도 간단한 프로그램을 만들 수 있지만, 코드가 길어지면 관리하기 어려워집니다. 같은 코드를 여러 번 작성하게 되고, 수정할 때도 여러 곳을 고쳐야 합니다. 이런 문제를 해결하는 것이 바로 **메서드(Method)**입니다.

메서드는 특정 작업을 수행하는 코드의 묶음입니다. 지금까지 우리는 `Console.WriteLine()`이나 `int.Parse()` 같은 메서드를 사용해왔습니다. 이제는 직접 메서드를 만들고 사용하는 방법을 배우겠습니다.

## 이 장에서 배울 내용

- 메서드 정의하고 호출하기
- 매개변수와 반환값 사용하기
- 메서드 오버로딩
- 선택적 매개변수와 명명된 인수
- 로컬 함수

---

## 7.1 메서드 정의와 호출

메서드는 특정 작업을 수행하는 코드 블록으로, 필요할 때마다 이름을 통해 호출(invocation)할 수 있습니다. 메서드를 사용하면 같은 코드를 반복해서 작성하지 않아도 되므로, 코드의 재사용성과 유지보수성이 크게 향상됩니다. 이는 소프트웨어 공학의 핵심 원칙인 **DRY(Don't Repeat Yourself)** 원칙을 실천하는 가장 기본적인 방법입니다.

**메서드 시그니처(Method Signature)의 이해:**

메서드의 기본 구조를 이해하기 전에, 먼저 메서드 시그니처라는 중요한 개념을 알아야 합니다. 메서드 시그니처는 메서드를 고유하게 식별하는 정보로, 메서드 이름과 매개변수의 타입 및 순서로 구성됩니다(반환 타입은 시그니처에 포함되지 않습니다). 이는 나중에 배울 메서드 오버로딩(Method Overloading)의 기초가 되는 개념으로, 컴파일러가 어떤 메서드를 호출해야 할지 결정하는 근거가 됩니다.

**메서드의 기본 구조(Anatomy of a Method):**

```csharp
[접근제한자] 반환타입 메서드이름(매개변수목록)
{
    // 메서드 본문(Method Body)
    // 메서드가 수행할 코드
    return 반환값;  // 반환타입이 void가 아닌 경우
}
```

각 구성 요소를 자세히 살펴보겠습니다:

1. **접근 제한자(Access Modifier)**: `public`, `private`, `protected`, `internal` 등으로 메서드의 가시성을 제어합니다. File-based App에서는 생략 가능하며, 기본값으로 처리됩니다.

2. **반환 타입(Return Type)**: 메서드가 반환하는 값의 데이터 타입입니다. 반환값이 없으면 `void`를 사용합니다. 이는 Pascal의 프로시저 개념을 반영한 것으로, 부작용만 수행하고 값을 반환하지 않는 메서드를 명시적으로 나타냅니다.

3. **메서드 이름**: 메서드를 식별하고 호출하는 데 사용됩니다. C#의 명명 규칙에 따라 PascalCase를 사용합니다.

4. **매개변수 목록(Parameter List)**: 메서드가 받을 입력값을 정의합니다. 괄호 안에 쉼표로 구분하여 나열하며, 매개변수가 없으면 빈 괄호 `()`를 사용합니다.

5. **메서드 본문**: 중괄호 `{}` 안에 실제 실행될 코드를 작성합니다. 이는 **코드 블록(Code Block)** 또는 **복합문(Compound Statement)**이라고도 합니다.

**가장 간단한 메서드:**

```csharp
// 반환값이 없고 매개변수도 없는 메서드
void SayHello()
{
    Console.WriteLine("안녕하세요!");
}

// 메서드 호출
SayHello();
SayHello();

// 출력:
// 안녕하세요!
// 안녕하세요!
```

**반환값이 있는 메서드:**

```csharp
// 정수를 반환하는 메서드
int GetNumber()
{
    return 42;
}

// 메서드 호출 및 반환값 사용
int result = GetNumber();
Console.WriteLine($"결과: {result}");

// 출력:
// 결과: 42
```

**실용 예제 - 인사 메서드:**

```csharp
void Greet()
{
    Console.WriteLine("환영합니다!");
    Console.WriteLine("C# 프로그래밍을 시작합니다.");
}

// 프로그램 시작
Console.WriteLine("프로그램 시작");
Greet();
Console.WriteLine("프로그램 종료");

// 출력:
// 프로그램 시작
// 환영합니다!
// C# 프로그래밍을 시작합니다.
// 프로그램 종료
```

**메서드를 사용하는 이유와 소프트웨어 공학 원칙:**

1. **코드 재사용(Code Reuse)**: 같은 코드를 여러 번 작성하지 않고 메서드 호출로 대체합니다. 이는 1968년 더글라스 맥일로이(Douglas McIlroy)가 제안한 **컴포넌트 기반 개발(Component-Based Development)**의 기초가 됩니다.

2. **가독성 향상(Improved Readability)**: 복잡한 코드를 의미 있는 이름의 메서드로 분리하면, 코드가 **자기 문서화(Self-Documenting)**됩니다. 로버트 마틴(Robert C. Martin)의 "Clean Code"에서 강조하는 "코드는 산문처럼 읽혀야 한다"는 원칙을 실천하는 방법입니다.

3. **유지보수 용이성(Maintainability)**: 기능 수정 시 메서드 하나만 수정하면 모든 호출 지점에 자동으로 반영됩니다. 이는 **단일 책임 원칙(Single Responsibility Principle, SRP)**과 직접적으로 연결되며, SOLID 원칙의 첫 번째 원칙입니다.

4. **테스트 편의성(Testability)**: 각 메서드를 독립적으로 테스트할 수 있어, **단위 테스트(Unit Testing)**의 기본 단위가 됩니다. 켄트 벡(Kent Beck)의 테스트 주도 개발(TDD) 방법론에서 핵심적인 역할을 합니다.

5. **추상화(Abstraction)**: 복잡한 구현 세부사항을 숨기고 인터페이스만 노출하여, 사용자가 "어떻게(How)"보다 "무엇을(What)"에 집중할 수 있게 합니다.

6. **디버깅 효율성**: 문제가 발생했을 때 특정 메서드로 범위를 좁혀 원인을 찾을 수 있어, 디버깅 시간이 단축됩니다.

**메서드 명명 규칙과 컨벤션:**

C#에서 메서드 이름은 **PascalCase**(파스칼 표기법)를 사용합니다. 이는 1970년대 닉러스 비르트(Niklaus Wirth)가 개발한 Pascal 언어에서 유래한 명명 방식으로, 각 단어의 첫 글자를 대문자로 작성합니다. Microsoft의 C# 코딩 규칙(Framework Design Guidelines)에서 공식적으로 권장하는 방식입니다.

```csharp
// ✅ 좋은 예 - 동사로 시작하는 명확한 이름
void CalculateTotal()        // 총계 계산
void SendEmail()             // 이메일 전송
void ValidateUserInput()     // 사용자 입력 검증
bool IsValidEmail()          // 불린 반환은 Is, Has, Can 등으로 시작
int GetUserAge()             // Get으로 시작하는 조회 메서드

// ❌ 나쁜 예
void calculate_total()       // snake_case는 Python, Ruby 스타일
void sendemail()             // 단어 구분이 없어 읽기 어려움
void Validate_User_Input()   // 혼합 스타일 (SCREAMING_SNAKE_CASE는 상수용)
void DoStuff()               // 모호한 이름
void Process()               // 무엇을 처리하는지 불명확
```

**메서드 명명의 모범 사례:**

1. **동사로 시작**: 메서드는 동작을 나타내므로 동사(Calculate, Send, Validate, Get, Set)로 시작합니다.
2. **명확성**: 메서드가 하는 일을 정확히 나타내야 합니다. `DoWork()` 대신 `ProcessPayment()`처럼 구체적으로 작성합니다.
3. **일관성**: 프로젝트 전체에서 유사한 기능에 일관된 명명 패턴을 사용합니다.
4. **길이**: 너무 짧지도, 너무 길지도 않게. 일반적으로 2-4단어가 적절합니다.
5. **약어 피하기**: `PrcUsrInpt()` 대신 `ProcessUserInput()`처럼 전체 단어를 사용합니다.

---

## 7.2 매개변수와 반환값

메서드는 외부로부터 데이터를 입력받아 처리할 수 있습니다. 이때 입력받는 데이터를 **매개변수(Parameter)** 또는 **인수(Argument)**라고 하며, 처리 결과를 돌려주는 것을 **반환값(Return Value)**이라고 합니다. 이러한 메커니즘은 프로그래밍 언어 이론에서 **함수 적용(Function Application)**과 **값 전달(Value Passing)**의 개념을 구현한 것입니다.

**매개변수와 인수의 명확한 구분:**

프로그래밍에서 매개변수(Parameter)와 인수(Argument)는 종종 혼용되지만, 엄밀히 구분하면 다음과 같습니다:

- **매개변수(Parameter, Formal Parameter)**: 메서드 정의에서 선언하는 **형식 매개변수**입니다. 메서드의 시그니처를 구성하며, 메서드가 어떤 종류의 데이터를 받을지 명시합니다.
- **인수(Argument, Actual Parameter)**: 메서드를 호출할 때 전달하는 **실제 인수** 또는 **실제 매개변수**입니다. 매개변수에 바인딩되는 구체적인 값입니다.

이 구분은 1960년대 알골(ALGOL) 언어 설계 시 확립된 개념으로, 현대의 모든 프로그래밍 언어에서 유지되고 있습니다.

```csharp
// a, b는 매개변수(Parameter) - 메서드 정의 시
int Multiply(int a, int b)
{
    return a * b;
}

// 5, 3은 인수(Argument) - 메서드 호출 시
int result = Multiply(5, 3);
```

**값 전달 방식의 이론적 배경:**

프로그래밍 언어는 매개변수를 전달하는 다양한 방식을 제공하며, 각각은 서로 다른 의미론(semantics)을 가집니다:

1. **값에 의한 전달(Pass by Value)**: 매개변수의 복사본이 전달됩니다. C, Java의 기본 타입, C#의 값 타입이 이에 해당합니다.

2. **참조에 의한 전달(Pass by Reference)**: 매개변수의 메모리 주소가 전달됩니다. C++의 참조, C#의 `ref`/`out` 키워드가 이를 명시적으로 구현합니다.

3. **공유에 의한 전달(Pass by Sharing, Call by Sharing)**: 참조 타입 객체의 참조가 값으로 전달됩니다. Java의 객체, C#의 참조 타입이 이 방식을 사용하며, "값에 의한 참조 전달"이라고도 합니다.

C#은 이 세 가지 방식을 모두 지원하며, 타입 시스템과 키워드를 통해 명시적으로 제어할 수 있습니다. 이는 C#이 **안전성(Safety)**과 **성능(Performance)**, **표현력(Expressiveness)**의 균형을 추구하는 언어임을 보여줍니다.

**매개변수가 있는 메서드:**

```csharp
// 이름을 매개변수로 받는 메서드
void Greet(string name)
{
    Console.WriteLine($"안녕하세요, {name}님!");
}

// 메서드 호출 - 인수 전달
Greet("홍길동");
Greet("김철수");

// 출력:
// 안녕하세요, 홍길동님!
// 안녕하세요, 김철수님!
```

**여러 개의 매개변수:**

```csharp
// 두 개의 정수를 받아 더하는 메서드
int Add(int a, int b)
{
    return a + b;
}

// 메서드 호출
int result1 = Add(10, 20);
int result2 = Add(5, 15);

Console.WriteLine($"10 + 20 = {result1}");
Console.WriteLine($"5 + 15 = {result2}");

// 출력:
// 10 + 20 = 30
// 5 + 15 = 20
```

**매개변수와 인수의 차이:**

- **매개변수(Parameter)**: 메서드 정의에서 선언하는 변수
- **인수(Argument)**: 메서드를 호출할 때 전달하는 실제 값

```csharp
// a, b는 매개변수
int Multiply(int a, int b)
{
    return a * b;
}

// 5, 3은 인수
int result = Multiply(5, 3);
```

### 7.2.1 값 타입 매개변수

값 타입(Value Type) 매개변수는 데이터의 **완전한 복사본**이 메서드로 전달됩니다. 따라서 메서드 내부에서 매개변수를 변경해도 원본 값은 영향을 받지 않습니다. `int`, `double`, `bool`, `struct`, `enum` 등이 값 타입에 해당합니다. 이는 컴퓨터 과학에서 **값 의미론(Value Semantics)**이라고 부르는 개념을 구현한 것입니다.

**메모리 관점에서의 값 타입 전달:**

값 타입이 메서드에 전달될 때, 실제로 무슨 일이 일어나는지 메모리 관점에서 이해하는 것이 중요합니다. 값 타입 변수는 **스택(Stack) 메모리**에 저장되며, 메서드 호출 시 새로운 스택 프레임(Stack Frame)이 생성됩니다. 이 스택 프레임 내에 원본 데이터의 비트 단위 복사본이 만들어지므로, 원본과 복사본은 완전히 독립적인 메모리 공간을 차지합니다.

이러한 설계는 **데이터 안전성(Data Safety)**과 **캡슐화(Encapsulation)**를 보장하지만, 큰 구조체의 경우 복사 비용이 증가할 수 있습니다. 이것이 바로 C# 컴파일러가 큰 구조체에 대해 경고를 표시하는 이유이며, 이런 경우 `in` 키워드나 `ref readonly`를 사용하여 성능을 최적화할 수 있습니다.

**값 타입의 동작:**

```csharp
void ChangeNumber(int number)
{
    number = 100;  // 복사본을 변경 (스택의 다른 메모리 위치)
    Console.WriteLine($"메서드 내부: {number}");
}

int original = 10;
Console.WriteLine($"호출 전: {original}");
ChangeNumber(original);  // original의 복사본이 전달됨
Console.WriteLine($"호출 후: {original}");

// 출력:
// 호출 전: 10
// 메서드 내부: 100
// 호출 후: 10  (원본은 변경되지 않음)
```

**값 타입 매개변수의 특징과 의미:**

1. **복사본 전달(Copy Semantics)**: 원본 데이터의 비트 단위 복사본이 생성되어 전달됩니다. 이는 C++의 복사 생성자(Copy Constructor) 개념과 유사하지만, C#에서는 암묵적으로 처리됩니다.

2. **안전성(Safety)**: 메서드가 원본 데이터를 변경할 수 없어 **부작용(Side Effects)**으로부터 안전합니다. 이는 함수형 프로그래밍의 **불변성(Immutability)** 개념과 연결됩니다.

3. **메모리와 성능**: 작은 데이터(일반적으로 16바이트 이하)에 적합하며, 큰 구조체는 복사 비용으로 인해 성능에 영향을 줄 수 있습니다. .NET의 성능 가이드라인에서는 구조체가 16바이트를 초과하면 `readonly ref` 전달을 권장합니다.

4. **스택 할당**: 값 타입은 스택에 할당되므로 가비지 컬렉션(Garbage Collection)의 부담이 없고, 메서드가 반환되면 자동으로 메모리가 해제됩니다. 이는 **결정론적 메모리 관리(Deterministic Memory Management)**의 한 형태입니다.

**실용 예제 - 기하학 계산 메서드:**

```csharp
double CalculateCircleArea(double radius)
{
    const double PI = 3.14159;
    return PI * radius * radius;
}

double r = 5.0;
double area = CalculateCircleArea(r);
Console.WriteLine($"반지름 {r}인 원의 넓이: {area:F2}");

// 출력:
// 반지름 5인 원의 넓이: 78.54
```

**구조체 매개변수의 주의사항:**

```csharp
struct Point
{
    public int X;
    public int Y;
}

void MovePoint(Point p)
{
    p.X += 10;  // 복사본을 수정
    p.Y += 10;
}

Point original = new Point { X = 5, Y = 5 };
Console.WriteLine($"이동 전: ({original.X}, {original.Y})");
MovePoint(original);  // 구조체 전체가 복사됨
Console.WriteLine($"이동 후: ({original.X}, {original.Y})");  // 변경 안 됨

// 출력:
// 이동 전: (5, 5)
// 이동 후: (5, 5)  // 원본은 그대로
```

### 7.2.2 참조 타입 매개변수

참조 타입(Reference Type) 매개변수는 데이터가 저장된 메모리 위치의 **참조(Reference)**가 전달됩니다. 더 정확히 말하면, 참조 자체가 값으로 복사되어 전달되는 것입니다(이를 "참조의 값 복사" 또는 "공유에 의한 전달"이라고 합니다). 따라서 메서드 내부에서 객체의 **내용**을 변경하면 원본에도 영향을 미칩니다. `string`, `array`, `class`, `interface`, `delegate` 등이 참조 타입에 해당합니다.

**참조 의미론(Reference Semantics)의 이해:**

참조 타입 변수는 실제 데이터가 아니라 **힙(Heap) 메모리**에 있는 객체를 가리키는 주소(또는 핸들)를 저장합니다. 메서드에 참조 타입을 전달하면, 이 주소 값이 복사되어 전달됩니다. 결과적으로 원본 변수와 매개변수 모두 같은 객체를 가리키게 되므로, 한쪽에서 객체를 수정하면 다른 쪽에서도 변경사항이 보입니다.

이는 C 언어의 포인터와 유사하지만, C#은 **관리되는 참조(Managed Reference)**를 사용하여 포인터 산술 연산이나 잘못된 메모리 접근을 원천적으로 차단합니다. 이를 통해 메모리 안정성을 확보하면서도 효율적인 데이터 공유가 가능합니다.

**중요한 개념적 차이:**

- **값 타입**: 변수 자체가 데이터를 직접 포함합니다 (스택에 인라인 저장).
- **참조 타입**: 변수는 데이터를 가리키는 참조를 포함합니다 (힙의 객체를 가리킴).

**참조 타입의 동작 - 배열:**

```csharp
void ModifyArray(int[] numbers)
{
    numbers[0] = 999;  // 원본 배열의 내용을 변경
    // numbers 매개변수와 myArray 변수 모두 같은 배열 객체를 가리킴
}

int[] myArray = { 1, 2, 3 };
Console.WriteLine($"변경 전: {myArray[0]}");
ModifyArray(myArray);  // 배열의 참조(주소)가 복사되어 전달됨
Console.WriteLine($"변경 후: {myArray[0]}");

// 출력:
// 변경 전: 1
// 변경 후: 999  (원본이 변경됨)
```

**메모리 다이어그램으로 이해하기:**

```
[호출 전]
myArray(스택) ---> [1, 2, 3] (힙)

[메서드 호출 시]
myArray(스택) ---> [1, 2, 3] (힙)
                    ^
numbers(스택) ------+ (같은 객체를 가리킴)

[numbers[0] = 999 실행 후]
myArray(스택) ---> [999, 2, 3] (힙)
                    ^
numbers(스택) ------+
```

**참조 타입의 특수성 - 문자열의 불변성:**

문자열(`string`)은 참조 타입이지만 **불변(Immutable)** 객체입니다. 이는 .NET의 독특한 설계 결정으로, 1995년 Java가 도입한 개념을 C#도 채택했습니다. 따라서 메서드 내에서 문자열을 "변경"하면 실제로는 새로운 문자열 객체가 생성되며, 매개변수는 이 새 객체를 가리키게 되지만 원본 변수는 여전히 기존 객체를 가리킵니다.

**문자열 불변성의 이유:**

1. **스레드 안정성(Thread Safety)**: 여러 스레드가 동시에 같은 문자열을 읽어도 안전합니다.
2. **보안(Security)**: 문자열을 키로 사용할 때 예측 가능성이 보장됩니다.
3. **최적화(Optimization)**: 문자열 인터닝(String Interning)을 통해 메모리를 절약할 수 있습니다.
4. **해시 코드 안정성**: Dictionary 등의 키로 사용할 때 해시 값이 변하지 않습니다.

```csharp
void ChangeString(string text)
{
    text = "변경된 문자열";  // 새로운 문자열 객체 생성
    // text는 이제 새 객체를 가리키지만, 호출자의 original은 여전히 기존 객체를 가리킴
    Console.WriteLine($"메서드 내부: {text}");
}

string original = "원본 문자열";
Console.WriteLine($"호출 전: {original}");
ChangeString(original);  // original의 참조가 복사되어 전달됨
Console.WriteLine($"호출 후: {original}");

// 출력:
// 호출 전: 원본 문자열
// 메서드 내부: 변경된 문자열
// 호출 후: 원본 문자열  (원본은 변경되지 않음!)
```

**참조 타입 매개변수의 특징:**

1. **효율적인 전달**: 큰 객체도 참조(일반적으로 4~8바이트)만 복사되므로 효율적입니다. 1MB 크기의 배열을 전달해도 8바이트의 참조만 복사됩니다.

2. **공유 가능성**: 같은 객체를 여러 메서드가 공유할 수 있어, 대용량 데이터 구조를 다룰 때 유리합니다.

3. **원본 수정 가능**: 객체의 내용(멤버, 요소 등)을 변경하면 원본에 반영됩니다. 이는 **참조 투명성(Referential Transparency)**을 깨뜨리므로 주의가 필요합니다.

4. **null 가능성**: 참조 타입은 `null` 값을 가질 수 있으므로, 메서드 내에서 null 체크가 필요할 수 있습니다(C# 8.0 이후 Nullable Reference Types로 개선).

5. **가비지 컬렉션**: 힙에 할당되므로 자동 가비지 컬렉션의 대상이 됩니다.

**실용 예제 - 배열 처리:**

```csharp
void FillArray(int[] array, int value)
{
    for (int i = 0; i < array.Length; i++)
    {
        array[i] = value;  // 원본 배열의 요소를 변경
    }
}

int[] numbers = new int[5];
FillArray(numbers, 7);

Console.Write("배열 내용: ");
foreach (int num in numbers)
{
    Console.Write($"{num} ");
}

// 출력:
// 배열 내용: 7 7 7 7 7
```

**참조 재할당과 내용 변경의 차이:**

```csharp
void ModifyReference(int[] arr)
{
    arr[0] = 100;           // ✅ 내용 변경 - 원본에 영향
    arr = new int[] { 1 };  // ❌ 참조 재할당 - 원본에 영향 없음
                            // arr은 이제 새 배열을 가리키지만, 호출자의 변수는 여전히 원래 배열을 가리킴
}

int[] original = { 1, 2, 3 };
ModifyReference(original);
Console.WriteLine(original[0]);  // 100 출력 (첫 번째 변경은 유효)
Console.WriteLine(original.Length);  // 3 출력 (두 번째 변경은 무효)
```

### 7.2.3 ref와 out 키워드

C#은 매개변수 전달 방식을 명시적으로 제어할 수 있는 `ref`와 `out` 키워드를 제공합니다. 이들은 값 타입을 **진정한 참조(true reference)**로 전달하거나, 메서드에서 여러 값을 반환해야 할 때 유용합니다. 이러한 키워드는 C++의 참조 매개변수(`&`)와 유사하지만, C#은 호출 지점에서도 명시적으로 키워드를 표기하도록 강제하여 코드의 의도를 명확히 합니다.

**역사적 배경과 설계 철학:**

`ref`와 `out` 키워드는 C 언어의 포인터와 C++의 참조 매개변수에서 영감을 받았지만, C#의 설계 철학인 **명시성(Explicitness)**과 **타입 안전성(Type Safety)**을 반영합니다. C++에서는 `func(x)`만 봐서는 x가 값으로 전달되는지 참조로 전달되는지 알 수 없지만, C#에서는 `func(ref x)` 또는 `func(out x)`처럼 호출 지점에서 명확히 표시됩니다. 이는 에릭 립퍼트(Eric Lippert, C# 언어 설계자)가 강조한 "놀라움 최소화 원칙(Principle of Least Astonishment)"을 실천한 사례입니다.

**ref 키워드 - 양방향 참조 전달:**

`ref` 키워드는 변수를 **참조로(by reference)** 전달합니다. 메모리 관점에서 보면, 매개변수는 원본 변수와 완전히 동일한 메모리 위치를 가리키는 **별칭(alias)**이 됩니다. 따라서 메서드 내에서 변경한 내용이 즉시 원본에 반영됩니다. `ref`를 사용하려면 변수가 **반드시 초기화**되어 있어야 합니다. 이는 초기화되지 않은 변수를 읽는 것을 방지하여 안전성을 보장하기 위함입니다.

**ref의 메모리 모델:**

```
[일반적인 값 타입 전달]
original(스택): 10
number(스택): 10 (복사본)

[ref 사용 시]
original(스택): 10 <--- number는 original의 별칭 (같은 메모리 위치)
```

```csharp
void Increment(ref int number)
{
    number++;  // 원본 메모리 위치의 값을 직접 수정
}

int count = 10;
Console.WriteLine($"호출 전: {count}");
Increment(ref count);  // ref 키워드를 호출 시에도 명시 (명시성 원칙)
Console.WriteLine($"호출 후: {count}");

// 출력:
// 호출 전: 10
// 호출 후: 11  (원본이 변경됨)
```

**ref의 특징과 계약(Contract):**

1. **초기화 필수**: 매개변수는 메서드 호출 전에 **반드시 초기화**되어 있어야 합니다. 컴파일러가 검증합니다.
2. **양방향 통신**: 메서드 내에서 값을 읽고 쓰기 모두 가능합니다 (in/out 모두).
3. **명시적 표기**: 메서드 정의와 호출 시 모두 `ref` 키워드가 필요합니다 (코드 가독성).
4. **성능 최적화**: 큰 구조체를 복사하지 않고 전달할 수 있어 성능상 이점이 있습니다.

**out 키워드 - 출력 전용 참조:**

`out` 키워드도 참조로 전달하지만, **출력 전용(output-only)** 매개변수입니다. 메서드 내에서 **반드시 값을 할당**해야 하며, 초기값을 읽을 수 없습니다. 이는 메서드가 여러 값을 반환해야 할 때 유용한 패턴으로, 함수형 프로그래밍의 **튜플(Tuple)** 반환과 유사한 역할을 합니다.

**out의 설계 의도:**

`out` 키워드는 C#이 메서드의 **계약(Contract)**을 컴파일 타임에 강제하는 메커니즘입니다. 메서드가 `out` 매개변수를 선언하면, 반드시 그 값을 할당해야 한다는 계약이 생성되고, 컴파일러가 이를 검증합니다. 이는 런타임 오류를 방지하는 **정적 분석(Static Analysis)**의 한 형태입니다.

```csharp
void Divide(int dividend, int divisor, out int quotient, out int remainder)
{
    quotient = dividend / divisor;      // 필수: 반드시 할당해야 함
    remainder = dividend % divisor;     // 필수: 반드시 할당해야 함
}

int a = 17;
int b = 5;
Divide(a, b, out int q, out int r);  // C# 7.0: out 변수 인라인 선언

Console.WriteLine($"{a} ÷ {b} = {q} 나머지 {r}");

// 출력:
// 17 ÷ 5 = 3 나머지 2
```

**out의 특징과 계약:**

1. **초기화 불필요**: 매개변수는 메서드 호출 전에 초기화되지 않아도 됩니다. 실제로 기존 값은 무시됩니다.
2. **할당 강제**: 메서드 내에서 **반드시 값을 할당**해야 합니다. 컴파일러가 모든 경로에서 할당을 검증합니다.
3. **읽기 금지**: 할당 전에는 값을 읽을 수 없습니다 (Definite Assignment Analysis).
4. **인라인 선언**: C# 7.0부터 호출 시 변수 선언이 가능합니다 (`out int result`).
5. **명시적 표기**: 메서드 정의와 호출 시 모두 `out` 키워드가 필요합니다.

**ref와 out의 비교 테이블:**

| 특성 | ref | out |
|------|-----|-----|
| **초기화 요구** | 필수 (호출 전) | 불필요 |
| **할당 요구** | 선택적 | 필수 (모든 경로) |
| **읽기** | 가능 | 할당 전 불가 |
| **쓰기** | 가능 | 필수 |
| **용도** | 양방향 통신 | 출력 전용 |
| **의미** | 기존 값 수정 | 새 값 반환 |
| **도입 버전** | C# 1.0 | C# 1.0 |
| **인라인 선언** | C# 7.0 | C# 7.0 |

```csharp
// ref - 초기화 필수, 읽기/쓰기 가능
void UseRef(ref int value)
{
    Console.WriteLine($"받은 값: {value}");  // ✅ 읽기 가능
    value = value * 2;  // ✅ 쓰기 가능
}

int x = 5;
UseRef(ref x);

// out - 초기화 불필요, 반드시 할당해야 함
void UseOut(out int value)
{
    // Console.WriteLine(value);  // ❌ 할당 전에는 읽을 수 없음 (CS0269 에러)
    value = 10;  // ✅ 반드시 할당
}

UseOut(out int y);  // 초기화하지 않아도 됨
```

**실용 예제 - TryParse 패턴:**

.NET의 많은 파싱 메서드들이 `out` 패턴을 사용합니다. 이는 성공/실패 정보와 결과 값을 동시에 반환하는 우아한 패턴입니다.

```csharp
void TryParseNumber(string input, out bool success, out int result)
{
    success = int.TryParse(input, out result);
    // int.TryParse가 이미 out 패턴을 사용하여 결과를 반환
}

string userInput = "123";
TryParseNumber(userInput, out bool isValid, out int number);

if (isValid)
{
    Console.WriteLine($"변환 성공: {number}");
}
else
{
    Console.WriteLine("변환 실패");
}

// 출력:
// 변환 성공: 123
```

**C# 7.0 이후의 개선사항:**

C# 7.0은 `out` 변수 선언을 더욱 편리하게 만들었습니다:

```csharp
// C# 6.0 이전 (장황함)
int result;
if (int.TryParse("123", out result))
{
    Console.WriteLine(result);
}

// C# 7.0 이후 (간결함)
if (int.TryParse("123", out int result))
{
    Console.WriteLine(result);
}

// C# 7.0: 무시 패턴(Discard Pattern)
if (int.TryParse("123", out _))  // 값은 필요 없고 성공 여부만 필요할 때
{
    Console.WriteLine("파싱 성공");
}
```

**ref와 out의 IL 코드 차이:**

흥미롭게도, 컴파일된 IL(Intermediate Language) 코드에서 `ref`와 `out`은 거의 동일합니다. 둘 다 메타데이터 속성으로만 구분되며, CLR(Common Language Runtime) 수준에서는 같은 방식으로 처리됩니다. 차이는 컴파일 타임의 의미론과 검증 규칙에만 있습니다. 이는 C# 컴파일러가 타입 안전성과 정확성을 강제하기 위해 의미론적 구분을 두었음을 보여줍니다.

```csharp
// ref - 초기화 필수, 읽기/쓰기 가능
void UseRef(ref int value)
{
    Console.WriteLine($"받은 값: {value}");  // 읽기 가능
    value = value * 2;  // 쓰기 가능
}

int x = 5;
UseRef(ref x);

// out - 초기화 불필요, 반드시 할당해야 함
void UseOut(out int value)
{
    // Console.WriteLine(value);  // ❌ 할당 전에는 읽을 수 없음
    value = 10;  // 반드시 할당
}

UseOut(out int y);  // 초기화하지 않아도 됨
```

**실용 예제 - 문자열 파싱:**

```csharp
void TryParseNumber(string input, out bool success, out int result)
{
    success = int.TryParse(input, out result);
}

string userInput = "123";
TryParseNumber(userInput, out bool isValid, out int number);

if (isValid)
{
    Console.WriteLine($"변환 성공: {number}");
}
else
{
    Console.WriteLine("변환 실패");
}

// 출력:
// 변환 성공: 123
```

---

## 7.3 메서드 오버로딩

메서드 오버로딩(Method Overloading, 또는 Function Overloading)은 같은 이름의 메서드를 매개변수의 **시그니처(Signature)**를 다르게 하여 여러 버전으로 정의하는 것입니다. 이는 **다형성(Polymorphism)**의 한 형태인 **컴파일 타임 다형성(Compile-time Polymorphism)** 또는 **정적 다형성(Static Polymorphism)**을 구현하는 메커니즘입니다. 이를 통해 비슷한 기능을 하는 메서드들을 하나의 일관된 이름으로 제공할 수 있어, API의 사용성과 학습성이 크게 향상됩니다.

**오버로딩의 역사적 배경:**

메서드 오버로딩 개념은 1960년대 후반 알골 68(ALGOL 68)에서 처음 도입되었으며, 1980년대 C++가 이를 대중화했습니다. 비야네 스트롭스트룹(Bjarne Stroustrup)은 C++에서 연산자 오버로딩과 함께 함수 오버로딩을 도입하여, 수학적 표기법에 가까운 직관적인 코드 작성을 가능하게 했습니다. C#은 이 전통을 계승하면서도, Java가 겪었던 일부 모호성 문제를 개선했습니다.

**오버로딩이 필요한 이유:**

오버로딩이 없다면 같은 개념적 작업에 대해 `AddIntegers()`, `AddDoubles()`, `AddStrings()`처럼 서로 다른 이름을 사용해야 합니다. 이는 1970년대 C 언어 표준 라이브러리의 문제점이었으며 (`strcpy`, `strncpy`, `memcpy` 등의 혼란), 현대 언어들이 오버로딩을 적극 채택하게 된 동기가 되었습니다.

**기본 개념:**

```csharp
// 매개변수 개수가 다른 오버로딩
void Print(string message)
{
    Console.WriteLine(message);
}

void Print(string message, int count)
{
    for (int i = 0; i < count; i++)
    {
        Console.WriteLine(message);
    }
}

// 사용 - 컴파일러가 자동으로 적절한 버전 선택
Print("안녕하세요");           // 첫 번째 메서드 호출
Print("반갑습니다", 3);        // 두 번째 메서드 호출

// 출력:
// 안녕하세요
// 반갑습니다
// 반갑습니다
// 반갑습니다
```

**매개변수 타입이 다른 오버로딩:**

```csharp
int Add(int a, int b)
{
    return a + b;
}

double Add(double a, double b)
{
    return a + b;
}

string Add(string a, string b)
{
    return a + b;
}

// 사용 - 인수 타입에 따라 자동으로 매칭
Console.WriteLine(Add(5, 3));                  // 8 (int 버전)
Console.WriteLine(Add(2.5, 3.7));              // 6.2 (double 버전)
Console.WriteLine(Add("Hello, ", "World!"));   // Hello, World! (string 버전)
```

**오버로딩 해결 규칙(Overload Resolution Rules):**

C# 컴파일러는 메서드 호출 시 다음 기준에 따라 가장 적합한 오버로드를 선택합니다. 이 과정을 **오버로드 해결(Overload Resolution)**이라고 하며, C# 언어 명세서에 정확히 정의되어 있습니다.

1. **메서드 시그니처 구성 요소**: 
   - 메서드 이름 (대소문자 구분)
   - 매개변수의 **개수**
   - 매개변수의 **타입**
   - 매개변수의 **순서**
   - `ref`, `out`, `in` 같은 매개변수 한정자

2. **제외 항목**:
   - **반환 타입**: 반환 타입만 다른 것은 오버로딩으로 간주되지 않음
   - **매개변수 이름**: 이름만 다른 것은 시그니처에 영향을 주지 않음
   - **접근 제한자**: `public`, `private` 등은 오버로딩과 무관

```csharp
// ✅ 올바른 오버로딩 - 매개변수 타입이 다름
int Calculate(int a, int b)
{
    return a + b;
}

double Calculate(double a, double b)
{
    return a + b;
}

// ✅ 올바른 오버로딩 - 매개변수 개수가 다름
void Display(string text)
{
    Console.WriteLine(text);
}

void Display(string text, ConsoleColor color)
{
    Console.ForegroundColor = color;
    Console.WriteLine(text);
    Console.ResetColor();
}

// ❌ 잘못된 오버로딩 - 반환 타입만 다름 (컴파일 오류 CS0111)
// int Process(int x)
// {
//     return x * 2;
// }
// 
// double Process(int x)  // ❌ 에러: 반환 타입만 다름
// {
//     return x * 2.0;
// }

// ❌ 잘못된 오버로딩 - 매개변수 이름만 다름 (컴파일 오류 CS0111)
// void Method(int a)
// {
// }
// 
// void Method(int b)  // ❌ 에러: 매개변수 이름만 다름, 타입은 같음
// {
// }
```

**오버로드 해결 알고리즘:**

컴파일러는 다음 단계로 최적의 오버로드를 선택합니다:

1. **적용 가능한 후보 선택**: 주어진 인수로 호출 가능한 모든 오버로드를 찾습니다.
2. **가장 구체적인 버전 선택**: 암묵적 형변환이 적을수록 우선순위가 높습니다.
3. **모호성 검사**: 두 개 이상의 오버로드가 동등하게 적합하면 컴파일 오류가 발생합니다.

```csharp
void Process(int x) { Console.WriteLine("int 버전"); }
void Process(double x) { Console.WriteLine("double 버전"); }
void Process(object x) { Console.WriteLine("object 버전"); }

Process(5);      // "int 버전" - 정확히 일치
Process(5.0);    // "double 버전" - 정확히 일치
Process("text"); // "object 버전" - string은 object로 업캐스팅
Process(5L);     // "double 버전" - long에서 double로 암묵적 변환 (int는 좁히기 변환이므로 제외)
```

**실용 예제 - 다양한 형태의 로그 출력:**

```csharp
void Log(string message)
{
    Console.WriteLine($"[INFO] {message}");
}

void Log(string message, string level)
{
    Console.WriteLine($"[{level}] {message}");
}

void Log(string message, string level, DateTime timestamp)
{
    Console.WriteLine($"[{timestamp:yyyy-MM-dd HH:mm:ss}] [{level}] {message}");
}

// 사용 - 상황에 맞는 버전을 자연스럽게 선택
Log("애플리케이션 시작");
Log("데이터베이스 연결 성공", "SUCCESS");
Log("파일 처리 완료", "INFO", DateTime.Now);

// 출력:
// [INFO] 애플리케이션 시작
// [SUCCESS] 데이터베이스 연결 성공
// [2024-01-15 14:30:25] [INFO] 파일 처리 완료
```

**C# 표준 라이브러리의 오버로딩 예시:**

C#의 거의 모든 표준 라이브러리가 오버로딩을 광범위하게 사용합니다. 이는 사용자에게 일관되고 직관적인 API를 제공하기 위한 설계 철학입니다.

```csharp
// Console.WriteLine의 다양한 오버로드 (16개 이상!)
Console.WriteLine();                    // 빈 줄 출력
Console.WriteLine("문자열");            // 문자열 출력
Console.WriteLine(123);                 // 정수 출력
Console.WriteLine(3.14);                // 실수 출력
Console.WriteLine(true);                // 불린 출력
Console.WriteLine('{0}', "값");         // 포맷 문자열

// String.Substring의 오버로드
string text = "Hello, World!";
string part1 = text.Substring(7);       // "World!" (시작 인덱스만)
string part2 = text.Substring(0, 5);    // "Hello" (시작 인덱스와 길이)

// Math.Max의 오버로드 (여러 타입 지원)
Math.Max(10, 20);        // int 버전
Math.Max(10.5, 20.3);    // double 버전
Math.Max(10L, 20L);      // long 버전
```

**오버로딩 vs 선택적 매개변수:**

C# 4.0에서 선택적 매개변수가 도입되면서, 일부 오버로딩을 선택적 매개변수로 대체할 수 있게 되었습니다. 각각의 장단점을 이해하고 상황에 맞게 선택해야 합니다.

```csharp
// 오버로딩 방식 (C# 3.0 이전부터 사용)
void Log(string message)
{
    Log(message, "INFO");
}

void Log(string message, string level)
{
    Console.WriteLine($"[{level}] {message}");
}

// 선택적 매개변수 방식 (C# 4.0 이후 가능)
void Log(string message, string level = "INFO")
{
    Console.WriteLine($"[{level}] {message}");
}
```

**오버로딩 사용 시 고려사항:**

1. **일관성**: 같은 이름의 메서드들은 개념적으로 유사한 작업을 수행해야 합니다.
2. **명확성**: 어떤 오버로드가 호출될지 애매하지 않도록 해야 합니다.
3. **유지보수**: 과도한 오버로딩은 오히려 복잡도를 높일 수 있습니다.
4. **바이너리 호환성**: 라이브러리를 개발할 때, 기존 오버로드를 변경하면 호환성 문제가 발생할 수 있습니다.
// ✅ 올바른 오버로딩 - 매개변수 타입이 다름
int Calculate(int a, int b)
{
    return a + b;
}

double Calculate(double a, double b)
{
    return a + b;
}

// ❌ 잘못된 오버로딩 - 반환 타입만 다름 (컴파일 오류)
// int Process(int x)
// {
//     return x * 2;
// }
// 
// double Process(int x)  // ❌ 에러: 반환 타입만 다름
// {
//     return x * 2.0;
// }

// ❌ 잘못된 오버로딩 - 매개변수 이름만 다름 (컴파일 오류)
// void Method(int a)
// {
// }
// 
// void Method(int b)  // ❌ 에러: 매개변수 이름만 다름
// {
// }
```

**실용 예제 - 다양한 형태의 로그 출력:**

```csharp
void Log(string message)
{
    Console.WriteLine($"[INFO] {message}");
}

void Log(string message, string level)
{
    Console.WriteLine($"[{level}] {message}");
}

void Log(string message, string level, DateTime timestamp)
{
    Console.WriteLine($"[{timestamp:yyyy-MM-dd HH:mm:ss}] [{level}] {message}");
}

// 사용
Log("애플리케이션 시작");
Log("데이터베이스 연결 성공", "SUCCESS");
Log("파일 처리 완료", "INFO", DateTime.Now);

// 출력:
// [INFO] 애플리케이션 시작
// [SUCCESS] 데이터베이스 연결 성공
// [2024-01-15 14:30:25] [INFO] 파일 처리 완료
```

**C# 표준 라이브러리의 오버로딩 예:**

C#의 많은 내장 메서드들이 오버로딩을 사용합니다:

```csharp
// Console.WriteLine의 다양한 오버로드
Console.WriteLine();                    // 빈 줄 출력
Console.WriteLine("문자열");            // 문자열 출력
Console.WriteLine(123);                 // 정수 출력
Console.WriteLine(3.14);                // 실수 출력
Console.WriteLine(true);                // 불린 출력

// String.Substring의 오버로드
string text = "Hello, World!";
string part1 = text.Substring(7);       // "World!" (시작 인덱스만)
string part2 = text.Substring(0, 5);    // "Hello" (시작 인덱스와 길이)
```

---

## 7.4 선택적 매개변수와 명명된 인수

C# 4.0에서 도입된 선택적 매개변수(Optional Parameters)와 명명된 인수(Named Arguments)는 메서드 호출을 더욱 유연하고 가독성 있게 만들어주는 현대적인 기능입니다. 이들은 원래 **COM Interop**(Component Object Model 상호운용성)을 개선하기 위해 도입되었지만, 일반 C# 프로그래밍에서도 매우 유용한 기능으로 자리잡았습니다.

**역사적 배경 - COM Interop의 문제:**

C# 4.0이 개발되던 2009년, Microsoft Office와 같은 COM 기반 애플리케이션을 C#에서 사용하는 것은 매우 번거로웠습니다. COM API는 많은 선택적 매개변수를 가지고 있었지만, C# 3.0까지는 이를 지원하지 않아 모든 매개변수를 명시적으로 전달해야 했습니다. 심지어 사용하지 않는 매개변수도 `Type.Missing` 같은 특수 값으로 채워야 했습니다:

```csharp
// C# 3.0 이전 - 고통스러운 COM Interop
workbook.SaveAs(filename, Type.Missing, Type.Missing, Type.Missing,
                Type.Missing, Type.Missing, XlSaveAsAccessMode.xlNoChange,
                Type.Missing, Type.Missing, Type.Missing, Type.Missing,
                Type.Missing);

// C# 4.0 이후 - 선택적 매개변수와 명명된 인수 사용
workbook.SaveAs(filename, AccessMode: XlSaveAsAccessMode.xlNoChange);
```

이러한 문제를 해결하기 위해 C# 팀은 VB.NET이 이미 제공하던 선택적 매개변수와 명명된 인수 기능을 C#에도 도입했습니다. 이는 Anders Hejlsberg가 주도한 "dynamic programming" 및 "improved interoperability"라는 C# 4.0의 두 가지 주요 테마 중 하나였습니다.

### 선택적 매개변수 (Optional Parameters)

선택적 매개변수는 기본값(Default Value)을 가지는 매개변수로, 호출 시 값을 생략할 수 있습니다. 기본값은 **컴파일 타임 상수**여야 하며, 컴파일러가 호출 지점에서 기본값을 자동으로 삽입합니다.

**컴파일 타임 바인딩의 이해:**

선택적 매개변수의 기본값은 메서드 호출 지점에서 컴파일 타임에 삽입됩니다. 이는 중요한 의미를 가집니다:

```csharp
// 라이브러리 DLL의 코드
void Log(string message, string level = "INFO")
{
    Console.WriteLine($"[{level}] {message}");
}

// 호출 코드 (다른 어셈블리)
Log("테스트");  // 컴파일러가 Log("테스트", "INFO")로 변환
```

만약 라이브러리에서 기본값을 `"INFO"`에서 `"DEBUG"`로 변경하고 재컴파일하면, **호출하는 코드도 재컴파일해야** 변경사항이 반영됩니다. 이는 오버로딩과의 중요한 차이점입니다.

```csharp
void Greet(string name, string greeting = "안녕하세요")
{
    Console.WriteLine($"{greeting}, {name}님!");
}

// 다양한 호출 방법
Greet("홍길동");                    // 컴파일러가 Greet("홍길동", "안녕하세요")로 변환
Greet("김철수", "반갑습니다");      // 사용자 정의 인사말

// 출력:
// 안녕하세요, 홍길동님!
// 반갑습니다, 김철수님!
```

**여러 개의 선택적 매개변수:**

```csharp
void CreateUser(string name, int age = 0, string city = "서울", bool isActive = true)
{
    Console.WriteLine($"이름: {name}");
    Console.WriteLine($"나이: {age}");
    Console.WriteLine($"도시: {city}");
    Console.WriteLine($"활성: {isActive}");
}

// 다양한 호출 - 왼쪽에서 오른쪽으로 순차적으로 생략 가능
CreateUser("이영희");                    // 나머지는 모두 기본값
CreateUser("박민수", 30);                // city와 isActive는 기본값
CreateUser("최지훈", 25, "부산");        // isActive만 기본값

// 출력:
// 이름: 이영희
// 나이: 0
// 도시: 서울
// 활성: True
// 
// 이름: 박민수
// 나이: 30
// 도시: 서울
// 활성: True
// 
// 이름: 최지훈
// 나이: 25
// 도시: 부산
// 활성: True
```

**선택적 매개변수의 규칙과 제약:**

1. **위치 규칙**: 선택적 매개변수는 **반드시** 필수 매개변수 뒤에 위치해야 합니다. 이는 호출 시 모호성을 방지하기 위함입니다.

2. **상수 제약**: 기본값은 컴파일 타임 상수여야 합니다. 리터럴, `const` 필드, `default(T)`, `null` 등만 가능합니다.

3. **ref/out 제외**: `ref`, `out`, `in` 매개변수는 선택적으로 만들 수 없습니다. 이들은 호출자와 메서드 간의 명시적 계약이므로 생략할 수 없습니다.

```csharp
// ✅ 올바른 순서
void Method1(int required, string optional = "default")
{
}

// ❌ 잘못된 순서 (CS1737 에러)
// void Method2(string optional = "default", int required)
// {
// }

// ✅ 가능한 기본값
void ValidDefaults(
    int a = 0,                    // 리터럴
    string b = "hello",           // 문자열 리터럴
    bool c = true,                // 불린 리터럴
    object d = null,              // null
    ConsoleColor e = ConsoleColor.Red,  // 열거형
    DateTime f = default(DateTime))     // default 표현식
{
}

// ❌ 불가능한 기본값
// void InvalidDefaults(
//     DateTime now = DateTime.Now)  // ❌ 런타임 값
// {
// }
```

### 명명된 인수 (Named Arguments)

명명된 인수는 매개변수 이름을 명시하여 값을 전달하는 방식으로, **순서에 관계없이** 인수를 전달할 수 있습니다. 이는 특히 많은 매개변수를 가진 메서드를 호출할 때 코드의 **자기 문서화(Self-Documentation)** 특성을 크게 향상시킵니다.

**명명된 인수의 철학:**

명명된 인수는 Python의 키워드 인수(Keyword Arguments)에서 영감을 받은 기능으로, "코드는 쓰는 것보다 읽히는 횟수가 훨씬 많다"는 원칙을 실천합니다. 메서드 호출 지점에서 각 인수가 무엇을 의미하는지 명확히 알 수 있어, 특히 불린 플래그나 의미가 불명확한 숫자 리터럴을 전달할 때 유용합니다.

```csharp
// 명명되지 않은 호출 - 의미가 불명확
Connect("localhost", 8080, 30, true, false);  // 각 인수가 무엇인지 파악하기 어려움

// 명명된 호출 - 명확하고 자기 문서화됨
Connect(
    host: "localhost",
    port: 8080,
    timeout: 30,
    useSSL: true,
    keepAlive: false
);
```

**기본 사용법:**

```csharp
void SetupConnection(string server, int port, string username, string password)
{
    Console.WriteLine($"서버: {server}:{port}");
    Console.WriteLine($"사용자: {username}");
}

// 명명된 인수 사용 - 순서대로
SetupConnection(
    server: "localhost",
    port: 8080,
    username: "admin",
    password: "1234"
);

// 순서를 바꿔도 됨 - 명명되었으므로 안전함
SetupConnection(
    username: "admin",
    password: "1234",
    server: "localhost",
    port: 8080
);

// 일부만 명명 (C# 7.2부터 개선됨)
SetupConnection("localhost", 8080, 
    username: "admin",    // 나머지는 명명
    password: "1234");
```

**선택적 매개변수와 명명된 인수의 시너지:**

두 기능을 결합하면 매우 강력한 표현력을 얻을 수 있습니다. 중간의 선택적 매개변수는 건너뛰고, 마지막 매개변수만 지정하는 것이 가능합니다.

```csharp
void SendEmail(
    string to, 
    string subject, 
    string body = "", 
    bool isHtml = false, 
    int priority = 1,
    bool includeAttachment = false)
{
    Console.WriteLine($"받는사람: {to}");
    Console.WriteLine($"제목: {subject}");
    Console.WriteLine($"HTML: {isHtml}");
    Console.WriteLine($"우선순위: {priority}");
}

// 중간 매개변수를 건너뛰고 원하는 것만 지정
SendEmail(
    to: "user@example.com",
    subject: "안내",
    isHtml: true,         // body는 기본값 사용
    priority: 5           // includeAttachment는 기본값
);

// 출력:
// 받는사람: user@example.com
// 제목: 안내
// HTML: True
// 우선순위: 5
```
void SendEmail(string to, string subject, string body = "", bool isHtml = false, int priority = 1)
{
    Console.WriteLine($"받는사람: {to}");
    Console.WriteLine($"제목: {subject}");
    Console.WriteLine($"내용: {body}");
    Console.WriteLine($"HTML: {isHtml}");
    Console.WriteLine($"우선순위: {priority}");
}

// 중간 매개변수만 지정하기
SendEmail(
    to: "user@example.com",
    subject: "안내",
    isHtml: true,      // body는 기본값, isHtml만 지정
    priority: 5
);

// 출력:
// 받는사람: user@example.com
// 제목: 안내
// 내용: 
// HTML: True
// 우선순위: 5
```

**실용 예제 - 로그 메서드:**

```csharp
void WriteLog(
    string message,
    string level = "INFO",
    bool includeTimestamp = true,
    bool includeLineBreak = true)
{
    if (includeTimestamp)
    {
        Console.Write($"[{DateTime.Now:HH:mm:ss}] ");
    }
    
    Console.Write($"[{level}] {message}");
    
    if (includeLineBreak)
    {
        Console.WriteLine();
    }
}

// 다양한 사용
WriteLog("애플리케이션 시작");
WriteLog("경고 메시지", level: "WARNING");
WriteLog("에러 발생", level: "ERROR", includeTimestamp: false);

// 출력:
// [14:30:15] [INFO] 애플리케이션 시작
// [14:30:15] [WARNING] 경고 메시지
// [ERROR] 에러 발생
```

**명명된 인수의 장점:**

1. **가독성**: 매개변수의 의미가 명확해짐
2. **유연성**: 순서에 관계없이 인수 전달 가능
3. **선택성**: 원하는 매개변수만 지정 가능

---

## 7.5 로컬 함수 (Local Functions)

로컬 함수(Local Functions)는 C# 7.0에서 도입된 현대적인 기능으로, 메서드 내부에 정의되는 **지역 범위(Local Scope)** 함수입니다. 이는 특정 메서드에서만 사용되는 헬퍼 함수를 정의할 때 유용하며, 코드의 **응집도(Cohesion)**를 높이고 외부에 노출되지 않아야 하는 로직을 **캡슐화(Encapsulation)**할 수 있습니다.

**역사적 배경과 설계 동기:**

로컬 함수는 함수형 프로그래밍 언어(Scala, F#, Haskell 등)의 **중첩 함수(Nested Functions)** 개념을 C#에 도입한 것입니다. 전통적으로 C#에서는 이러한 헬퍼 로직을 private 메서드나 람다 표현식으로 구현했지만, 각각 단점이 있었습니다:

- **Private 메서드**: 특정 메서드에서만 사용되는데도 클래스 전체에 노출되어 캡슐화가 약화됩니다.
- **람다 표현식**: 재귀가 복잡하고, 제네릭 타입 매개변수를 사용하기 어려우며, 델리게이트 할당으로 인한 성능 오버헤드가 있습니다.

로컬 함수는 이 두 접근법의 장점을 결합하여, **강한 캡슐화**와 **제로 오버헤드**를 동시에 제공합니다. Mads Torgersen(C# 언어 PM)은 로컬 함수를 "메서드 안의 메서드"라고 설명하며, 이것이 코드의 가독성과 유지보수성을 크게 향상시킨다고 강조했습니다.

**기본 구조:**

```csharp
void OuterMethod()
{
    Console.WriteLine("외부 메서드 시작");
    
    // 로컬 함수 정의 - 외부 메서드 내부에 중첩됨
    void InnerFunction()
    {
        Console.WriteLine("로컬 함수 실행");
    }
    
    // 로컬 함수 호출
    InnerFunction();
    InnerFunction();
    
    Console.WriteLine("외부 메서드 종료");
}

OuterMethod();

// 출력:
// 외부 메서드 시작
// 로컬 함수 실행
// 로컬 함수 실행
// 외부 메서드 종료
```

**로컬 함수의 특징과 장점:**

1. **범위 제한(Scope Restriction)**: 정의된 메서드 내부에서만 호출 가능합니다. 외부에서는 접근할 수 없어, **정보 은닉(Information Hiding)** 원칙을 강력하게 시행합니다.

2. **클로저(Closure) 지원**: 외부 메서드의 지역 변수, 매개변수, `this`에 자유롭게 접근할 수 있습니다. 이는 함수형 프로그래밍의 **자유 변수(Free Variables)** 캡처 메커니즘입니다.

3. **재귀 호출**: 로컬 함수는 자기 자신을 호출할 수 있어, 재귀 알고리즘을 우아하게 구현할 수 있습니다. 람다 표현식보다 재귀가 훨씬 간단합니다.

4. **제네릭 지원**: 타입 매개변수를 가질 수 있어, 제네릭 알고리즘을 로컬하게 구현할 수 있습니다.

5. **성능 효율성**: 델리게이트 할당 없이 직접 호출되므로, 람다 표현식보다 성능이 우수합니다(특히 반복 호출 시).

**클로저와 변수 캡처:**

로컬 함수는 **클로저(Closure)**를 형성하여 외부 범위의 변수를 캡처할 수 있습니다. 이는 함수형 프로그래밍의 핵심 개념으로, 함수가 자신이 정의된 환경을 "기억"합니다.

```csharp
void ProcessData(int[] numbers)
{
    int sum = 0;  // 외부 범위의 변수
    
    // 로컬 함수는 외부 변수를 캡처하여 접근 가능 (클로저)
    void AddToSum(int value)
    {
        sum += value;  // 외부 변수 sum을 수정
    }
    
    foreach (int num in numbers)
    {
        AddToSum(num);
    }
    
    Console.WriteLine($"합계: {sum}");
}

ProcessData(new int[] { 1, 2, 3, 4, 5 });

// 출력:
// 합계: 15
```

**컴파일러의 변수 캡처 구현:**

로컬 함수가 외부 변수를 캡처하면, 컴파일러는 내부적으로 **디스플레이 클래스(Display Class)** 또는 **환경 클로저 클래스(Environment Closure Class)**를 생성합니다. 이 클래스는 캡처된 변수들을 필드로 저장하며, 로컬 함수는 이 클래스의 멤버로 변환됩니다. 하지만 변수를 캡처하지 않으면 일반 메서드처럼 컴파일되어 오버헤드가 없습니다.

**반환값이 있는 로컬 함수:**

```csharp
void CalculateStatistics(int[] numbers)
{
    // 평균을 계산하는 로컬 함수
    double GetAverage()
    {
        double sum = 0;
        foreach (int num in numbers)  // 외부 매개변수 접근
        {
            sum += num;
        }
        return sum / numbers.Length;
    }
    
    // 최댓값을 찾는 로컬 함수
    int GetMax()
    {
        int max = numbers[0];
        foreach (int num in numbers)
        {
            if (num > max)
            {
                max = num;
            }
        }
        return max;
    }
    
    Console.WriteLine($"평균: {GetAverage():F2}");
    Console.WriteLine($"최댓값: {GetMax()}");
}

CalculateStatistics(new int[] { 5, 12, 8, 20, 3 });

// 출력:
// 평균: 9.60
// 최댓값: 20
```

**재귀 로컬 함수:**

로컬 함수는 재귀 호출을 매우 직관적으로 지원합니다. 람다 표현식으로 재귀를 구현하려면 복잡한 트릭이 필요하지만, 로컬 함수는 일반 메서드처럼 자연스럽게 자신을 호출할 수 있습니다.

```csharp
void PrintFactorial(int n)
{
    // 팩토리얼을 계산하는 재귀 로컬 함수
    long Factorial(int number)
    {
        if (number <= 1)
        {
            return 1;
        }
        return number * Factorial(number - 1);  // 재귀 호출
    }
    
    Console.WriteLine($"{n}! = {Factorial(n)}");
}

PrintFactorial(5);
PrintFactorial(7);

// 출력:
// 5! = 120
// 7! = 5040
```

**로컬 함수 vs 람다 표현식:**

로컬 함수와 람다 표현식은 비슷해 보이지만, 중요한 차이가 있습니다:

```csharp
void CompareLocalFunctionAndLambda()
{
    // 로컬 함수 - 메서드처럼 정의
    int LocalAdd(int a, int b)
    {
        return a + b;
    }
    
    // 람다식 - 델리게이트 변수에 할당
    Func<int, int, int> lambdaAdd = (a, b) => a + b;
    
    Console.WriteLine($"로컬 함수: {LocalAdd(5, 3)}");
    Console.WriteLine($"람다식: {lambdaAdd(5, 3)}");
}
```

**상세 비교 테이블:**

| 특성 | 로컬 함수 | 람다 표현식 |
|------|----------|-----------|
| **재귀 호출** | 간단하고 직관적 | 복잡함 (변수 미리 선언 필요) |
| **제네릭** | 완전 지원 | 제한적 |
| **오버로딩** | 가능 | 불가능 |
| **반환 타입** | 명시적 선언 | 추론됨 |
| **성능** | 델리게이트 할당 없음 (빠름) | 델리게이트 객체 생성 (약간 느림) |
| **디버깅** | 스택 트레이스에 이름 표시 | 익명이라 디버깅 어려움 |
| **가독성** | 명확한 메서드 형태 | 간결한 식 형태 |
| **사용 시기** | 복잡한 로직, 재귀, 헬퍼 메서드 | 간단한 식, LINQ 쿼리, 콜백 |

**제네릭 로컬 함수 (C# 8.0+):**

C# 8.0부터 로컬 함수도 제네릭 타입 매개변수를 가질 수 있습니다:

```csharp
void ProcessItems()
{
    // 제네릭 로컬 함수
    void PrintItem<T>(T item)
    {
        Console.WriteLine($"타입: {typeof(T).Name}, 값: {item}");
    }
    
    PrintItem(42);           // int 버전
    PrintItem("Hello");      // string 버전
    PrintItem(3.14);         // double 버전
}
```

**실용 예제 - 유효성 검증:**

로컬 함수는 복잡한 유효성 검증 로직을 깔끔하게 구조화하는 데 이상적입니다:

```csharp
bool ValidateUser(string username, string email, int age)
{
    // 사용자명 검증 로컬 함수
    bool IsValidUsername()
    {
        return !string.IsNullOrWhiteSpace(username) && 
               username.Length >= 3 &&
               username.Length <= 20;
    }
    
    // 이메일 검증 로컬 함수
    bool IsValidEmail()
    {
        return !string.IsNullOrWhiteSpace(email) && 
               email.Contains("@") &&
               email.Contains(".");
    }
    
    // 나이 검증 로컬 함수
    bool IsValidAge()
    {
        return age >= 0 && age <= 150;
    }
    
    // 모든 검증 수행 - 조기 반환 패턴
    if (!IsValidUsername())
    {
        Console.WriteLine("사용자명이 유효하지 않습니다.");
        return false;
    }
    
    if (!IsValidEmail())
    {
        Console.WriteLine("이메일이 유효하지 않습니다.");
        return false;
    }
    
    if (!IsValidAge())
    {
        Console.WriteLine("나이가 유효하지 않습니다.");
        return false;
    }
    
    Console.WriteLine("모든 검증 통과!");
    return true;
}

// 테스트
ValidateUser("홍길동", "hong@example.com", 25);
ValidateUser("김", "invalid-email", 30);

// 출력:
// 모든 검증 통과!
// 사용자명이 유효하지 않습니다.
```

**static 로컬 함수 (C# 8.0+):**

C# 8.0부터는 로컬 함수에 `static` 한정자를 추가할 수 있습니다. 이는 로컬 함수가 외부 변수를 캡처하지 못하도록 강제하여, 실수로 인한 변수 캡처를 방지합니다:

```csharp
void ProcessNumbers(int[] numbers)
{
    int sum = 0;
    
    // static 로컬 함수 - 외부 변수 캡처 불가
    static int Square(int x)
    {
        return x * x;
        // sum += x;  // ❌ 컴파일 오류: static 로컬 함수는 외부 변수 접근 불가
    }
    
    foreach (int num in numbers)
    {
        sum += Square(num);
    }
}
```

**언제 로컬 함수를 사용할까:**

1. **헬퍼 함수**: 특정 메서드에서만 사용되는 보조 로직
2. **반복 제거**: 같은 로직이 메서드 내에서 여러 번 반복될 때
3. **가독성 향상**: 복잡한 메서드를 논리적 단위로 분리할 때
4. **캡슐화**: 외부에 노출할 필요 없는 구현 세부사항을 숨길 때
5. **재귀 알고리즘**: 재귀 로직을 메서드 내부에 국한시킬 때
6. **유효성 검증**: 여러 검증 규칙을 명확하게 분리할 때
PrintFactorial(7);

// 출력:
// 5! = 120
// 7! = 5040
```

**로컬 함수 vs 람다식:**

로컬 함수는 람다식과 비슷해 보일 수 있지만 몇 가지 중요한 차이가 있습니다:

```csharp
void CompareLocalFunctionAndLambda()
{
    // 로컬 함수
    int LocalAdd(int a, int b)
    {
        return a + b;
    }
    
    // 람다식
    Func<int, int, int> lambdaAdd = (a, b) => a + b;
    
    Console.WriteLine($"로컬 함수: {LocalAdd(5, 3)}");
    Console.WriteLine($"람다식: {lambdaAdd(5, 3)}");
}
```

**로컬 함수의 장점:**

| 특성 | 로컬 함수 | 람다식 |
|------|----------|--------|
| 재귀 | 쉽게 가능 | 복잡함 |
| 성능 | 약간 더 효율적 | 델리게이트 생성 |
| 제네릭 | 지원 | 제한적 |
| 가독성 | 명확한 메서드 형태 | 간결한 표현 |

**실용 예제 - 유효성 검증:**

```csharp
bool ValidateUser(string username, string email, int age)
{
    // 사용자명 검증 로컬 함수
    bool IsValidUsername()
    {
        return !string.IsNullOrWhiteSpace(username) && username.Length >= 3;
    }
    
    // 이메일 검증 로컬 함수
    bool IsValidEmail()
    {
        return !string.IsNullOrWhiteSpace(email) && email.Contains("@");
    }
    
    // 나이 검증 로컬 함수
    bool IsValidAge()
    {
        return age >= 0 && age <= 150;
    }
    
    // 모든 검증 수행
    if (!IsValidUsername())
    {
        Console.WriteLine("사용자명이 유효하지 않습니다.");
        return false;
    }
    
    if (!IsValidEmail())
    {
        Console.WriteLine("이메일이 유효하지 않습니다.");
        return false;
    }
    
    if (!IsValidAge())
    {
        Console.WriteLine("나이가 유효하지 않습니다.");
        return false;
    }
    
    Console.WriteLine("모든 검증 통과!");
    return true;
}

// 테스트
ValidateUser("홍길동", "hong@example.com", 25);
ValidateUser("김", "invalid-email", 30);

// 출력:
// 모든 검증 통과!
// 사용자명이 유효하지 않습니다.
```

**언제 로컬 함수를 사용할까:**

1. **헬퍼 함수**: 특정 메서드에서만 사용되는 보조 함수
2. **반복 제거**: 같은 로직이 메서드 내에서 여러 번 반복될 때
3. **가독성**: 복잡한 메서드를 논리적 단위로 분리할 때
4. **캡슐화**: 외부에 노출할 필요 없는 로직을 숨길 때

---

## 7장 정리 및 요약

이 장에서는 C#에서 메서드를 정의하고 활용하는 다양한 방법을 학습했습니다.

### 핵심 개념 정리

1. **메서드 정의와 호출**
   - 메서드는 특정 작업을 수행하는 코드의 묶음
   - 코드 재사용성과 유지보수성 향상
   - 반환 타입, 메서드 이름, 매개변수로 구성

2. **매개변수와 반환값**
   - 값 타입: 복사본 전달, 원본 변경 안 됨
   - 참조 타입: 참조 전달, 원본 변경 가능 (불변 타입 제외)
   - `ref`: 초기화 필수, 양방향 전달
   - `out`: 초기화 불필요, 메서드에서 반드시 할당

3. **메서드 오버로딩**
   - 같은 이름, 다른 매개변수로 여러 버전 정의
   - 매개변수의 개수, 타입, 순서로 구분
   - 반환 타입만 다른 것은 오버로딩 불가

4. **선택적 매개변수와 명명된 인수**
   - 선택적 매개변수: 기본값을 가지는 매개변수
   - 명명된 인수: 매개변수 이름으로 값 전달
   - 코드의 유연성과 가독성 향상

5. **로컬 함수**
   - 메서드 내부에 정의되는 지역 함수
   - 외부 변수 접근 가능
   - 헬퍼 함수, 캡슐화에 유용

### 실습 문제

#### 문제 1: 기본 계산 메서드

네 가지 사칙연산을 수행하는 메서드를 작성하세요.

```csharp
int Add(int a, int b)
{
    return a + b;
}

int Subtract(int a, int b)
{
    return a - b;
}

int Multiply(int a, int b)
{
    return a * b;
}

double Divide(int a, int b)
{
    if (b == 0)
    {
        Console.WriteLine("0으로 나눌 수 없습니다.");
        return 0;
    }
    return (double)a / b;
}

// 테스트
Console.WriteLine($"10 + 5 = {Add(10, 5)}");
Console.WriteLine($"10 - 5 = {Subtract(10, 5)}");
Console.WriteLine($"10 × 5 = {Multiply(10, 5)}");
Console.WriteLine($"10 ÷ 5 = {Divide(10, 5)}");
```

#### 문제 2: 배열 처리 메서드

배열의 합계와 평균을 계산하는 메서드를 작성하세요.

```csharp
void PrintArrayStatistics(int[] numbers)
{
    // 합계를 계산하는 로컬 함수
    int GetSum()
    {
        int sum = 0;
        foreach (int num in numbers)
        {
            sum += num;
        }
        return sum;
    }
    
    // 평균을 계산하는 로컬 함수
    double GetAverage()
    {
        return (double)GetSum() / numbers.Length;
    }
    
    Console.WriteLine($"합계: {GetSum()}");
    Console.WriteLine($"평균: {GetAverage():F2}");
}

// 테스트
int[] scores = { 85, 90, 78, 92, 88 };
PrintArrayStatistics(scores);

// 출력:
// 합계: 433
// 평균: 86.60
```

#### 문제 3: 오버로딩을 사용한 출력 메서드

다양한 형태로 메시지를 출력하는 오버로드된 메서드를 작성하세요.

```csharp
void PrintMessage(string message)
{
    Console.WriteLine(message);
}

void PrintMessage(string message, int count)
{
    for (int i = 0; i < count; i++)
    {
        Console.WriteLine($"{i + 1}. {message}");
    }
}

void PrintMessage(string message, char border)
{
    string line = new string(border, message.Length + 4);
    Console.WriteLine(line);
    Console.WriteLine($"{border} {message} {border}");
    Console.WriteLine(line);
}

// 테스트
PrintMessage("안녕하세요");
Console.WriteLine();

PrintMessage("C# 학습", 3);
Console.WriteLine();

PrintMessage("중요 공지", '*');

// 출력:
// 안녕하세요
// 
// 1. C# 학습
// 2. C# 학습
// 3. C# 학습
// 
// **********
// * 중요 공지 *
// **********
```

#### 문제 4: ref/out 키워드 활용

두 변수의 값을 교환하는 메서드와 최대/최소값을 찾는 메서드를 작성하세요.

```csharp
void Swap(ref int a, ref int b)
{
    int temp = a;
    a = b;
    b = temp;
}

void FindMinMax(int[] numbers, out int min, out int max)
{
    min = numbers[0];
    max = numbers[0];
    
    foreach (int num in numbers)
    {
        if (num < min) min = num;
        if (num > max) max = num;
    }
}

// 테스트
int x = 10, y = 20;
Console.WriteLine($"교환 전: x={x}, y={y}");
Swap(ref x, ref y);
Console.WriteLine($"교환 후: x={x}, y={y}");

Console.WriteLine();

int[] values = { 15, 3, 27, 8, 42, 19 };
FindMinMax(values, out int minimum, out int maximum);
Console.WriteLine($"최솟값: {minimum}");
Console.WriteLine($"최댓값: {maximum}");

// 출력:
// 교환 전: x=10, y=20
// 교환 후: x=20, y=10
// 
// 최솟값: 3
// 최댓값: 42
```

### 다음 장 예고

8장 "고급 메서드 활용"에서는 메서드를 더욱 강력하게 활용하는 방법을 학습합니다:
- 람다 식 (Lambda Expressions)
- Func와 Action 델리게이트
- 확장 메서드 (Extension Methods)
- 재귀 함수 (Recursive Functions)

이를 통해 함수형 프로그래밍의 개념을 이해하고, 더욱 표현력 있는 코드를 작성할 수 있게 됩니다!
