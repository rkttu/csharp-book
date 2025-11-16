# 28장. Nullable 참조 타입

Nullable 참조 타입(Nullable Reference Types)은 C# 8.0에서 도입된 기능으로, null 참조 예외(NullReferenceException)를 방지하기 위한 컴파일 타임 안전장치입니다. 이 기능을 사용하면 변수가 null을 가질 수 있는지 여부를 명시적으로 표현할 수 있으며, 컴파일러가 잠재적인 null 참조 오류를 경고합니다.

## 28.1 Nullable 참조 타입이란?

전통적으로 C#에서 참조 타입(class, interface, delegate 등)은 기본적으로 null을 허용했습니다. 이로 인해 NullReferenceException은 가장 흔한 런타임 오류 중 하나였습니다.

```csharp
// 기존 C# - 모든 참조 타입이 null 가능
string name = null;  // 문제없이 컴파일됨
Console.WriteLine(name.Length);  // 런타임 오류: NullReferenceException!
```

C# 8.0부터는 참조 타입의 null 허용 여부를 명시적으로 구분할 수 있습니다:

```csharp
// Nullable 참조 타입 활성화 시
string name = null;    // 경고: null을 null 불허 타입에 할당
string? nullableName = null;  // OK: ? 표시로 null 허용을 명시
```

**주요 개념:**
- **null 불허 참조 타입**: 기본적으로 null을 허용하지 않는 참조 타입
- **null 허용 참조 타입**: `?`를 사용하여 null을 명시적으로 허용하는 참조 타입
- **컴파일 타임 분석**: 런타임이 아닌 컴파일 시점에 null 관련 경고 제공

**역사적 배경:**

Tony Hoare는 1965년 null 참조를 발명했지만, 이를 "10억 달러의 실수"라고 표현했습니다. Nullable 참조 타입은 이러한 역사적 문제를 해결하기 위한 C#의 접근 방식입니다.

## 28.2 프로젝트에서 활성화하기

Nullable 참조 타입 기능은 기본적으로 비활성화되어 있으므로, 프로젝트에서 명시적으로 활성화해야 합니다.

**방법 1: 프로젝트 파일(.csproj)에서 활성화**

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
  </PropertyGroup>
</Project>
```

**방법 2: 파일 단위로 활성화**

```csharp
#nullable enable

public class MyClass
{
    // 이 파일에서만 Nullable 참조 타입 활성화
}

#nullable disable  // 다시 비활성화
```

**Nullable 설정 옵션:**

| 설정 | 설명 |
|------|------|
| `enable` | Nullable 참조 타입 활성화 (권장) |
| `disable` | Nullable 참조 타입 비활성화 (기본값) |
| `warnings` | 경고만 활성화 |
| `annotations` | 주석만 활성화 |

**점진적 마이그레이션 전략:**

기존 프로젝트에 Nullable 참조 타입을 도입할 때는 단계적으로 진행하는 것이 좋습니다:

```csharp
// 1단계: 특정 파일에서만 활성화
#nullable enable
public class NewFeature
{
    public string Name { get; set; }  // null 불허
}
#nullable restore

// 2단계: 경고를 수정하면서 점진적으로 확대
// 3단계: 최종적으로 프로젝트 전체에 적용
```

## 28.3 null 허용 및 불허 타입

Nullable 참조 타입이 활성화되면, 참조 타입의 null 허용 여부를 명시적으로 표현할 수 있습니다.

**null 불허 타입 (Non-nullable):**

```csharp
string name;              // null을 허용하지 않음
name = "Hello";           // OK
name = null;              // 경고: null을 null 불허 타입에 할당
```

**null 허용 타입 (Nullable):**

```csharp
string? nullableName;     // null을 명시적으로 허용
nullableName = "Hello";   // OK
nullableName = null;      // OK: null 허용됨
```

**실전 예제:**

```csharp
public class Person
{
    public string FirstName { get; set; }      // null 불허 - 반드시 값이 있어야 함
    public string LastName { get; set; }       // null 불허
    public string? MiddleName { get; set; }    // null 허용 - 선택적 정보
    public string? Nickname { get; set; }      // null 허용
    
    public Person(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }
    
