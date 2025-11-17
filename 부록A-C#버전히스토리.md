# 부록 A. C# 버전 히스토리

C#은 2000년 첫 출시 이후 지속적으로 발전해온 현대적인 프로그래밍 언어입니다. Microsoft의 Anders Hejlsberg가 이끄는 언어 설계팀은 개발자 커뮤니티의 피드백을 반영하며 C#을 계속 진화시켜 왔습니다. 이 부록에서는 C# 1.0부터 10까지의 주요 기능과 변화를 간략하게 살펴봅니다.

---

## A.1 C# 1.0 ~ 5.0

### C# 1.0 (2002년)

C# 1.0은 .NET Framework 1.0과 함께 출시된 첫 번째 버전입니다. Java와 C++의 영향을 받아 설계되었으며, Windows 애플리케이션 개발을 위한 현대적인 객체지향 언어로 등장했습니다.

**주요 기능:**
- 클래스, 인터페이스, 구조체 등 기본 객체지향 개념
- 속성(Properties)과 이벤트
- 델리게이트(Delegates)
- 특성(Attributes)
- foreach 문과 컬렉션 지원
- 가비지 컬렉션

```csharp
// C# 1.0 예제
public class Person
{
    private string name;
    
    public string Name
    {
        get { return name; }
        set { name = value; }
    }
}
```

### C# 2.0 (2005년)

제네릭(Generics)이 도입되어 타입 안전성과 성능이 크게 향상되었습니다. 이는 C# 언어의 표현력을 크게 높인 중요한 변화였습니다.

**주요 기능:**
- 제네릭(Generics): `List<T>`, `Dictionary<TKey, TValue>` 등
- 익명 메서드(Anonymous Methods)
- nullable 값 타입: `int?`, `bool?`
- 반복자(Iterators): `yield return`
- partial 클래스

```csharp
// 제네릭 사용 예제
List<string> names = new List<string>();
names.Add("Alice");
names.Add("Bob");

// nullable 값 타입
int? age = null;
```

### C# 3.0 (2007년)

LINQ(Language Integrated Query)의 도입으로 데이터 쿼리가 언어의 일급 시민이 되었습니다. 함수형 프로그래밍 개념이 C#에 도입된 중요한 버전입니다.

**주요 기능:**
- LINQ (Language Integrated Query)
- 람다 식(Lambda Expressions): `x => x * 2`
- 확장 메서드(Extension Methods)
- 익명 타입(Anonymous Types)
- 자동 구현 속성(Auto-implemented Properties)
- 객체 및 컬렉션 초기화 구문
- var 키워드를 통한 타입 추론

```csharp
// LINQ 예제
var numbers = new[] { 1, 2, 3, 4, 5 };
var evenNumbers = numbers.Where(n => n % 2 == 0);

// 자동 구현 속성
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

### C# 4.0 (2010년)

동적 타입과 상호 운용성이 개선되었으며, COM 상호 운용성이 크게 향상되었습니다.

**주요 기능:**
- dynamic 타입
- 명명된 인수(Named Arguments)와 선택적 매개변수(Optional Parameters)
- 제네릭 공변성과 반공변성(Covariance and Contravariance)
- COM 상호 운용성 개선

```csharp
// 선택적 매개변수와 명명된 인수
public void CreateUser(string name, int age = 18, string city = "Seoul")
{
    Console.WriteLine($"{name}, {age}, {city}");
}

CreateUser("Alice");  // age와 city는 기본값 사용
CreateUser("Bob", city: "Busan", age: 25);  // 명명된 인수
```

### C# 5.0 (2012년)

async/await 패턴의 도입으로 비동기 프로그래밍이 혁신적으로 간단해졌습니다. 이는 C#의 가장 성공적인 기능 중 하나로 평가받습니다.

**주요 기능:**
- async와 await 키워드
- 호출자 정보 특성(Caller Info Attributes)

```csharp
// async/await 예제
public async Task<string> FetchDataAsync(string url)
{
    using (var client = new HttpClient())
    {
        string result = await client.GetStringAsync(url);
        return result;
    }
}
```

---

## A.2 C# 6.0 ~ 9.0

### C# 6.0 (2015년)

코드를 더 간결하고 읽기 쉽게 만드는 다양한 문법적 개선이 이루어졌습니다.

**주요 기능:**
- 문자열 보간(String Interpolation): `$"Hello {name}"`
- null 조건부 연산자(Null-conditional Operator): `?.`
- 표현식 본문 멤버(Expression-bodied Members): `public int Square(int x) => x * x;`
- Auto-property 초기화: `public string Name { get; set; } = "Default";`
- nameof 연산자
- 예외 필터(Exception Filters): `catch (Exception ex) when (ex.Code == 404)`

```csharp
// C# 6.0 주요 기능 예제
public class Person
{
    public string Name { get; set; } = "Unknown";
    