    public string GetFullName()
    {
        // MiddleName은 null일 수 있으므로 null 검사 필요
        if (MiddleName != null)
        {
            return $"{FirstName} {MiddleName} {LastName}";
        }
        return $"{FirstName} {LastName}";
    }
}
```

**null 허용성 흐름 분석:**

컴파일러는 코드의 흐름을 분석하여 변수가 null인지 아닌지를 추적합니다:

```csharp
string? message = GetMessage();

// 여기서는 message가 null일 수 있음
// Console.WriteLine(message.Length);  // 경고!

if (message != null)
{
    // 이 블록에서는 message가 null이 아님을 컴파일러가 인식
    Console.WriteLine(message.Length);  // OK
}

// null-forgiving 연산자 (!)
// 개발자가 null이 아님을 확신할 때 사용
Console.WriteLine(message!.Length);  // 경고 억제 (주의해서 사용)
```

**제네릭과 Nullable:**

```csharp
// 제네릭 타입 제약
public class Repository<T> where T : class
{
    public T? FindById(int id)  // null 허용 반환
    {
        // 데이터를 찾지 못하면 null 반환
        return null;
    }
    
    public void Save(T entity)  // null 불허 매개변수
    {
        ArgumentNullException.ThrowIfNull(entity);
        // entity를 저장
    }
}
```

## 28.4 null 병합 연산자 (??, ??=)

null 병합 연산자는 null 값을 처리하는 간결한 방법을 제공합니다.

**?? 연산자 (Null-coalescing operator):**

왼쪽 피연산자가 null이면 오른쪽 값을 반환하고, 그렇지 않으면 왼쪽 값을 반환합니다.

```csharp
string? name = null;
string displayName = name ?? "Unknown";  // "Unknown"

string? name2 = "John";
string displayName2 = name2 ?? "Unknown";  // "John"
```

**기존 방식과 비교:**

```csharp
// 기존 방식
string displayName;
if (name != null)
{
    displayName = name;
}
else
{
    displayName = "Unknown";
}

// ?? 연산자 사용
string displayName = name ?? "Unknown";
```

**??= 연산자 (Null-coalescing assignment):**

왼쪽 피연산자가 null이면 오른쪽 값을 할당합니다. (C# 8.0+)

```csharp
string? name = null;
name ??= "Default";  // name이 null이므로 "Default" 할당
Console.WriteLine(name);  // "Default"

name ??= "Another";  // name이 이미 값이 있으므로 변경 없음
Console.WriteLine(name);  // "Default"
```

**실전 예제:**

```csharp
public class UserSettings
{
    private string? _theme;
    
    public string Theme
    {
        get => _theme ?? "Light";  // null이면 기본값 "Light"
        set => _theme = value;
    }
    
    // 지연 초기화(Lazy initialization) 패턴
    private List<string>? _recentFiles;
    public List<string> RecentFiles
    {
        get
        {
            _recentFiles ??= new List<string>();  // null이면 새 리스트 생성
            return _recentFiles;
        }
    }
}
```

**연쇄적 null 병합:**

```csharp
string? primaryEmail = null;
string? secondaryEmail = null;
string? backupEmail = "backup@example.com";

string contactEmail = primaryEmail ?? secondaryEmail ?? backupEmail ?? "no-email@example.com";
Console.WriteLine(contactEmail);  // "backup@example.com"
```

**메서드 호출과 결합:**

```csharp
public class ConfigurationService
{
    private Dictionary<string, string>? _cache;
    
    public string GetValue(string key)
    {
        // _cache가 null이면 빈 딕셔너리 생성 후 TryGetValue 호출
        if ((_cache ??= new Dictionary<string, string>()).TryGetValue(key, out var value))
        {
            return value;
        }
        return "DefaultValue";
    }
}
```

## 28.5 null 조건부 연산자 (?., ?[])

null 조건부 연산자는 객체가 null일 때 안전하게 멤버에 접근할 수 있게 합니다.

**?. 연산자 (Null-conditional operator):**

객체가 null이 아닐 때만 멤버에 접근하고, null이면 null을 반환합니다.

```csharp
string? name = null;
int? length = name?.Length;  // name이 null이므로 length는 null
Console.WriteLine(length);   // (출력 없음 - null)

string? name2 = "John";
int? length2 = name2?.Length;  // name2가 null이 아니므로 length2는 4
Console.WriteLine(length2);     // 4
```

**기존 방식과 비교:**

```csharp
// 기존 방식
int? length;
if (name != null)
{
    length = name.Length;
}
else
{
    length = null;
}