    // 표현식 본문 메서드
    public string GetGreeting() => $"Hello, {Name}!";
    
    // null 조건부 연산자
    public int? GetLength(string text) => text?.Length;
}
```

### C# 7.0 (2017년)

튜플과 패턴 매칭이 도입되어 함수형 프로그래밍 스타일이 더욱 강화되었습니다.

**주요 기능:**
- 튜플(Tuples)과 해체(Deconstruction)
- 패턴 매칭(Pattern Matching): `is` 표현식과 `switch` 문
- out 변수 인라인 선언
- 로컬 함수(Local Functions)
- ref return과 ref local
- 숫자 리터럴 문법 개선: `1_000_000`, `0b1010`
- throw 식(Throw Expressions)

```csharp
// 튜플 예제
public (string Name, int Age) GetPerson()
{
    return ("Alice", 30);
}

var person = GetPerson();
Console.WriteLine($"{person.Name} is {person.Age} years old");

// 해체
var (name, age) = GetPerson();

// 패턴 매칭
object obj = "Hello";
if (obj is string text)
{
    Console.WriteLine(text.ToUpper());
}
```

### C# 7.1, 7.2, 7.3 (2017-2018년)

마이너 업데이트를 통해 점진적인 개선이 이루어졌습니다.

**C# 7.1 주요 기능:**
- async Main 메서드
- default 리터럴 표현식
- 튜플 요소 이름 추론

**C# 7.2 주요 기능:**
- readonly struct
- ref readonly
- private protected 접근 제한자

**C# 7.3 주요 기능:**
- 튜플 동등성 비교 지원
- 제네릭 제약 조건 개선

### C# 8.0 (2019년)

nullable 참조 타입이 도입되어 null 참조 예외를 컴파일 타임에 방지할 수 있게 되었습니다.

**주요 기능:**
- nullable 참조 타입(Nullable Reference Types)
- 비동기 스트림(Async Streams): `IAsyncEnumerable<T>`
- 범위와 인덱스(Ranges and Indices): `array[^1]`, `array[1..5]`
- switch 식(Switch Expressions)
- 속성 패턴(Property Patterns)
- using 선언(Using Declarations)
- 정적 로컬 함수(Static Local Functions)
- null 병합 할당(Null-coalescing Assignment): `??=`

```csharp
// nullable 참조 타입
#nullable enable
public class User
{
    public string Name { get; set; }  // null이 아닌 문자열
    public string? Nickname { get; set; }  // null 가능 문자열
}

// switch 식
string GetSeasonName(int month) => month switch
{
    12 or 1 or 2 => "겨울",
    3 or 4 or 5 => "봄",
    6 or 7 or 8 => "여름",
    9 or 10 or 11 => "가을",
    _ => "알 수 없음"
};

// 범위와 인덱스
int[] numbers = { 1, 2, 3, 4, 5 };
var last = numbers[^1];  // 5 (마지막 요소)
var middle = numbers[1..4];  // { 2, 3, 4 }
```

### C# 9.0 (2020년)

record 타입이 도입되어 불변 데이터 모델을 쉽게 만들 수 있게 되었습니다.

**주요 기능:**
- record 타입(Reference Type Records)
- init-only 속성(Init-only Properties)
- 최상위 문(Top-level Statements)
- 패턴 매칭 개선
- 대상 타입 new 식(Target-typed New Expressions)
- 함수 포인터(Function Pointers)
- 로컬 함수의 특성 지원

```csharp
// record 타입
public record Person(string FirstName, string LastName);

var person1 = new Person("John", "Doe");
var person2 = person1 with { LastName = "Smith" };