// ?. 연산자 사용
int? length = name?.Length;
```

**?[] 연산자 (Null-conditional indexer):**

배열이나 인덱서가 null일 때 안전하게 접근합니다.

```csharp
string[]? names = null;
string? firstName = names?[0];  // names가 null이므로 firstName은 null

string[]? names2 = new[] { "John", "Jane" };
string? firstName2 = names2?[0];  // "John"
```

**연쇄 호출 (Method chaining):**

```csharp
public class Order
{
    public Customer? Customer { get; set; }
}

public class Customer
{
    public Address? Address { get; set; }
}

public class Address
{
    public string? City { get; set; }
}

Order? order = GetOrder();
string? city = order?.Customer?.Address?.City;
// order, Customer, Address 중 하나라도 null이면 city는 null
```

**메서드 호출과 결합:**

```csharp
public class Product
{
    public string? Name { get; set; }
    
    public string? GetUpperName()
    {
        return Name?.ToUpper();  // Name이 null이면 null 반환
    }
}

Product? product = null;
string? upperName = product?.GetUpperName();  // null
```

**이벤트 호출에서의 활용:**

```csharp
public class Button
{
    public event EventHandler? Clicked;
    
    protected void OnClick()
    {
        // 기존 방식
        if (Clicked != null)
        {
            Clicked(this, EventArgs.Empty);
        }
        
        // null 조건부 연산자 사용
        Clicked?.Invoke(this, EventArgs.Empty);
    }
}
```

**?? 연산자와 함께 사용:**

```csharp
string? name = null;
int length = name?.Length ?? 0;  // name이 null이므로 0
Console.WriteLine(length);  // 0

string? name2 = "John";
int length2 = name2?.Length ?? 0;  // name2가 null이 아니므로 4
Console.WriteLine(length2);  // 4
```

**실전 예제:**

```csharp
public class UserProfile
{
    public string? Username { get; set; }
    public ContactInfo? Contact { get; set; }
}

public class ContactInfo
{
    public string? Email { get; set; }
    public string? Phone { get; set; }
}

public class UserService
{
    public string GetContactEmail(UserProfile? profile)
    {
        // 여러 단계의 null 검사를 간결하게 처리
        return profile?.Contact?.Email ?? "no-email@example.com";
    }
    
    public void SendNotification(UserProfile? profile, string message)
    {
        // profile이나 Contact가 null이어도 안전
        string? email = profile?.Contact?.Email;
        
        if (email != null)
        {
            Console.WriteLine($"Sending to {email}: {message}");
        }
    }
}
```

**컬렉션과 LINQ에서의 활용:**

```csharp
List<Product>? products = GetProducts();

// null 안전하게 LINQ 쿼리 실행
int count = products?.Count ?? 0;
Product? firstProduct = products?.FirstOrDefault();
List<string>? names = products?.Select(p => p.Name).ToList();

// 메서드 체이닝
var result = products?
    .Where(p => p.Price > 100)
    .OrderBy(p => p.Name)
    .FirstOrDefault();
```

---

## 마치며

Nullable 참조 타입은 C#의 타입 안전성을 크게 향상시키는 기능입니다. 이 장에서 학습한 내용:

1. **Nullable 참조 타입**: null 허용 여부를 명시적으로 표현하여 NullReferenceException 방지
2. **프로젝트 활성화**: `.csproj` 파일이나 `#nullable` 지시문을 통한 활성화
3. **null 허용/불허 타입**: `?`를 사용한 명시적 null 허용성 표현
4. **null 병합 연산자**: `??`와 `??=`를 사용한 간결한 null 처리
5. **null 조건부 연산자**: `?.`와 `?[]`를 사용한 안전한 멤버 접근

**다음 단계:**
- 29장에서는 성능과 메모리 최적화 기법을 학습합니다
- Nullable 참조 타입을 기존 프로젝트에 점진적으로 적용해보세요
- null 관련 경고를 수정하면서 코드의 안전성을 높여보세요

**실습 과제:**
1. 기존 프로젝트에 Nullable 참조 타입 활성화하기
2. null 허용/불허 타입을 적절히 구분하여 모델 클래스 작성하기
3. null 병합 연산자와 null 조건부 연산자를 활용하여 코드 개선하기