// 최상위 문 (Program.cs)
Console.WriteLine("Hello, World!");
// Main 메서드 없이 바로 실행 가능

// 대상 타입 new
List<int> numbers = new() { 1, 2, 3 };
```

---

## A.3 C# 10 주요 변경사항

C# 10은 .NET 6와 함께 2021년 11월에 출시되었으며, 코드의 간결성과 생산성을 대폭 향상시키는 기능들이 추가되었습니다.

### Global using 지시문

프로젝트의 모든 파일에서 공통으로 사용되는 네임스페이스를 한 곳에서 선언할 수 있습니다.

```csharp
// GlobalUsings.cs
global using System;
global using System.Collections.Generic;
global using System.Linq;

// 다른 파일에서는 using 선언 없이 사용 가능
public class MyClass
{
    public List<string> GetNames() => new() { "Alice", "Bob" };
}
```

### File-scoped 네임스페이스

네임스페이스 선언을 세미콜론으로 끝내어 한 단계의 들여쓰기를 제거할 수 있습니다.

```csharp
// 기존 방식
namespace MyCompany.MyProduct
{
    public class MyClass
    {
        // 코드
    }
}

// File-scoped 네임스페이스
namespace MyCompany.MyProduct;

public class MyClass
{
    // 코드
}
```

### 개선된 람다 식

람다 식에 특성을 적용하고 명시적 반환 타입을 지정할 수 있게 되었습니다.

```csharp
// 명시적 반환 타입
var square = int (int x) => x * x;

// 특성 적용 (ASP.NET Minimal API에서 유용)
app.MapGet("/", [Authorize] () => "Hello World!");
```

### Record struct

C# 9의 record를 값 타입으로 확장한 기능입니다.

```csharp
// record struct 정의
public record struct Point(int X, int Y);

var point = new Point(10, 20);
Console.WriteLine(point);  // Point { X = 10, Y = 20 }

// with 식 사용 가능
var newPoint = point with { X = 30 };
```

### 필수 속성 (Required Properties)

C# 11에서 정식 도입되었지만 C# 10의 흐름을 이어받은 기능으로, 객체 초기화 시 반드시 설정해야 하는 속성을 지정할 수 있습니다.

```csharp
public class Person
{
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
    public int Age { get; set; }  // 선택적
}

// 필수 속성을 설정하지 않으면 컴파일 오류
var person = new Person
{
    FirstName = "John",
    LastName = "Doe"
};
```

### 상수 문자열 보간 개선

상수 문자열에도 보간을 사용할 수 있게 되었습니다.

```csharp
const string Language = "C#";
const string Version = "10";
const string Message = $"{Language} {Version}";  // C# 10에서 가능
```

### 확장 속성 패턴

중첩된 속성을 더 간결하게 패턴 매칭할 수 있습니다.

```csharp
// 기존 방식
if (person is { Address: { City: "Seoul" } })

// C# 10
if (person is { Address.City: "Seoul" })
```

---

## 마치며

C#은 지난 20년 간 지속적으로 발전하여 현대적이고 강력한 프로그래밍 언어로 자리잡았습니다. 초기의 객체지향 기반 언어에서 시작하여, LINQ를 통한 함수형 프로그래밍 지원, async/await를 통한 비동기 프로그래밍 혁신, record를 통한 불변 데이터 모델링, 그리고 nullable 참조 타입을 통한 null 안전성 강화까지, C#은 개발자의 생산성과 코드 품질을 지속적으로 향상시켜 왔습니다.

각 버전의 주요 기능들은 단순히 새로운 문법을 추가하는 것을 넘어, 더 안전하고, 더 간결하며, 더 표현력 있는 코드를 작성할 수 있도록 돕습니다. 이러한 발전은 앞으로도 계속될 것이며, C# 11, 12 그리고 그 이후의 버전들도 개발자 커뮤니티의 요구를 반영하여 지속적으로 진화할 것입니다.

**참고 자료:**
- [C# 공식 문서](https://docs.microsoft.com/ko-kr/dotnet/csharp/)
- [C# 언어 버전 관리](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/configure-language-version)
- [C# 버전 및 지원](https://docs.microsoft.com/ko-kr/dotnet/csharp/whats-new/)
