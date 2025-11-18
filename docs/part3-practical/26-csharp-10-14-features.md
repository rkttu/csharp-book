# 26장. C# 10-14 주요 기능

C# 10부터 14까지의 버전은 언어의 간결성, 표현력, 성능을 지속적으로 향상시킨 중요한 릴리스들입니다. 이 장에서는 C# 10 (2021년, .NET 6), C# 11 (2022년, .NET 7), C# 12 (2023년, .NET 8), C# 13 (2024년, .NET 9), 그리고 C# 14 (프리뷰, .NET 10)의 주요 기능들을 체계적으로 살펴봅니다.

**각 버전의 핵심 주제:**

- **C# 10**: Global using, File-scoped namespace, Record struct - 코드 간결성
- **C# 11**: Raw string literals, Generic attributes, List patterns - 문자열과 제네릭의 진화
- **C# 12**: Primary constructors, Collection expressions - 간결성의 극대화
- **C# 13**: params collections, New Lock type - 성능과 유연성
- **C# 14 (프리뷰)**: Field keyword, Extensions - 미래를 향한 진화

## 이 장에서 배울 내용

### C# 10 기능
- Global using 지시문
- File-scoped 네임스페이스
- 개선된 람다 식
- Record struct
- 필수 속성 (Required Properties)

### C# 11 기능
- Raw string literals (원시 문자열 리터럴)
- Generic attributes (제네릭 특성)
- List patterns (리스트 패턴)
- Required members (필수 멤버)
- File-local types (파일 범위 타입)
- Generic math support (제네릭 수학)

### C# 12 기능
- Primary constructors (주 생성자)
- Collection expressions (컬렉션 식)
- Default lambda parameters (람다 기본 매개변수)
- Alias any type (타입 별칭 확장)
- Inline arrays (인라인 배열)
- ref readonly parameters

### C# 13 기능
- params collections (params 컬렉션)
- New Lock type (새로운 lock 타입)
- New escape sequence: \e
- ref struct interfaces
- Overload resolution priority

### C# 14 프리뷰 기능
- Field keyword (필드 키워드)
- Extensions (확장 타입)
- Discriminated unions (판별된 합 타입)
- 패턴 매칭 개선

---

# 제1부: C# 10 주요 기능

C# 10은 2021년 11월 .NET 6와 함께 출시된 버전으로, 언어의 간결성과 표현력을 대폭 향상시킨 주요 릴리스입니다. Anders Hejlsberg와 Mads Torgersen이 이끄는 C# 언어 설계팀은 개발자의 생산성을 높이고 보일러플레이트 코드를 줄이는 데 초점을 맞췄습니다. 특히 이 버전은 "minimal API"와 같은 현대적 개발 패턴을 지원하기 위해 문법을 대폭 개선했으며, record struct의 도입으로 값 타입의 표현력을 크게 향상시켰습니다.

**C# 10의 설계 철학:**

C# 10의 개발은 세 가지 핵심 원칙을 중심으로 진행되었습니다:

1. **간결성(Conciseness)**: Global using과 File-scoped namespace를 통해 반복적인 코드를 제거하고, 실제 비즈니스 로직에 집중할 수 있게 합니다.

2. **일관성(Consistency)**: Record struct의 도입으로 참조 타입(record class)과 값 타입(record struct) 간의 일관된 문법을 제공합니다.

3. **표현력(Expressiveness)**: 개선된 람다 식과 Required Properties를 통해 의도를 더 명확하게 표현할 수 있습니다.

**현대 C# 개발의 변화:**

C# 10은 .NET 6의 "minimal API" 패러다임과 함께 등장하여, 마이크로서비스와 클라우드 네이티브 애플리케이션 개발에 최적화되었습니다. 전통적인 ASP.NET 애플리케이션이 수십 줄의 보일러플레이트 코드를 요구했다면, C# 10과 .NET 6는 단 몇 줄로 웹 서버를 구축할 수 있게 합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 C# 10의 주요 기능들을 체계적으로 학습하게 됩니다:

- **Global using 지시문**: 프로젝트 전체에서 공통으로 사용되는 네임스페이스를 한 곳에서 관리하여 각 파일의 using 선언을 줄이는 방법을 배웁니다.

- **File-scoped 네임스페이스**: 한 단계의 들여쓰기를 제거하여 코드의 가독성을 높이고, 파일 전체를 하나의 네임스페이스로 간단하게 선언하는 방법을 익힙니다.

- **개선된 람다 식**: 람다 식에 특성(Attributes)을 적용하고, 명시적 반환 타입을 지정하며, var를 사용한 자연스러운 타입 추론을 활용하는 방법을 학습합니다.

- **Record struct**: 불변 값 타입을 우아하게 표현하는 record struct의 개념과 활용법을 배우며, record class와의 차이점을 이해합니다.

- **필수 속성(Required Properties)**: 객체 초기화 시 반드시 설정해야 하는 속성을 컴파일 타임에 검증하는 방법을 익힙니다.

**학습 목표:**
- C# 10의 새로운 기능들을 실무에 적용하는 방법 이해
- 코드의 간결성과 가독성을 높이는 현대적 C# 작성법 습득
- .NET 6 이상의 프로젝트에서 생산성을 향상시키는 기법 체득

---

## 26.1 Global using 지시문

Global using 지시문은 C# 10에서 도입된 기능으로, 프로젝트의 모든 파일에서 공통으로 사용되는 네임스페이스를 한 곳에서 선언할 수 있게 합니다. 이는 Java의 import static이나 Python의 __init__.py와 유사한 개념으로, 반복적인 using 선언을 제거하여 코드의 간결성을 크게 향상시킵니다.

**역사적 배경과 설계 동기:**

C# 언어 설계팀은 수년간 개발자 커뮤니티로부터 "반복적인 using 선언의 부담"에 대한 피드백을 받아왔습니다. 특히 ASP.NET Core 프로젝트에서는 거의 모든 파일이 동일한 10~15개의 using 선언으로 시작하는 패턴이 일반적이었습니다. 이는 DRY(Don't Repeat Yourself) 원칙에 위배되며, 새로운 파일을 생성할 때마다 이러한 선언을 복사하거나 IDE의 자동 완성에 의존해야 하는 불편함이 있었습니다.

Global using은 이러한 문제를 해결하기 위해 도입되었으며, C# 9의 Top-level statements와 함께 "minimal API" 패러다임을 완성하는 핵심 기능이 되었습니다. Microsoft는 .NET 6 SDK 프로젝트 템플릿에서 이 기능을 기본으로 활성화하여, 신규 프로젝트의 초기 학습 곡선을 낮추고 코드의 가독성을 높였습니다.

**문제 상황 - 전통적 접근의 한계:**

전통적인 C# 프로젝트에서는 거의 모든 파일이 동일한 using 선언으로 시작합니다:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

// 실제 코드는 여기서부터 시작
public class MyClass
{
    // ...
}
```

대규모 프로젝트에서는 수백 개의 파일에 이러한 반복적인 선언이 존재하며, 이는 다음과 같은 문제를 야기합니다:

1. **코드 중복(Code Duplication)**: DRY 원칙 위반으로 유지보수 부담 증가
2. **파일 크기 증가**: 실제 비즈니스 로직보다 using 선언이 더 많은 경우도 발생
3. **일관성 문제**: 팀원마다 다른 using 순서나 누락된 선언으로 인한 혼란
4. **리팩토링 어려움**: 공통 네임스페이스 추가 시 모든 파일을 수정해야 함
5. **신규 개발자의 진입 장벽**: 어떤 using이 필수인지 파악하는 데 시간 소요

**통계적 영향:**

Microsoft의 내부 분석에 따르면, ASP.NET Core 프로젝트에서:
- 평균적으로 각 .cs 파일의 상위 10~20줄이 using 선언
- 100개 파일 프로젝트에서 약 1,000~2,000줄이 반복적인 using 선언
- 이는 전체 코드베이스의 약 5~10%에 해당하는 중복

**Global using의 해결책:**

Global using을 사용하면 프로젝트의 한 파일(일반적으로 `GlobalUsings.cs`)에서 공통 네임스페이스를 선언할 수 있습니다:

```csharp
// GlobalUsings.cs
global using System;
global using System.Collections.Generic;
global using System.Linq;
global using System.Threading.Tasks;
```

이제 다른 모든 파일에서는 이러한 using 선언 없이 바로 시작할 수 있습니다:

```csharp
// MyClass.cs
public class MyClass
{
    // System, List<T>, LINQ 등을 바로 사용 가능
    public List<string> GetNames()
    {
        return new List<string> { "Alice", "Bob" };
    }
}
```

**컴파일러의 처리 방식:**

Global using은 컴파일 시점에 처리되며, C# 컴파일러(Roslyn)는 다음과 같은 단계로 이를 처리합니다:

1. **수집 단계(Collection Phase)**: 프로젝트의 모든 소스 파일을 스캔하여 `global using` 선언을 수집
2. **정렬 단계(Ordering Phase)**: 선언 순서와 관계없이 알파벳순으로 정렬하여 일관성 보장
3. **주입 단계(Injection Phase)**: 각 소스 파일에 global using을 논리적으로 주입 (실제 파일은 변경되지 않음)
4. **이름 확인 단계(Name Resolution)**: 일반 using과 동일한 방식으로 타입 이름 확인

이는 다음과 같은 장점을 제공합니다:
- **컴파일 시간 영향 최소화**: 한 번만 파싱하고 모든 파일에 재사용
- **순서 독립적**: 어느 파일에 선언되든 동일한 결과
- **충돌 감지**: 중복 선언이나 모호한 타입 참조를 컴파일 타임에 감지

**실전 예제:**

```csharp
// GlobalUsings.cs - 프로젝트 전체에서 사용할 네임스페이스 정의
global using System;
global using System.Collections.Generic;
global using System.Linq;
global using System.Text;
global using System.Threading.Tasks;
global using System.Text.Json;

// 프로젝트 특정 네임스페이스도 추가 가능
global using MyCompany.Common;
global using MyCompany.Models;
```

```csharp
// Product.cs - using 선언 없이 시작
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

```csharp
// ProductService.cs - 역시 using 선언 없이 시작
public class ProductService
{
    private readonly List<Product> products = new();
    
    public void AddProduct(Product product)
    {
        products.Add(product);
    }
    
    public List<Product> GetExpensiveProducts(decimal minPrice)
    {
        return products.Where(p => p.Price >= minPrice).ToList();
    }
    
    public string SerializeProducts()
    {
        return JsonSerializer.Serialize(products);
    }
}
```

**고급 사용 패턴:**

### 1. 네임스페이스 별칭(Namespace Aliases)

Global using은 별칭과 함께 사용하여 긴 네임스페이스나 충돌하는 타입 이름을 해결할 수 있습니다:

```csharp
// GlobalUsings.cs
global using Json = System.Text.Json.JsonSerializer;
global using NewtonsoftJson = Newtonsoft.Json.JsonConvert;
global using IO = System.IO;

// 사용 파일
public class DataProcessor
{
    public string SerializeWithSystemTextJson(object data)
    {
        return Json.Serialize(data);  // System.Text.Json.JsonSerializer
    }
    
    public string SerializeWithNewtonsoft(object data)
    {
        return NewtonsoftJson.SerializeObject(data);  // Newtonsoft.Json.JsonConvert
    }
    
    public void SaveToFile(string path, string content)
    {
        IO.File.WriteAllText(path, content);  // System.IO.File
    }
}
```

### 2. Static Using과 결합

전역으로 정적 멤버를 가져올 수도 있습니다:

```csharp
// GlobalUsings.cs
global using static System.Console;
global using static System.Math;
global using static System.String;

// 사용 파일
public class MathHelper
{
    public void Calculate()
    {
        WriteLine("계산 시작...");  // Console.WriteLine 대신
        
        var result = Sqrt(16);  // Math.Sqrt 대신
        WriteLine($"결과: {result}");
        
        var text = Join(", ", new[] { "a", "b", "c" });  // String.Join 대신
        WriteLine(text);
    }
}
```

### 3. 조건부 컴파일과 결합

전처리기 지시문을 사용하여 환경별로 다른 네임스페이스를 포함할 수 있습니다:

```csharp
// GlobalUsings.cs
global using System;
global using System.Collections.Generic;

#if DEBUG
global using System.Diagnostics;
global using MyCompany.Logging.Verbose;
#else
global using MyCompany.Logging.Production;
#endif

#if USE_NEWTONSOFT_JSON
global using Newtonsoft.Json;
#else
global using System.Text.Json;
#endif
```

**암시적 Global Usings:**

.NET 6 이상의 SDK 스타일 프로젝트에서는 `<ImplicitUsings>enable</ImplicitUsings>` 설정을 통해 프레임워크가 자동으로 일반적인 네임스페이스를 포함시킵니다:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```

이 설정은 프로젝트 유형에 따라 자동으로 다음 네임스페이스들을 포함합니다:

**콘솔/라이브러리 프로젝트 (Microsoft.NET.Sdk):**
```csharp
global using System;
global using System.Collections.Generic;
global using System.IO;
global using System.Linq;
global using System.Net.Http;
global using System.Threading;
global using System.Threading.Tasks;
```

**ASP.NET Core 프로젝트 (Microsoft.NET.Sdk.Web):**
위의 네임스페이스에 추가로:
```csharp
global using Microsoft.AspNetCore.Builder;
global using Microsoft.AspNetCore.Hosting;
global using Microsoft.AspNetCore.Http;
global using Microsoft.AspNetCore.Routing;
global using Microsoft.Extensions.Configuration;
global using Microsoft.Extensions.DependencyInjection;
global using Microsoft.Extensions.Hosting;
global using Microsoft.Extensions.Logging;
```

**Worker Service 프로젝트 (Microsoft.NET.Sdk.Worker):**
```csharp
global using Microsoft.Extensions.Hosting;
global using Microsoft.Extensions.DependencyInjection;
global using Microsoft.Extensions.Logging;
```

**암시적 usings 제어:**

특정 네임스페이스를 제거하거나 추가할 수 있습니다:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
  
  <ItemGroup>
    <!-- 암시적으로 포함된 네임스페이스 제거 -->
    <Using Remove="System.Net.Http" />
    
    <!-- 추가 네임스페이스를 암시적으로 포함 -->
    <Using Include="MyCompany.Core" />
    <Using Include="MyCompany.Models" />
    
    <!-- 별칭과 함께 추가 -->
    <Using Include="System.Text.Json" Alias="Json" />
  </ItemGroup>
</Project>
```

**모범 사례와 권장사항:**

1. **프로젝트 전체에서 자주 사용하는 네임스페이스만 포함**: 
   - 80% 이상의 파일에서 사용되는 네임스페이스가 적절
   - 특정 기능에만 필요한 네임스페이스는 로컬 using 사용

2. **GlobalUsings.cs 파일 구조화**:
```csharp
// GlobalUsings.cs
// ============================================
// .NET Framework 기본 네임스페이스
// ============================================
global using System;
global using System.Collections.Generic;
global using System.Linq;
global using System.Threading.Tasks;

// ============================================
// 프로젝트 공통 네임스페이스
// ============================================
global using MyCompany.Core;
global using MyCompany.Core.Extensions;
global using MyCompany.Core.Exceptions;

// ============================================
// 써드파티 라이브러리
// ============================================
global using Newtonsoft.Json;
global using AutoMapper;

// ============================================
// 별칭 정의
// ============================================
global using Json = System.Text.Json.JsonSerializer;
global using Validation = System.ComponentModel.DataAnnotations;
```

3. **명시적으로 필요한 경우 로컬 using 사용**: 
   특정 파일에서만 필요한 네임스페이스는 해당 파일에 선언하여 의도를 명확히 합니다.

```csharp
// SpecializedService.cs
using System.Security.Cryptography;  // 이 파일에서만 사용
using System.Net.Security;  // 이 파일에서만 사용

public class SpecializedService
{
    // 암호화 관련 코드...
}
```

4. **팀 컨벤션 수립**: 
   - `GlobalUsings.cs` 파일의 위치와 명명 규칙 통일
   - 네임스페이스 추가/제거 시 팀 리뷰 프로세스 적용
   - 프로젝트별 가이드라인 문서화

5. **성능 고려사항**:
   - Global using은 컴파일 시간에 미미한 영향만 줌
   - IntelliSense는 global using을 인식하여 자동 완성 제공
   - 런타임 성능에는 전혀 영향 없음 (컴파일 후 IL 코드는 동일)

**안티패턴과 주의사항:**

1. **모든 네임스페이스를 global로 만들지 않기**:
```csharp
// ❌ 나쁜 예: 너무 많은 global using
global using System.Data.SqlClient;  // 대부분의 파일에서 불필요
global using System.Drawing;  // 일부 파일에서만 사용
global using System.Windows.Forms;  // 특정 UI 파일에서만 필요
```

2. **네임스페이스 오염(Namespace Pollution) 방지**:
```csharp
// ❌ 나쁜 예: static using 남용
global using static System.Console;
global using static System.Math;
global using static System.String;
global using static System.Linq.Enumerable;
// 너무 많은 static 멤버가 글로벌 스코프에 노출됨
```

3. **타입 이름 충돌 주의**:
```csharp
// ❌ 문제 발생 가능
global using MyCompany.Models;  // Task 클래스 포함
global using System.Threading.Tasks;  // Task 클래스 포함

// 해결 방법: 별칭 사용
global using DomainTask = MyCompany.Models.Task;
global using System.Threading.Tasks;
```

**실무 시나리오 - 마이그레이션 전략:**

기존 프로젝트에 Global using 도입 시 단계적 접근:

```csharp
// 1단계: 현재 프로젝트의 using 패턴 분석
// 스크립트나 도구로 가장 자주 사용되는 네임스페이스 식별

// 2단계: GlobalUsings.cs 생성 및 상위 10개 네임스페이스 추가
// GlobalUsings.cs
global using System;
global using System.Collections.Generic;
global using System.Linq;
global using System.Threading.Tasks;
global using MyCompany.Core;

// 3단계: 점진적으로 로컬 using 제거
// IDE의 "Remove unnecessary usings" 기능 활용

// 4단계: 빌드 및 테스트로 검증
// CI/CD 파이프라인에서 자동 검증

// 5단계: 팀원들과 공유 및 피드백 수집
```

**디버깅과 문제 해결:**

Global using 관련 문제 발생 시:

```csharp
// 문제: "타입을 찾을 수 없음" 오류
// 해결 1: GlobalUsings.cs가 프로젝트에 포함되었는지 확인
// 해결 2: 해당 네임스페이스가 참조된 어셈블리에 있는지 확인

// 문제: 타입 이름 모호성
// 예: The type 'Task' exists in both 'System.Threading.Tasks' and 'MyCompany.Models'
// 해결: 명시적 네임스페이스 사용 또는 별칭 정의

public class TaskProcessor
{
    public void Process(System.Threading.Tasks.Task task) { }  // 명시적
    public void Process(DomainTask task) { }  // 별칭 사용 (global using 정의 필요)
}
```

---

## 26.2 File-scoped 네임스페이스

File-scoped 네임스페이스는 C# 10에서 도입된 문법으로, 파일 전체를 하나의 네임스페이스로 선언할 때 중괄호와 한 단계의 들여쓰기를 제거할 수 있게 합니다. 이는 Python의 모듈 개념이나 Go의 패키지 선언과 유사한 간결함을 제공하며, "one file, one namespace" 원칙을 코드 수준에서 강제하는 메커니즘입니다.

**역사적 배경과 설계 철학:**

C#의 네임스페이스 시스템은 C++ 11의 네임스페이스와 Java의 패키지 개념을 결합하여 설계되었습니다. 전통적으로 C#은 한 파일에 여러 네임스페이스를 허용했지만, 실무에서는 "한 파일에 하나의 네임스페이스"가 압도적으로 일반적인 패턴이었습니다. Microsoft의 코드 분석에 따르면, .NET Core 런타임 코드베이스의 99.9% 이상이 이 패턴을 따르고 있었습니다.

File-scoped 네임스페이스는 이러한 실무 패턴을 인식하고, Robert C. Martin의 "Clean Code" 원칙 중 하나인 "코드는 의도를 명확히 표현해야 한다"는 철학을 구현합니다. 중괄호를 제거함으로써:

1. **수직 공간 절약**: 80줄 파일에서 2줄 절약은 2.5% 향상
2. **수평 공간 절약**: 4칸 들여쓰기 제거로 가독성 향상 (특히 120자 줄 제한 환경)
3. **의도의 명확화**: 파일 구조가 네임스페이스 구조를 반영함을 명시적으로 표현
4. **일관성 강제**: 한 파일 = 한 네임스페이스 규칙을 문법 수준에서 강제

**전통적인 네임스페이스 선언의 문제점:**

```csharp
using System;

namespace MyCompany.MyProduct.MyFeature
{
    public class Calculator
    {
        public int Add(int a, int b)
        {
            return a + b;
        }
    }
}
```

전체 파일이 한 단계 들여쓰기되어 있으며, 다음과 같은 문제가 있습니다:

1. **불필요한 들여쓰기**: 모든 코드가 한 레벨 깊어짐
2. **수평 공간 낭비**: 긴 네임스페이스 + 들여쓰기 시 120자 제한 도달 쉬움
3. **중괄호 매칭**: 파일이 길어질수록 여는 중괄호와 닫는 중괄호 매칭이 어려움
4. **네임스페이스 변경 시**: 전체 파일의 들여쓰기 재조정 필요

**File-scoped 네임스페이스:**

```csharp
using System;

namespace MyCompany.MyProduct.MyFeature;

public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```

세미콜론(`;`)으로 네임스페이스를 선언하면, 파일의 나머지 모든 코드가 해당 네임스페이스에 속하게 됩니다.

**실전 예제:**

```csharp
// Product.cs
namespace MyCompany.Models;

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public DateTime CreatedAt { get; set; }
}
```

```csharp
// IProductRepository.cs
namespace MyCompany.Repositories;

public interface IProductRepository
{
    void Add(Product product);
    Product? GetById(int id);
    List<Product> GetAll();
    void Update(Product product);
    void Delete(int id);
}
```

```csharp
// ProductRepository.cs
namespace MyCompany.Repositories;

public class ProductRepository : IProductRepository
{
    private readonly List<Product> products = new();
    
    public void Add(Product product)
    {
        products.Add(product);
    }
    
    public Product? GetById(int id)
    {
        return products.FirstOrDefault(p => p.Id == id);
    }
    
    public List<Product> GetAll()
    {
        return products.ToList();
    }
    
    public void Update(Product product)
    {
        var existing = GetById(product.Id);
        if (existing != null)
        {
            var index = products.IndexOf(existing);
            products[index] = product;
        }
    }
    
    public void Delete(int id)
    {
        var product = GetById(id);
        if (product != null)
        {
            products.Remove(product);
        }
    }
}
```

**장점:**

1. **가독성 향상**: 한 단계 들여쓰기가 줄어들어 코드가 더 명확하고 읽기 쉬워집니다.
2. **수평 공간 절약**: 특히 긴 네임스페이스 이름을 사용할 때 화면에 더 많은 코드가 보입니다.
3. **현대적 스타일**: .NET 6 이상의 새 프로젝트 템플릿에서 기본으로 사용됩니다.
4. **중괄호 매칭 불필요**: 파일이 길어져도 네임스페이스 중괄호 찾기 불필요
5. **리팩토링 용이성**: 네임스페이스 이름 변경 시 들여쓰기 재조정 불필요

**제약 사항:**

1. **한 파일에 하나의 네임스페이스만**: File-scoped 네임스페이스는 파일당 하나만 선언할 수 있습니다.
2. **전통적 방식과 혼용 불가**: 같은 파일에서 file-scoped와 전통적 네임스페이스를 함께 사용할 수 없습니다.
3. **하위 네임스페이스 불가**: 중첩된 네임스페이스 선언 불가능

```csharp
// ❌ 컴파일 오류: file-scoped 네임스페이스 내에 또 다른 네임스페이스 불가
namespace MyCompany.Product;

namespace MyCompany.Product.Internal  // 오류!
{
}
```

**컴파일러의 처리 방식과 IL 코드:**

File-scoped 네임스페이스는 순전히 문법적 편의 기능(syntactic sugar)입니다. 컴파일 후 생성되는 IL(Intermediate Language) 코드는 전통적 방식과 완전히 동일합니다:

```csharp
// C# 코드 (file-scoped)
namespace MyCompany.Models;

public class Product
{
    public string Name { get; set; }
}
```

```csharp
// C# 코드 (전통적)
namespace MyCompany.Models
{
    public class Product
    {
        public string Name { get; set; }
    }
}
```

두 코드 모두 동일한 IL로 컴파일됩니다:
```il
.class public auto ansi beforefieldinit 
    MyCompany.Models.Product extends [System.Runtime]System.Object
{
    // IL 코드는 완전히 동일
}
```

이는 다음을 의미합니다:
- **런타임 성능**: 완전히 동일
- **메타데이터**: 차이 없음
- **리플렉션**: 동일한 타입 정보 반환
- **상호 운용성**: 다른 어셈블리에서 사용 시 차이 없음

**고급 사용 패턴:**

### 1. 프로젝트 구조와의 일치

File-scoped 네임스페이스는 디렉터리 구조와 네임스페이스의 일대일 대응을 강화합니다:

```
MyCompany.Product/
├── Models/
│   ├── Product.cs              // namespace MyCompany.Product.Models;
│   ├── Category.cs             // namespace MyCompany.Product.Models;
│   └── Tag.cs                  // namespace MyCompany.Product.Models;
├── Services/
│   ├── ProductService.cs       // namespace MyCompany.Product.Services;
│   └── CategoryService.cs      // namespace MyCompany.Product.Services;
└── Repositories/
    ├── IProductRepository.cs   // namespace MyCompany.Product.Repositories;
    └── ProductRepository.cs    // namespace MyCompany.Product.Repositories;
```

이러한 구조는 다음 장점을 제공합니다:
- **네비게이션 용이성**: 파일 위치로 네임스페이스 예측 가능
- **자동화 도구 친화적**: 파일 위치 기반 네임스페이스 생성/검증 도구 작성 용이
- **팀 협업**: 일관된 규칙으로 코드 위치 파악 쉬움

### 2. Partial 클래스와의 조합

File-scoped 네임스페이스는 partial 클래스와 함께 사용할 때 특히 유용합니다:

```csharp
// Product.cs
namespace MyCompany.Models;

public partial class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

```csharp
// Product.Validation.cs
namespace MyCompany.Models;

public partial class Product
{
    public bool IsValid()
    {
        return !string.IsNullOrEmpty(Name) && Price > 0;
    }
    
    public IEnumerable<string> GetValidationErrors()
    {
        if (string.IsNullOrEmpty(Name))
            yield return "Name is required";
        if (Price <= 0)
            yield return "Price must be greater than zero";
    }
}
```

```csharp
// Product.Extensions.cs
namespace MyCompany.Models;

public partial class Product
{
    public string GetDisplayName() => $"{Name} (${Price})";
    
    public Product WithDiscount(decimal percentage)
    {
        return new Product
        {
            Id = this.Id,
            Name = this.Name,
            Price = this.Price * (1 - percentage / 100)
        };
    }
}
```

모든 파일이 같은 네임스페이스를 공유하면서도 들여쓰기는 최소화됩니다.

### 3. 생성된 코드(Generated Code)와의 통합

Source Generator나 T4 템플릿으로 생성된 코드도 file-scoped 네임스페이스를 사용할 수 있습니다:

```csharp
// <auto-generated />
// Generated by EntityFramework Core
namespace MyCompany.Data;

public partial class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Product> Products { get; set; }
    public virtual DbSet<Category> Categories { get; set; }
}
```

**마이그레이션 전략과 도구:**

기존 프로젝트를 file-scoped 네임스페이스로 변환하는 방법:

### 1. Visual Studio Code Cleanup 사용

Visual Studio 2022는 자동 변환 기능을 제공합니다:

```
1. 도구 > 옵션 > 텍스트 편집기 > C# > 코드 스타일 > 일반
2. "네임스페이스 선언" 드롭다운에서 "파일 범위" 선택
3. 솔루션 탐색기에서 프로젝트 우클릭 > 코드 정리 > 코드 정리 실행
```

### 2. .editorconfig를 통한 자동화

프로젝트에 `.editorconfig` 파일 추가:

```ini
# .editorconfig
root = true

[*.cs]
csharp_style_namespace_declarations = file_scoped:warning

# 또는 오류로 강제
csharp_style_namespace_declarations = file_scoped:error
```

### 3. Roslyn Analyzer를 통한 검증

CI/CD 파이프라인에서 자동 검증:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
    <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
  </PropertyGroup>
</Project>
```

### 4. 수동 변환 스크립트

PowerShell 스크립트를 사용한 일괄 변환:

```powershell
# Convert-ToFileScopedNamespace.ps1
Get-ChildItem -Path . -Filter *.cs -Recurse | ForEach-Object {
    $content = Get-Content $_.FullName -Raw
    # 정규식으로 네임스페이스 패턴 찾기
    if ($content -match '(?s)namespace\s+([^\s{]+)\s*{(.+)}') {
        $namespace = $Matches[1]
        $body = $Matches[2].Trim()
        
        # 들여쓰기 제거
        $lines = $body -split "`n" | ForEach-Object {
            $_.TrimStart()
        }
        
        $newContent = "namespace $namespace;`n`n" + ($lines -join "`n")
        Set-Content -Path $_.FullName -Value $newContent
    }
}
```

**실무 시나리오와 권장사항:**

### 시나리오 1: 새 프로젝트 시작

```csharp
// ✅ 권장: .NET 6+ 프로젝트는 기본적으로 file-scoped 사용
namespace MyNewProject.Services;

public class UserService
{
    // 구현...
}
```

### 시나리오 2: 레거시 프로젝트 마이그레이션

```csharp
// 단계적 접근 권장:
// 1. 새 파일은 file-scoped 사용
// 2. 수정하는 파일은 변환
// 3. 전체 프로젝트 일괄 변환은 신중히 결정
```

### 시나리오 3: 여러 네임스페이스가 필요한 경우

```csharp
// ❌ File-scoped는 사용 불가, 전통적 방식 유지
namespace MyCompany.Public
{
    public class PublicApi { }
}

namespace MyCompany.Internal
{
    internal class InternalHelper { }
}
```

하지만 이런 경우는 매우 드물며, 파일을 분리하는 것이 더 나은 설계입니다.

---

## 26.3 개선된 람다 식

C# 10은 람다 식의 표현력을 크게 향상시켰습니다. 람다 식에 특성(Attributes)을 적용할 수 있게 되었고, 명시적 반환 타입을 지정할 수 있으며, 자연스러운 타입 추론이 가능해졌습니다. 이는 람다 식을 메서드와 거의 동등한 수준의 일급 시민(First-class citizen)으로 만들어줍니다.

**람다 식의 역사와 진화:**

람다 식은 C# 3.0(2007년)에서 처음 도입되어 LINQ의 핵심 문법이 되었습니다. 1930년대 수학자 Alonzo Church가 개발한 람다 계산법(Lambda Calculus)에서 이름을 따온 이 기능은 함수형 프로그래밍의 개념을 C#에 도입했습니다.

람다 식의 진화 과정:
- **C# 3.0 (2007)**: 기본 람다 식 도입 - `x => x * 2`
- **C# 4.0 (2010)**: 공변성/반공변성 지원으로 람다의 타입 호환성 향상
- **C# 5.0 (2012)**: async 람다 - `async x => await ProcessAsync(x)`
- **C# 6.0 (2015)**: 표현식 본문 멤버와의 통합
- **C# 7.0 (2017)**: 튜플과 해체 지원 - `(x, y) => x + y`
- **C# 9.0 (2020)**: 정적 람다, 함수 포인터와의 상호 운용성
- **C# 10.0 (2021)**: 특성 지원, 명시적 반환 타입, 자연스러운 타입 추론

**기존 람다 식의 한계와 문제점:**

C# 9까지의 람다 식은 다음과 같은 제약이 있었습니다:

1. **특성(Attributes) 부재**: ASP.NET Minimal API 같은 시나리오에서 메타데이터를 표현할 방법이 없었습니다
2. **타입 추론 의존성**: 반환 타입을 명시적으로 지정할 수 없어 복잡한 경우 가독성 저하
3. **델리게이트 강제**: 람다를 변수에 할당하려면 반드시 델리게이트 타입 지정 필요
4. **매개변수 검증 불가**: `[NotNull]`, `[Range]` 같은 검증 특성을 매개변수에 적용할 수 없었습니다

이러한 한계는 특히 ASP.NET Core의 Minimal API 패턴이 등장하면서 심각한 제약으로 작용했습니다. 전통적인 컨트롤러 메서드는 특성을 통해 라우팅, 인증, 검증, OpenAPI 문서화를 처리했지만, 람다 기반 엔드포인트는 이러한 메타데이터를 표현할 방법이 없었습니다.

**C# 10의 혁신적 개선사항:**

### 1. 명시적 반환 타입 지정

람다 식에 명시적으로 반환 타입을 지정할 수 있게 되었습니다. 이는 특히 복잡한 타입이나 제네릭 타입을 반환할 때 가독성을 크게 향상시킵니다.

```csharp
// 기존 방식: 델리게이트 타입으로만 선언 가능
Func<int, int> square = x => x * x;
Func<string> getName = () => "John";

// C# 10: 명시적 반환 타입과 var 결합
var square = int (int x) => x * x;
var getName = string () => "John";
var divide = double (int a, int b) => (double)a / b;

// 복잡한 타입도 명확하게 표현
var createUser = User (string name, int age) => new User { Name = name, Age = age };
var getData = IEnumerable<int> () => Enumerable.Range(1, 100);
```

**이론적 배경:**

명시적 반환 타입은 Hindley-Milner 타입 시스템의 타입 주석(Type Annotation) 개념을 구현합니다. 이는 다음 장점을 제공합니다:

1. **가독성 향상**: 람다의 의도가 타입 시그니처로 명확히 표현됨
2. **컴파일러 최적화**: 타입 추론 부담 감소로 컴파일 속도 향상
3. **오류 메시지 개선**: 타입 불일치 시 더 명확한 오류 메시지
4. **문서화 효과**: 코드 자체가 자기 문서화(Self-documenting)됨

**실전 예제:**

```csharp
// 비동기 람다with 명시적 타입
var fetchData = async Task<List<Product>> (int categoryId) =>
{
    var products = await _repository.GetByCategoryAsync(categoryId);
    return products.Where(p => p.IsActive).ToList();
};

// Nullable 반환 타입 명시
var findUser = User? (int id) =>
{
    return _users.FirstOrDefault(u => u.Id == id);
};

// 복잡한 제네릭 타입
var transform = Dictionary<string, List<int>> (IEnumerable<Product> products) =>
{
    return products
        .GroupBy(p => p.Category)
        .ToDictionary(g => g.Key, g => g.Select(p => p.Id).ToList());
};
```

### 2. 특성(Attributes) 적용 - ASP.NET Minimal API의 혁명

C# 10의 람다 식 특성 지원은 ASP.NET Core Minimal API를 실용적으로 만든 핵심 기능입니다. 이제 람다 기반 엔드포인트도 기존 컨트롤러 메서드와 동등한 메타데이터 표현력을 갖게 되었습니다.

```csharp
// 람다 식 자체에 특성 적용
var lambda = [MyAttribute] (int x) => x * 2;

// 매개변수에 특성 적용
var validated = ([NotNull] string input) => input.ToUpper();

// 반환 값에 특성 적용
var getData = [return: NotNull] () => "data";
```

**ASP.NET Minimal API에서의 실전 활용:**

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using System.ComponentModel.DataAnnotations;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// 인증/인가 특성 적용
app.MapGet("/admin/users", 
    [Authorize(Roles = "Admin")]
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status401Unauthorized)]
    async ([FromServices] IUserRepository repo) =>
    {
        var users = await repo.GetAllAsync();
        return Results.Ok(users);
    });

// 복잡한 검증 특성 조합
app.MapPost("/products",
    [Authorize]
    [ProducesResponseType(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    async (
        [FromBody, Required] ProductCreateRequest request,
        [FromServices] IProductService service,
        [FromServices] IValidator<ProductCreateRequest> validator) =>
    {
        var validationResult = await validator.ValidateAsync(request);
        if (!validationResult.IsValid)
            return Results.BadRequest(validationResult.Errors);
        
        var product = await service.CreateAsync(request);
        return Results.Created($"/products/{product.Id}", product);
    });

// OpenAPI/Swagger 문서화 특성
app.MapPut("/products/{id}",
    [Tags("Products")]
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    [EndpointDescription("Updates an existing product")]
    [EndpointSummary("Update product")]
    async (
        [FromRoute] int id,
        [FromBody] ProductUpdateRequest request,
        [FromServices] IProductService service) =>
    {
        var result = await service.UpdateAsync(id, request);
        return result is not null ? Results.Ok(result) : Results.NotFound();
    })
    .WithName("UpdateProduct")
    .WithTags("Products");
```

**커스텀 특성 정의 및 활용:**

```csharp
// 커스텀 특성 정의
[AttributeUsage(AttributeTargets.Method | AttributeTargets.Parameter)]
public class ValidateIdAttribute : Attribute
{
    public int MinValue { get; set; } = 1;
}

[AttributeUsage(AttributeTargets.Method)]
public class RateLimitAttribute : Attribute
{
    public int RequestsPerMinute { get; set; }
    
    public RateLimitAttribute(int requestsPerMinute)
    {
        RequestsPerMinute = requestsPerMinute;
    }
}

// 사용
app.MapGet("/api/data/{id}",
    [RateLimit(60)]
    [Authorize]
    async ([ValidateId(MinValue = 1)] int id, [FromServices] IDataService service) =>
    {
        return await service.GetByIdAsync(id);
    });
```

### 3. 자연스러운 타입 추론 (Natural Type Inference)

C# 10 이전에는 람다를 변수에 할당할 때 반드시 델리게이트 타입을 명시해야 했습니다. C# 10부터는 컴파일러가 람다의 시그니처를 분석하여 자동으로 적절한 델리게이트 타입을 추론합니다.

```csharp
// C# 9 이전: 명시적 타입 필요
Func<string, int> parse = s => int.Parse(s);
Action<string> print = message => Console.WriteLine(message);

// C# 10: var 사용 가능
var parse = (string s) => int.Parse(s);  // Func<string, int>로 추론
var print = (string message) => Console.WriteLine(message);  // Action<string>으로 추론
var add = (int a, int b) => a + b;  // Func<int, int, int>로 추론
```

**타입 추론의 메커니즘:**

컴파일러는 다음 규칙으로 델리게이트 타입을 추론합니다:

1. **반환 타입 분석**:
   - `void` 반환 → `Action<...>` 델리게이트
   - 값 반환 → `Func<..., TResult>` 델리게이트

2. **매개변수 타입 분석**:
   - 모든 매개변수 타입이 명시되어야 함
   - 타입 생략 시 추론 불가 → 컴파일 오류

3. **특수 케이스**:
   - async 람다 → `Task` 또는 `Task<T>` 반환하는 델리게이트
   - ref/out 매개변수 → 커스텀 델리게이트 타입 필요

```csharp
// ✅ 추론 성공 예제
var multiply = (int x, int y) => x * y;  // Func<int, int, int>
var log = (string msg) => Console.WriteLine(msg);  // Action<string>
var asyncFetch = async (int id) => await FetchDataAsync(id);  // Func<int, Task<Data>>

// ❌ 추론 실패 예제
var ambiguous = x => x * 2;  // 오류: 매개변수 타입 불명확
var tooComplex = (ref int x) => x++;  // 오류: ref는 Func/Action으로 표현 불가

// 해결: 명시적 델리게이트 타입 사용
Func<int, int> ambiguous = x => x * 2;
delegate void RefAction(ref int x);
RefAction tooComplex = (ref int x) => x++;
```

**실전 활용 - 함수형 프로그래밍 패턴:**

```csharp
// 고차 함수(Higher-order Function) 구현
var compose = <T1, T2, T3>(Func<T1, T2> f, Func<T2, T3> g) => 
    (T1 x) => g(f(x));

var addOne = (int x) => x + 1;
var double = (int x) => x * 2;

var addOneThenDouble = compose(addOne, double);
Console.WriteLine(addOneThenDouble(5));  // 12

// 커링(Currying) 패턴
var curriedAdd = (int x) => (int y) => x + y;
var add5 = curriedAdd(5);
Console.WriteLine(add5(10));  // 15

// 파이프라인 구축
var pipeline = new List<Func<string, string>>
{
    s => s.Trim(),
    s => s.ToLower(),
    s => s.Replace(" ", "-")
};

var process = (string input) =>
{
    var result = input;
    foreach (var step in pipeline)
        result = step(result);
    return result;
};

Console.WriteLine(process("  Hello World  "));  // "hello-world"
```

**성능 고려사항과 최적화:**

람다 식의 성능 특성을 이해하는 것은 중요합니다:

```csharp
// 1. 캡처(Closure)가 없는 람다 - 최적 성능
var simple = (int x) => x * 2;
// 컴파일러가 정적 메서드로 변환 → 오버헤드 최소

// 2. 지역 변수 캡처 - 클로저 생성
int factor = 2;
var withClosure = (int x) => x * factor;
// 클로저 객체 생성 필요 → 약간의 메모리/성능 오버헤드

// 3. LINQ에서의 람다 - 반복 평가 주의
var numbers = Enumerable.Range(1, 1000);
var threshold = 100;

// ❌ 비효율: 람다가 매번 평가됨
var filtered = numbers.Where(x => x > threshold);

// ✅ 효율적: 지역 변수 캡처 한 번만
var localThreshold = threshold;
var filtered2 = numbers.Where(x => x > localThreshold);

// 또는 메서드 그룹 사용 (가장 효율적)
bool IsAboveThreshold(int x) => x > threshold;
var filtered3 = numbers.Where(IsAboveThreshold);
```

**람다 식 vs 로컬 함수 - 선택 가이드:**

```csharp
public class Calculator
{
    // 람다 식: 델리게이트로 저장/전달 필요 시
    private Func<int, int, int> _operation;
    
    public Calculator(Func<int, int, int> operation)
    {
        _operation = operation;
    }
    
    // 로컬 함수: 헬퍼 로직, 재귀, 제네릭 필요 시
    public int Calculate(int[] numbers, Func<int, int, int> operation)
    {
        // 로컬 함수 - 더 나은 성능, 제네릭 지원
        int ReduceRecursive<T>(T[] array, Func<T, T, T> op, int index, T accumulator)
        {
            if (index >= array.Length) return accumulator;
            return ReduceRecursive(array, op, index + 1, op(accumulator, array[index]));
        }
        
        return numbers.Length > 0 
            ? ReduceRecursive(numbers, operation, 1, numbers[0])
            : 0;
    }
}
```

**디버깅과 진단:**

람다 식 디버깅을 위한 팁:

```csharp
// 1. 명시적 반환 타입으로 타입 오류 조기 발견
var process = int (string input) =>  // 컴파일 오류를 더 명확하게 표시
{
    return int.Parse(input);  // 예외 발생 가능 지점 명확
};

// 2. 람다를 여러 줄로 분리하여 디버깅
var complex = (int x) =>
{
    var step1 = x * 2;      // 중단점 설정 가능
    var step2 = step1 + 10; // 변수 검사 가능
    return step2;           // 반환 값 확인 가능
};

// 3. 표현식 람다보다 블록 람다 선호 (디버깅 시)
var expr = (int x) => x * 2 + 10;  // 한 줄 - 중단점 하나만
var block = (int x) =>              // 여러 줄 - 중단점 여러 개
{
    var doubled = x * 2;
    return doubled + 10;
};
```

---

## 26.4 Record struct

Record struct는 C# 10에서 도입된 기능으로, C# 9의 record 타입(참조 타입)을 값 타입으로 확장한 것입니다. 불변(Immutable) 값 타입을 우아하게 표현할 수 있으며, 값 기반 동등성(Value-based equality)과 with 식을 지원합니다. 이는 함수형 프로그래밍의 불변 데이터 구조와 .NET의 성능 최적화를 결합한 혁신적인 기능입니다.

**값 타입과 참조 타입의 근본적 차이:**

C#의 타입 시스템은 CLR(Common Language Runtime)의 메모리 모델을 반영합니다:

| 측면 | 값 타입 (Value Type) | 참조 타입 (Reference Type) |
|------|---------------------|---------------------------|
| **할당 위치** | 스택 (Stack) 또는 인라인 | 힙 (Heap) |
| **전달 방식** | 값 복사 (Copy by value) | 참조 복사 (Copy by reference) |
| **기본 동등성** | 비트 단위 비교 | 참조 비교 (주소) |
| **GC 부담** | 없음 (스택 자동 해제) | GC가 추적 및 수집 |
| **null 허용** | Nullable<T>로만 가능 | 기본적으로 가능 |
| **상속** | 불가능 | 가능 |
| **성능** | 작은 크기: 빠름, 큰 크기: 느림 | 일관된 성능 |

**Record의 진화와 설계 철학:**

Record 타입의 도입 배경은 함수형 프로그래밍과 도메인 주도 설계(DDD)의 영향을 받았습니다:

- **C# 9 (2020)**: `record` (record class) 도입
  - Eric Evans의 DDD에서 제시한 **값 객체(Value Object)** 패턴 구현
  - 불변성(Immutability)과 구조적 동등성(Structural Equality)
  - with 식을 통한 비파괴적 변경(Non-destructive Mutation)

- **C# 10 (2021)**: `record struct` 도입
  - 값 타입의 성능 이점 + record의 표현력
  - 작은 불변 데이터 구조의 완벽한 표현
  - 좌표, 색상, 범위 등 "값"의 의미를 갖는 타입에 최적

**기본 record struct와 자동 생성 멤버:**

```csharp
// 간단한 record struct 정의
public record struct Point(int X, int Y);

// 컴파일러가 자동으로 생성하는 것:
// 1. 주 생성자 (Primary Constructor)
// 2. 프로퍼티 (init-only by default)
// 3. Deconstruct 메서드
// 4. ToString 오버라이드
// 5. Equals 오버라이드 (값 기반)
// 6. GetHashCode 오버라이드
// 7. == 및 != 연산자
// 8. EqualityContract (타입 비교)

// 사용
var p1 = new Point(10, 20);
var p2 = new Point(10, 20);

Console.WriteLine(p1 == p2);  // True (값 기반 동등성)
Console.WriteLine(p1);  // Point { X = 10, Y = 20 } (자동 ToString)

// Deconstruction
var (x, y) = p1;
Console.WriteLine($"X: {x}, Y: {y}");  // X: 10, Y: 20
```

**일반 struct vs Record struct - 보일러플레이트 비교:**

일반 struct로 동일한 기능을 구현하려면 상당한 코드가 필요합니다:

```csharp
// 일반 struct - 약 30줄의 보일러플레이트 코드
public struct PointStruct : IEquatable<PointStruct>
{
    public int X { get; init; }
    public int Y { get; init; }
    
    public PointStruct(int x, int y)
    {
        X = x;
        Y = y;
    }
    
    // 값 기반 동등성 구현
    public bool Equals(PointStruct other)
    {
        return X == other.X && Y == other.Y;
    }
    
    public override bool Equals(object? obj)
    {
        return obj is PointStruct other && Equals(other);
    }
    
    public override int GetHashCode()
    {
        return HashCode.Combine(X, Y);
    }
    
    public override string ToString()
    {
        return $"PointStruct {{ X = {X}, Y = {Y} }}";
    }
    
    public void Deconstruct(out int x, out int y)
    {
        x = X;
        y = Y;
    }
    
    public static bool operator ==(PointStruct left, PointStruct right)
    {
        return left.Equals(right);
    }
    
    public static bool operator !=(PointStruct left, PointStruct right)
    {
        return !left.Equals(right);
    }
}

// Record struct - 단 1줄!
public record struct Point(int X, int Y);

// 동일한 기능, 30배 간결함
```

**메모리 레이아웃과 성능 특성:**

Record struct의 메모리 효율성을 이해하는 것은 중요합니다:

```csharp
public record struct Point3D(float X, float Y, float Z);  // 12 bytes
public record struct Color(byte R, byte G, byte B, byte A);  // 4 bytes
public record struct Guid(long A, long B);  // 16 bytes

// 스택 할당 예제
void ProcessPoints()
{
    var points = stackalloc Point3D[1000];  // 12KB 스택 할당 - 매우 빠름!
    // GC 부담 전혀 없음
    
    for (int i = 0; i < 1000; i++)
    {
        points[i] = new Point3D(i, i * 2, i * 3);
    }
}

// 참조 타입과 비교
void ProcessPointsClass()
{
    var points = new List<PointClass>(1000);  // 힙 할당
    // 1000개의 객체 = 1000번의 힙 할당 = GC 부담 증가
    
    for (int i = 0; i < 1000; i++)
    {
        points.Add(new PointClass(i, i * 2, i * 3));
    }
}

// 성능 비교 (1백만 번 반복):
// Record struct:  ~10ms, GC: 0 수집
// Record class:   ~50ms, GC: Gen0 50회, Gen1 10회
```

**크기 가이드라인:**

- **16 bytes 이하**: Record struct 강력 추천
- **17-24 bytes**: 상황에 따라 판단 (`in` 파라미터 사용 고려)
- **25+ bytes**: Record class 고려 (복사 비용 > GC 비용)

**실전 예제:**

```csharp
// 좌표계 record struct
public record struct Coordinate(double Latitude, double Longitude)
{
    // 추가 메서드 정의 가능
    public double DistanceTo(Coordinate other)
    {
        var lat = Latitude - other.Latitude;
        var lon = Longitude - other.Longitude;
        return Math.Sqrt(lat * lat + lon * lon);
    }
}

// 사용
var seoul = new Coordinate(37.5665, 126.9780);
var busan = new Coordinate(35.1796, 129.0756);
var distance = seoul.DistanceTo(busan);
Console.WriteLine($"Distance: {distance}");
```

```csharp
// RGB 색상 표현
public record struct Color(byte Red, byte Green, byte Blue)
{
    public static Color White => new(255, 255, 255);
    public static Color Black => new(0, 0, 0);
    
    public Color Darken(double factor)
    {
        return new Color(
            (byte)(Red * factor),
            (byte)(Green * factor),
            (byte)(Blue * factor)
        );
    }
}

// 사용
var red = new Color(255, 0, 0);
var darkRed = red.Darken(0.5);
Console.WriteLine(darkRed);  // Color { Red = 127, Green = 0, Blue = 0 }
```

**with 식을 사용한 비파괴적 변경:**

```csharp
public record struct Product(int Id, string Name, decimal Price);

var product1 = new Product(1, "Laptop", 1000m);

// 일부 속성만 변경하여 새 인스턴스 생성
var product2 = product1 with { Price = 900m };

Console.WriteLine(product1);  // Product { Id = 1, Name = Laptop, Price = 1000 }
Console.WriteLine(product2);  // Product { Id = 1, Name = Laptop, Price = 900 }
```

**readonly record struct:**

```csharp
// 완전히 불변인 record struct
public readonly record struct ImmutablePoint(int X, int Y);

// 컴파일러가 모든 속성을 readonly로 만듦
var point = new ImmutablePoint(10, 20);
// point.X = 30;  // 컴파일 오류!
```

**Mutable record struct:**

```csharp
// 가변 record struct (권장하지 않음)
public record struct MutablePoint(int X, int Y);

var point = new MutablePoint(10, 20);
point.X = 30;  // 가능하지만 record의 철학과 맞지 않음
```

**사용 시기:**

1. **Record struct 사용**: 작은 불변 값 타입, 좌표, 색상, 날짜 범위 등
2. **Record class 사용**: 도메인 엔티티, 대용량 데이터
3. **Class 사용**: 가변 상태, 다형성이 필요한 경우
4. **Struct 사용**: 성능이 매우 중요하고 값 의미론이 필요한 경우

**성능 고려사항:**

Record struct는 값 타입이므로:
- 스택에 할당되어 가비지 컬렉션 부담이 적음
- 작은 크기(16바이트 이하)일 때 성능상 이점
- 큰 크기의 경우 복사 비용이 높아질 수 있으므로 `in` 매개변수 사용 고려

```csharp
// 큰 record struct를 효율적으로 전달
public record struct LargeData(string A, string B, string C, string D);

public void ProcessData(in LargeData data)  // 복사 없이 참조로 전달
{
    Console.WriteLine(data.A);
}
```

---

## 26.5 필수 속성 (Required Properties)

C# 11에서 정식 도입되었지만 개념적으로 C# 10의 흐름을 이어받은 Required Properties는 객체 초기화 시 반드시 설정해야 하는 속성을 컴파일 타임에 검증할 수 있게 합니다. 이는 NullReferenceException을 방지하고 객체의 유효성을 보장하는 강력한 도구입니다.

**기존 방식의 문제점:**

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int Age { get; set; }
}

// 문제: 필수 속성을 설정하지 않아도 컴파일됨
var person = new Person();  // FirstName과 LastName이 null!
Console.WriteLine(person.FirstName.Length);  // NullReferenceException!
```

**생성자를 통한 해결 (전통적 방식):**

```csharp
public class Person
{
    public Person(string firstName, string lastName, int age)
    {
        FirstName = firstName;
        LastName = lastName;
        Age = age;
    }
    
    public string FirstName { get; }
    public string LastName { get; }
    public int Age { get; }
}

// 모든 값을 생성자에 전달해야 함
var person = new Person("John", "Doe", 30);
```

하지만 이 방식은 속성이 많을 때 불편하고, 선택적 속성을 다루기 어렵습니다.

**Required Properties의 해결책:**

```csharp
public class Person
{
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
    public int Age { get; set; }  // 선택적
}

// 컴파일 타임에 필수 속성 검증
var person = new Person
{
    FirstName = "John",
    LastName = "Doe"
    // Age는 선택적이므로 생략 가능
};

// ❌ 컴파일 오류: FirstName이 설정되지 않음
// var invalid = new Person { LastName = "Doe" };
```

**실전 예제:**

```csharp
// 제품 클래스
public class Product
{
    public required int Id { get; set; }
    public required string Name { get; set; }
    public required decimal Price { get; set; }
    
    // 선택적 속성
    public string? Description { get; set; }
    public DateTime? CreatedAt { get; set; }
    public List<string> Tags { get; set; } = new();
}

// 사용
var product = new Product
{
    Id = 1,
    Name = "Laptop",
    Price = 1299.99m,
    Description = "High-performance laptop",  // 선택적
    Tags = { "electronics", "computer" }
};
```

```csharp
// 구성 클래스
public class DatabaseConfig
{
    public required string ConnectionString { get; init; }
    public required string DatabaseName { get; init; }
    public int MaxPoolSize { get; init; } = 100;
    public int TimeoutSeconds { get; init; } = 30;
}

// 사용 - 필수 값만 설정
var config = new DatabaseConfig
{
    ConnectionString = "Server=localhost;",
    DatabaseName = "MyDatabase"
    // MaxPoolSize와 TimeoutSeconds는 기본값 사용
};
```

**Required와 init 결합:**

```csharp
public class ImmutablePerson
{
    public required string FirstName { get; init; }
    public required string LastName { get; init; }
    public int Age { get; init; }
}

// 초기화 시에만 설정 가능
var person = new ImmutablePerson
{
    FirstName = "John",
    LastName = "Doe",
    Age = 30
};

// person.FirstName = "Jane";  // 컴파일 오류! init만 가능
```

**생성자와 함께 사용:**

```csharp
public class Employee
{
    public Employee()
    {
        // 기본 생성자
    }
    
    // SetsRequiredMembers 특성: 이 생성자가 필수 멤버를 설정함을 표시
    [SetsRequiredMembers]
    public Employee(string firstName, string lastName, string employeeId)
    {
        FirstName = firstName;
        LastName = lastName;
        EmployeeId = employeeId;
    }
    
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
    public required string EmployeeId { get; set; }
}

// 두 가지 방식 모두 가능
var emp1 = new Employee
{
    FirstName = "John",
    LastName = "Doe",
    EmployeeId = "E001"
};

var emp2 = new Employee("Jane", "Smith", "E002");
```

**상속 시나리오:**

```csharp
public class Animal
{
    public required string Name { get; set; }
    public int Age { get; set; }
}

public class Dog : Animal
{
    public required string Breed { get; set; }
    public string? FavoriteActivity { get; set; }
}

// 기본 클래스와 파생 클래스의 필수 속성 모두 설정
var dog = new Dog
{
    Name = "Buddy",      // Animal의 필수 속성
    Breed = "Labrador",  // Dog의 필수 속성
    Age = 3,             // 선택적
    FavoriteActivity = "Fetching"  // 선택적
};
```

**모범 사례:**

1. **필수 속성 식별**: 객체가 유효하려면 반드시 필요한 속성에만 `required` 사용
2. **기본값이 있는 속성은 선택적으로**: 의미 있는 기본값이 있다면 required 불필요
3. **null 허용 타입과 구분**: `required string`은 null이 아닌 값이 필요, `string?`은 null 허용
4. **문서화 효과**: `required` 키워드 자체가 API 문서 역할을 함

**Required Properties의 장점:**

1. **컴파일 타임 검증**: 런타임 오류 대신 컴파일 시점에 발견
2. **유연성**: 생성자보다 더 유연한 객체 초기화
3. **가독성**: 객체 초기화 구문이 명확하고 읽기 쉬움
4. **유지보수성**: 필수 속성 변경 시 컴파일러가 모든 사용처를 확인

---


---

# 제2부: C# 11 주요 기능

## 26.6 Raw String Literals (원시 문자열 리터럴)

Raw string literals는 C# 11의 가장 눈에 띄는 기능 중 하나로, 복잡한 문자열(JSON, XML, SQL, 정규식 등)을 이스케이프 없이 직접 작성할 수 있게 합니다.

**문제 상황 - 전통적 접근의 한계:**

기존 C#에서 여러 줄의 JSON이나 SQL을 문자열로 표현하려면 이스케이프 문자나 verbatim string(`@`)을 사용해야 했습니다:

```csharp
// 전통적인 방식 - 이스케이프 필요
string json = "{\"name\":\"홍길동\",\"age\":30}";

// Verbatim string - 여러 줄 가능하지만 따옴표 이스케이프 필요
string sql = @"
SELECT *
FROM Users
WHERE Name = ""홍길동""
";
```

**Raw String Literals의 해결책:**

Raw string literals는 최소 3개의 따옴표(`"""`)로 시작하고 끝나며, 내부의 모든 문자를 그대로 표현합니다:

```csharp
// Raw string literal - 이스케이프 불필요
string json = """
{
    "name": "홍길동",
    "age": 30,
    "address": "서울시 강남구"
}
""";

// SQL 쿼리도 간단하게
string sql = """
SELECT *
FROM Users
WHERE Name = '홍길동'
    AND Age > 20
""";

// 정규식도 명확하게
string regex = """^\d{3}-\d{4}-\d{4}$""";
```

**문자열 보간과 결합:**

Raw string literals는 문자열 보간과도 함께 사용할 수 있습니다:

```csharp
string name = "홍길동";
int age = 30;

string json = $$"""
{
    "name": "{{name}}",
    "age": {{age}},
    "timestamp": "{{DateTime.Now:yyyy-MM-dd}}"
}
""";

Console.WriteLine(json);
```

**주의사항:**
- `$` 기호의 개수만큼 중괄호를 사용해야 합니다 (`$$"""` → `{{}}`)
- 여러 줄 문자열의 들여쓰기는 닫는 따옴표의 위치를 기준으로 결정됩니다

**실전 예제 - API 응답 생성:**

```csharp
// API 응답 JSON 생성
string CreateApiResponse(string status, string message, object data)
{
    string dataJson = System.Text.Json.JsonSerializer.Serialize(data);
    
    return $$"""
    {
        "status": "{{status}}",
        "message": "{{message}}",
        "data": {{dataJson}},
        "timestamp": "{{DateTime.UtcNow:O}}"
    }
    """;
}

var response = CreateApiResponse("success", "데이터 조회 완료", new { id = 1, name = "테스트" });
Console.WriteLine(response);
```

---

## 26.7 Generic Attributes (제네릭 특성)

C# 11 이전에는 특성(Attributes)에 제네릭 타입 매개변수를 직접 사용할 수 없었습니다. C# 11에서는 제네릭 타입을 사용하는 특성을 정의할 수 있게 되었습니다.

**문제 상황:**

```csharp
// C# 10 이전 - 타입 정보를 Type으로 전달해야 함
public class TypedAttribute : Attribute
{
    public TypedAttribute(Type type)
    {
        Type = type;
    }
    
    public Type Type { get; }
}

// 사용 - 타입 안전하지 않음
[Typed(typeof(string))]
public class MyClass { }
```

**Generic Attributes 해결책:**

```csharp
// C# 11 - 제네릭 특성
public class TypedAttribute<T> : Attribute
{
    public Type Type => typeof(T);
}

// 사용 - 타입 안전
[Typed<string>]
public class MyClass { }

[Typed<int>]
public class MyOtherClass { }
```

**실전 예제 - 의존성 주입:**

```csharp
// 서비스 등록 특성
public class RegisterServiceAttribute<TService, TImplementation> : Attribute
    where TImplementation : TService
{
    public Type ServiceType => typeof(TService);
    public Type ImplementationType => typeof(TImplementation);
}

// 사용
public interface IUserService
{
    User GetUser(int id);
}

[RegisterService<IUserService, UserService>]
public class UserService : IUserService
{
    public User GetUser(int id)
    {
        // 구현
        return new User { Id = id, Name = "사용자" };
    }
}

public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

---

## 26.8 List Patterns (리스트 패턴)

List patterns는 배열이나 리스트의 요소에 대해 패턴 매칭을 수행할 수 있게 해줍니다. 이는 함수형 프로그래밍 언어의 패턴 매칭과 유사한 기능입니다.

**기본 문법:**

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };

// 첫 번째와 마지막 요소 매칭
if (numbers is [var first, .., var last])
{
    Console.WriteLine($"첫 번째: {first}, 마지막: {last}");
    // 출력: 첫 번째: 1, 마지막: 5
}

// 특정 패턴 매칭
if (numbers is [1, 2, ..])
{
    Console.WriteLine("1, 2로 시작하는 배열");
}

// 정확한 길이 매칭
if (numbers is [_, _, _])
{
    Console.WriteLine("정확히 3개의 요소");
}
else if (numbers is [_, _, _, _, _])
{
    Console.WriteLine("정확히 5개의 요소");
}
```

**실전 예제 - 명령어 파싱:**

```csharp
void ProcessCommand(string[] args)
{
    switch (args)
    {
        case ["help"]:
            ShowHelp();
            break;
            
        case ["create", var name]:
            CreateItem(name);
            break;
            
        case ["delete", var id]:
            DeleteItem(id);
            break;
            
        case ["update", var id, .. var properties]:
            UpdateItem(id, properties);
            break;
            
        case ["list", .. var options]:
            ListItems(options);
            break;
            
        default:
            Console.WriteLine("알 수 없는 명령어");
            break;
    }
}

void ShowHelp() => Console.WriteLine("도움말을 표시합니다.");
void CreateItem(string name) => Console.WriteLine($"'{name}' 생성");
void DeleteItem(string id) => Console.WriteLine($"ID {id} 삭제");
void UpdateItem(string id, string[] properties) 
    => Console.WriteLine($"ID {id} 업데이트: {string.Join(", ", properties)}");
void ListItems(string[] options) 
    => Console.WriteLine($"목록 표시 (옵션: {string.Join(", ", options)})");

// 사용 예
ProcessCommand(new[] { "help" });
ProcessCommand(new[] { "create", "새 항목" });
ProcessCommand(new[] { "update", "123", "name=홍길동", "age=30" });
```

**슬라이스 패턴:**

```csharp
int[] scores = { 95, 87, 92, 78, 88, 91 };

// 첫 3개와 나머지 분리
if (scores is [var s1, var s2, var s3, .. var rest])
{
    Console.WriteLine($"상위 3개: {s1}, {s2}, {s3}");
    Console.WriteLine($"나머지: {string.Join(", ", rest)}");
}

// 중간 요소 건너뛰기
if (scores is [var first, .., var last])
{
    Console.WriteLine($"첫 점수: {first}, 마지막 점수: {last}");
}
```

---

## 26.9 Required Members (필수 멤버)

Required members는 객체 초기화 시 반드시 설정해야 하는 속성이나 필드를 표시할 수 있게 합니다. 이는 컴파일 타임에 검증되어 런타임 오류를 방지합니다.

**문제 상황:**

```csharp
// 기존 방식 - 생성자 오버로드 또는 런타임 검증
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public Person(string name, int age)
    {
        Name = name ?? throw new ArgumentNullException(nameof(name));
        Age = age;
    }
}

// 또는 초기화 후 검증
public class PersonV2
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public void Validate()
    {
        if (string.IsNullOrEmpty(Name))
            throw new InvalidOperationException("Name은 필수입니다.");
    }
}
```

**Required Members 해결책:**

```csharp
public class Person
{
    public required string Name { get; set; }
    public required int Age { get; set; }
    public string? Email { get; set; }  // 선택적
}

// 컴파일 성공
var person1 = new Person { Name = "홍길동", Age = 30 };

// 컴파일 오류! Name이 설정되지 않음
// var person2 = new Person { Age = 30 };
```

**생성자와 함께 사용:**

```csharp
public class Product
{
    public required string Name { get; set; }
    public required decimal Price { get; set; }
    public string Description { get; set; }
    
    public Product()
    {
        Description = "설명 없음";
    }
}

// Name과 Price는 반드시 설정해야 함
var product = new Product 
{ 
    Name = "노트북", 
    Price = 1500000 
};
```

**SetsRequiredMembers 특성:**

생성자에서 required 멤버를 모두 설정하는 경우, `SetsRequiredMembers` 특성을 사용하여 호출자가 다시 설정하지 않아도 되게 할 수 있습니다:

```csharp
public class User
{
    public required string Username { get; set; }
    public required string Email { get; set; }
    
    [SetsRequiredMembers]
    public User(string username, string email)
    {
        Username = username;
        Email = email;
    }
}

// 생성자 사용 시 required 멤버 설정 불필요
var user = new User("hong", "hong@example.com");
```

---

## 26.10 File-local Types (파일 범위 타입)

`file` 접근 제한자를 사용하여 타입의 가시성을 현재 파일로만 제한할 수 있습니다. 이는 구현 세부사항을 숨기고 네임스페이스 오염을 방지하는 데 유용합니다.

**기본 사용법:**

```csharp
// MyClass.cs
file class Helper
{
    public static string Format(string text)
    {
        return text.ToUpper();
    }
}

public class MyClass
{
    public string Process(string input)
    {
        return Helper.Format(input);  // 같은 파일 내에서만 접근 가능
    }
}
```

**실전 예제 - 파일별 확장 메서드:**

```csharp
// StringExtensions.cs
file static class StringExtensions
{
    public static bool IsValidEmail(this string email)
    {
        return email.Contains("@") && email.Contains(".");
    }
}

public class EmailValidator
{
    public bool Validate(string email)
    {
        // 이 파일에서만 IsValidEmail 확장 메서드 사용 가능
        return email.IsValidEmail();
    }
}
```

---

## 26.11 기타 C# 11 개선사항

### Generic Math Support (제네릭 수학 지원)

제네릭 코드에서 수학 연산을 수행할 수 있게 하는 인터페이스가 추가되었습니다:

```csharp
using System.Numerics;

// 제네릭 수학 함수
T Add<T>(T left, T right) where T : INumber<T>
{
    return left + right;
}

// 다양한 숫자 타입에서 작동
Console.WriteLine(Add(1, 2));           // int
Console.WriteLine(Add(1.5, 2.3));       // double
Console.WriteLine(Add(1.5m, 2.3m));     // decimal
```

### Unsigned Right-shift Operator (부호 없는 오른쪽 시프트 연산자)

`>>>` 연산자를 사용하여 부호 없는 오른쪽 시프트를 수행할 수 있습니다:

```csharp
int value = -8;  // 11111111111111111111111111111000 (2의 보수)

// 기존 >> 연산자 - 부호 비트 유지
int signed = value >> 2;    // -2

// 새로운 >>> 연산자 - 부호 비트를 0으로 채움
int unsigned = value >>> 2;  // 1073741822

Console.WriteLine($"Signed: {signed}, Unsigned: {unsigned}");
```

### Extended nameof Scope (확장된 nameof 범위)

메서드 매개변수 내에서 `nameof`를 사용할 수 있게 되었습니다:

```csharp
public void Process(string input, 
    [CallerArgumentExpression(nameof(input))] string inputExpression = null)
{
    Console.WriteLine($"처리 중: {inputExpression}");
}

var data = "테스트";
Process(data);  // 출력: 처리 중: data
```

### Auto-default Structs (자동 기본값 구조체)

구조체의 생성자에서 모든 필드를 초기화하지 않아도 자동으로 기본값으로 설정됩니다:

```csharp
public struct Point
{
    public double X;
    public double Y;
    
    // Y를 초기화하지 않아도 자동으로 0으로 설정됨
    public Point(double x)
    {
        X = x;
        // Y는 자동으로 0.0
    }
}

var point = new Point(10);
Console.WriteLine($"X: {point.X}, Y: {point.Y}");  // X: 10, Y: 0
```

---

## 26.12 실전 종합 예제

다음은 C# 11의 여러 기능을 활용한 종합 예제입니다:

```csharp
// 사용자 관리 시스템
public class UserManager
{
    public required List<User> Users { get; init; }
    
    public string ExportToJson()
    {
        // Raw string literals 사용
        var users = Users.Select(u => $$"""
            {
                "id": {{u.Id}},
                "name": "{{u.Name}}",
                "email": "{{u.Email}}"
            }
            """);
        
        return $$"""
        {
            "users": [
                {{string.Join(",\n", users)}}
            ],
            "count": {{Users.Count}}
        }
        """;
    }
    
    public void ProcessCommand(string[] args)
    {
        // List patterns 사용
        switch (args)
        {
            case ["add", var name, var email]:
                AddUser(name, email);
                break;
                
            case ["find", var query, .. var filters]:
                FindUsers(query, filters);
                break;
                
            case ["delete", var id]:
                DeleteUser(int.Parse(id));
                break;
                
            default:
                Console.WriteLine("알 수 없는 명령어");
                break;
        }
    }
    
    private void AddUser(string name, string email)
    {
        var user = new User { Id = Users.Count + 1, Name = name, Email = email };
        Users.Add(user);
        Console.WriteLine($"사용자 추가됨: {user.Name}");
    }
    
    private void FindUsers(string query, string[] filters)
    {
        Console.WriteLine($"'{query}' 검색 중... (필터: {string.Join(", ", filters)})");
    }
    
    private void DeleteUser(int id)
    {
        Users.RemoveAll(u => u.Id == id);
        Console.WriteLine($"사용자 {id} 삭제됨");
    }
}

public class User
{
    public required int Id { get; init; }
    public required string Name { get; set; }
    public required string Email { get; set; }
}

// 제네릭 특성 사용
[Validated<User>]
public class UserValidator
{
    public static bool Validate(User user)
    {
        return !string.IsNullOrEmpty(user.Name) && 
               !string.IsNullOrEmpty(user.Email);
    }
}

public class ValidatedAttribute<T> : Attribute
{
    public Type ValidatedType => typeof(T);
}

// 사용 예제
var manager = new UserManager 
{ 
    Users = new List<User>()
};

manager.ProcessCommand(new[] { "add", "홍길동", "hong@example.com" });
manager.ProcessCommand(new[] { "add", "김철수", "kim@example.com" });
manager.ProcessCommand(new[] { "find", "홍", "active", "verified" });

Console.WriteLine(manager.ExportToJson());
```

---


---

# 제3부: C# 12 주요 기능

## 26.13 Primary Constructors (주 생성자)

Primary constructors는 C# 12의 가장 주목할 만한 기능으로, 클래스나 구조체의 선언부에 직접 생성자 매개변수를 정의할 수 있게 합니다. 이는 record 타입에서만 가능했던 기능을 일반 클래스와 구조체로 확장한 것입니다.

**문제 상황 - 전통적 접근의 한계:**

기존에는 생성자 매개변수를 필드에 할당하는 보일러플레이트 코드가 많이 필요했습니다:

```csharp
// 전통적인 방식
public class Person
{
    private readonly string _firstName;
    private readonly string _lastName;
    
    public Person(string firstName, string lastName)
    {
        _firstName = firstName;
        _lastName = lastName;
    }
    
    public string FullName => $"{_firstName} {_lastName}";
}
```

**Primary Constructors의 해결책:**

```csharp
// C# 12 - 주 생성자
public class Person(string firstName, string lastName)
{
    public string FullName => $"{firstName} {lastName}";
    
    public void Introduce()
    {
        Console.WriteLine($"안녕하세요, {firstName} {lastName}입니다.");
    }
}

// 사용
var person = new Person("홍", "길동");
Console.WriteLine(person.FullName);  // 홍 길동
person.Introduce();  // 안녕하세요, 홍 길동입니다.
```

**의존성 주입과 함께 사용:**

```csharp
// 서비스 클래스
public class UserService(IUserRepository repository, ILogger logger)
{
    public User GetUser(int id)
    {
        logger.LogInformation($"사용자 {id} 조회 중");
        return repository.GetById(id);
    }
    
    public void SaveUser(User user)
    {
        logger.LogInformation($"사용자 {user.Id} 저장 중");
        repository.Save(user);
    }
}

// 인터페이스와 모의 구현
public interface IUserRepository
{
    User GetById(int id);
    void Save(User user);
}

public interface ILogger
{
    void LogInformation(string message);
}
```

**속성과 함께 사용:**

```csharp
public class Product(string name, decimal price)
{
    // 주 생성자 매개변수를 속성으로 노출
    public string Name { get; } = name;
    public decimal Price { get; set; } = price;
    
    // 추가 속성
    public decimal Discount { get; set; }
    
    public decimal FinalPrice => Price * (1 - Discount);
}

var product = new Product("노트북", 1500000)
{
    Discount = 0.1m  // 10% 할인
};

Console.WriteLine($"{product.Name}: {product.FinalPrice:C}");
```

**주의사항:**
- 주 생성자 매개변수는 클래스 전체에서 사용 가능합니다.
- 명시적으로 필드나 속성으로 저장하지 않으면 캡처되어 보관됩니다.
- record 타입과 달리 자동으로 속성이 생성되지 않습니다.

---

## 26.14 Collection Expressions (컬렉션 식)

Collection expressions는 배열, 리스트, 스팬 등 모든 컬렉션 타입을 통일된 문법으로 초기화할 수 있게 하는 혁신적인 기능입니다.

**기본 문법:**

```csharp
// 배열
int[] array = [1, 2, 3, 4, 5];

// 리스트
List<int> list = [1, 2, 3, 4, 5];

// Span
Span<int> span = [1, 2, 3, 4, 5];

// ImmutableArray
ImmutableArray<int> immutable = [1, 2, 3, 4, 5];

// 모두 동일한 문법!
```

**스프레드 연산자 (..):**

기존 컬렉션을 펼쳐서 새로운 컬렉션을 만들 수 있습니다:

```csharp
int[] numbers1 = [1, 2, 3];
int[] numbers2 = [4, 5, 6];

// 두 배열 결합
int[] combined = [.. numbers1, .. numbers2];
// 결과: [1, 2, 3, 4, 5, 6]

// 중간에 요소 추가
int[] extended = [0, .. numbers1, 99, .. numbers2, 100];
// 결과: [0, 1, 2, 3, 99, 4, 5, 6, 100]
```

**실전 예제 - 데이터 변환:**

```csharp
// 여러 소스에서 데이터 수집
List<string> GetAllNames(User[] users, Admin[] admins, Guest[] guests)
{
    return [
        .. users.Select(u => u.Name),
        .. admins.Select(a => a.Name),
        .. guests.Select(g => g.Name)
    ];
}

// 필터링과 결합
List<int> GetValidScores(int[] scores1, int[] scores2)
{
    return [
        .. scores1.Where(s => s >= 0),
        .. scores2.Where(s => s >= 0)
    ];
}

// 사용
var users = new[] 
{ 
    new User { Name = "홍길동" }, 
    new User { Name = "김철수" } 
};
var admins = new[] 
{ 
    new Admin { Name = "관리자1" } 
};
var guests = new[] 
{ 
    new Guest { Name = "게스트1" } 
};

var allNames = GetAllNames(users, admins, guests);
Console.WriteLine(string.Join(", ", allNames));
```

**빈 컬렉션:**

```csharp
// 빈 컬렉션 생성
List<int> empty = [];
int[] emptyArray = [];

// 조건부 컬렉션
List<int> GetNumbers(bool includeNegative)
{
    return includeNegative 
        ? [-1, 0, 1, 2, 3]
        : [0, 1, 2, 3];
}
```

**메서드 매개변수:**

```csharp
void PrintNumbers(List<int> numbers)
{
    Console.WriteLine(string.Join(", ", numbers));
}

// 직접 컬렉션 식 전달
PrintNumbers([1, 2, 3, 4, 5]);

// 여러 소스 결합하여 전달
int[] a = [1, 2];
int[] b = [3, 4];
PrintNumbers([.. a, .. b, 5]);
```

---

## 26.15 Default Lambda Parameters (람다 기본 매개변수)

C# 12에서는 람다 식에서도 메서드처럼 기본 매개변수를 사용할 수 있게 되었습니다.

**기본 사용법:**

```csharp
// 람다에 기본 매개변수
var greet = (string name = "손님") => $"안녕하세요, {name}님!";

Console.WriteLine(greet());          // 안녕하세요, 손님님!
Console.WriteLine(greet("홍길동"));   // 안녕하세요, 홍길동님!

// 여러 매개변수
var calculate = (int x, int y = 10) => x + y;

Console.WriteLine(calculate(5));      // 15
Console.WriteLine(calculate(5, 20));  // 25
```

**실전 예제 - 유연한 필터:**

```csharp
// 기본값이 있는 필터 함수
var filterByAge = (List<Person> people, int minAge = 0, int maxAge = 150) 
    => people.Where(p => p.Age >= minAge && p.Age <= maxAge).ToList();

var people = new List<Person>
{
    new("홍길동", 25),
    new("김철수", 35),
    new("이영희", 45)
};

// 기본값 사용 - 모든 연령
var all = filterByAge(people);

// 최소 연령만 지정
var over30 = filterByAge(people, 30);

// 범위 지정
var between30And40 = filterByAge(people, 30, 40);

public record Person(string Name, int Age);
```

**델리게이트와 함께:**

```csharp
Func<int, int, int> power = (int baseNum, int exponent = 2) 
    => (int)Math.Pow(baseNum, exponent);

Console.WriteLine(power(5));      // 25 (5^2)
Console.WriteLine(power(5, 3));   // 125 (5^3)
```

---

## 26.16 Alias Any Type (타입 별칭 확장)

C# 12에서는 `using` 별칭을 통해 튜플, 배열, 포인터 등 모든 타입에 대한 별칭을 만들 수 있게 되었습니다.

**기본 사용법:**

```csharp
// 튜플 별칭
using Point = (int X, int Y);
using Point3D = (int X, int Y, int Z);

// 복잡한 제네릭 타입
using StringDictionary = System.Collections.Generic.Dictionary<string, string>;
using IntList = System.Collections.Generic.List<int>;

// 사용
Point p = (10, 20);
Console.WriteLine($"좌표: ({p.X}, {p.Y})");

Point3D p3d = (10, 20, 30);
Console.WriteLine($"3D 좌표: ({p3d.X}, {p3d.Y}, {p3d.Z})");
```

**실전 예제 - 복잡한 타입 간소화:**

```csharp
// 복잡한 중첩 제네릭
using UserCache = System.Collections.Generic.Dictionary<
    string, 
    System.Collections.Generic.List<(int Id, string Name, System.DateTime LastLogin)>
>;

// 간단하게 사용
UserCache cache = new();
cache["admin"] = [(1, "관리자", DateTime.Now)];

// 함수 시그니처
using Validator = System.Func<string, (bool IsValid, string Message)>;

Validator emailValidator = email =>
{
    bool isValid = email.Contains("@");
    return (isValid, isValid ? "유효함" : "유효하지 않음");
};

var result = emailValidator("test@example.com");
Console.WriteLine($"결과: {result.Message}");
```

**배열 별칭:**

```csharp
using IntArray = int[];
using Matrix = int[][];

IntArray numbers = [1, 2, 3, 4, 5];
Matrix matrix = [[1, 2], [3, 4], [5, 6]];
```

---

## 26.17 Inline Arrays (인라인 배열)

Inline arrays는 구조체 내에 고정 크기 배열을 효율적으로 정의할 수 있게 하는 고급 기능입니다. 주로 고성능 시나리오에서 사용됩니다.

**기본 정의:**

```csharp
using System.Runtime.CompilerServices;

[InlineArray(10)]
public struct Buffer10
{
    private int _element0;
}

// 사용
Buffer10 buffer = default;
buffer[0] = 1;
buffer[1] = 2;

for (int i = 0; i < 10; i++)
{
    buffer[i] = i * 10;
}

// 배열처럼 접근
foreach (int value in buffer)
{
    Console.Write($"{value} ");
}
// 출력: 0 10 20 30 40 50 60 70 80 90
```

**Span과 함께 사용:**

```csharp
[InlineArray(100)]
public struct LargeBuffer
{
    private byte _element0;
}

void ProcessData()
{
    LargeBuffer buffer = default;
    Span<byte> span = buffer;
    
    // Span 연산 활용
    span.Fill(0xFF);
    span[0] = 0x00;
    span[99] = 0x00;
}
```

**주의사항:**
- 고성능이 중요한 시나리오에서만 사용
- 일반적인 컬렉션이 필요한 경우 List<T> 사용 권장
- stackalloc과 유사한 성능 특성

---

## 26.18 ref readonly Parameters (ref readonly 매개변수)

`ref readonly` 매개변수는 값을 참조로 전달하되, 메서드 내에서 수정할 수 없게 합니다. 이는 대형 구조체를 효율적으로 전달하면서도 안전성을 보장합니다.

**기본 사용법:**

```csharp
// 대형 구조체
public struct LargeStruct
{
    public int[] Data;
    public string Description;
    
    public LargeStruct()
    {
        Data = new int[1000];
        Description = "Large data structure";
    }
}

// ref readonly로 효율적이고 안전하게 전달
void ProcessData(ref readonly LargeStruct data)
{
    // 읽기는 가능
    Console.WriteLine(data.Description);
    Console.WriteLine($"데이터 크기: {data.Data.Length}");
    
    // 컴파일 오류! 수정 불가
    // data.Description = "Modified";
}

// 사용
LargeStruct largeData = new();
ProcessData(ref largeData);
```

**in vs ref readonly:**

```csharp
// in 매개변수 (C# 7.2)
void MethodWithIn(in int value)
{
    // 읽기만 가능
    Console.WriteLine(value);
}

// ref readonly 매개변수 (C# 12)
void MethodWithRefReadonly(ref readonly int value)
{
    // 읽기만 가능, 명시적으로 ref로 전달 필요
    Console.WriteLine(value);
}

int x = 10;
MethodWithIn(x);           // ref 없이 호출 가능
MethodWithRefReadonly(ref x);  // ref 필요
```

**실전 예제 - 성능 최적화:**

```csharp
public struct Vector3D
{
    public double X, Y, Z;
    
    public Vector3D(double x, double y, double z)
    {
        X = x; Y = y; Z = z;
    }
}

// 효율적인 벡터 연산
double DotProduct(ref readonly Vector3D a, ref readonly Vector3D b)
{
    return a.X * b.X + a.Y * b.Y + a.Z * b.Z;
}

Vector3D v1 = new(1, 2, 3);
Vector3D v2 = new(4, 5, 6);
double result = DotProduct(ref v1, ref v2);
Console.WriteLine($"내적: {result}");  // 32
```

---

## 26.19 실전 종합 예제

C# 12의 여러 기능을 활용한 실용적인 예제입니다:

```csharp
// 주 생성자 사용
public class TodoService(ITodoRepository repository, ILogger logger)
{
    // 컬렉션 식 사용
    public List<Todo> GetAllTodos() => [.. repository.GetAll()];
    
    public List<Todo> GetTodosByStatus(TodoStatus status = TodoStatus.Pending)
    {
        logger.LogInfo($"상태별 할 일 조회: {status}");
        return [.. repository.GetAll().Where(t => t.Status == status)];
    }
    
    public List<Todo> GetCombinedTodos(List<Todo> personal, List<Todo> work)
    {
        // 스프레드 연산자로 병합
        return [.. personal, .. work];
    }
    
    public void ProcessBatch(ref readonly TodoBatch batch)
    {
        logger.LogInfo($"배치 처리: {batch.Items.Length}개 항목");
        foreach (var item in batch.Items)
        {
            repository.Save(item);
        }
    }
}

// 타입 별칭
using TodoList = System.Collections.Generic.List<Todo>;
using TodoValidator = System.Func<Todo, (bool IsValid, string Message)>;

public class Todo(int id, string title)
{
    public int Id { get; } = id;
    public string Title { get; set; } = title;
    public TodoStatus Status { get; set; } = TodoStatus.Pending;
    public DateTime CreatedAt { get; init; } = DateTime.Now;
}

public enum TodoStatus
{
    Pending,
    InProgress,
    Completed
}

// 인라인 배열 사용
[InlineArray(50)]
public struct TodoBatch
{
    public Todo _element0;
}

// 인터페이스
public interface ITodoRepository
{
    IEnumerable<Todo> GetAll();
    void Save(Todo todo);
}

public interface ILogger
{
    void LogInfo(string message);
}

// 람다 기본 매개변수
TodoValidator CreateValidator(int maxTitleLength = 100)
{
    return todo =>
    {
        if (string.IsNullOrEmpty(todo.Title))
            return (false, "제목이 비어있습니다");
        if (todo.Title.Length > maxTitleLength)
            return (false, $"제목이 너무 깁니다 (최대 {maxTitleLength}자)");
        return (true, "유효합니다");
    };
}

// 사용 예제
var mockRepo = new MockTodoRepository();
var mockLogger = new MockLogger();
var service = new TodoService(mockRepo, mockLogger);

// 컬렉션 식으로 할 일 생성
TodoList todos = 
[
    new(1, "C# 12 학습"),
    new(2, "프로젝트 완료"),
    new(3, "코드 리뷰")
];

foreach (var todo in todos)
{
    mockRepo.Save(todo);
}

// 기본 매개변수 사용
var pending = service.GetTodosByStatus();
var completed = service.GetTodosByStatus(TodoStatus.Completed);

Console.WriteLine($"대기 중: {pending.Count}개");
Console.WriteLine($"완료: {completed.Count}개");

// Mock 구현
class MockTodoRepository : ITodoRepository
{
    private readonly List<Todo> _todos = [];
    
    public IEnumerable<Todo> GetAll() => _todos;
    public void Save(Todo todo) => _todos.Add(todo);
}

class MockLogger : ILogger
{
    public void LogInfo(string message) => Console.WriteLine($"[INFO] {message}");
}
```

---


---

# 제4부: C# 13 주요 기능

## 26.20 params Collections (params 컬렉션)

C# 13은 `params` 키워드를 배열뿐만 아니라 `Span<T>`, `ReadOnlySpan<T>` 등 다양한 컬렉션 타입으로 확장했습니다. 이는 성능을 크게 향상시키면서도 메모리 할당을 줄일 수 있게 합니다.

**전통적인 params (배열):**

```csharp
// C# 12 이전 - 항상 배열로 할당
void PrintNumbers(params int[] numbers)
{
    foreach (int num in numbers)
    {
        Console.WriteLine(num);
    }
}

PrintNumbers(1, 2, 3);  // int[] 배열 할당 발생
```

**C# 13의 params 확장:**

```csharp
// Span 사용 - 스택 할당 가능
void PrintNumbers(params ReadOnlySpan<int> numbers)
{
    foreach (int num in numbers)
    {
        Console.WriteLine(num);
    }
}

// 스택 할당으로 성능 향상
PrintNumbers(1, 2, 3, 4, 5);

// IEnumerable 사용
void ProcessItems(params IEnumerable<string> items)
{
    foreach (var item in items)
    {
        Console.WriteLine(item.ToUpper());
    }
}

ProcessItems("hello", "world", "C#");
```

**실전 예제 - 수학 함수:**

```csharp
// 가변 인자 수학 함수들
double Sum(params ReadOnlySpan<double> values)
{
    double sum = 0;
    foreach (var value in values)
    {
        sum += value;
    }
    return sum;
}

double Average(params ReadOnlySpan<double> values)
{
    if (values.Length == 0) return 0;
    return Sum(values) / values.Length;
}

double Max(params ReadOnlySpan<double> values)
{
    if (values.Length == 0) 
        throw new ArgumentException("최소 하나의 값이 필요합니다");
    
    double max = values[0];
    for (int i = 1; i < values.Length; i++)
    {
        if (values[i] > max) max = values[i];
    }
    return max;
}

// 사용
Console.WriteLine($"합계: {Sum(1.5, 2.3, 3.7, 4.2)}");
Console.WriteLine($"평균: {Average(10, 20, 30, 40)}");
Console.WriteLine($"최댓값: {Max(5.5, 3.2, 8.9, 2.1)}");
```

**성능 비교:**

```csharp
using System.Diagnostics;

// 배열 기반 (힙 할당)
void OldWay(params int[] numbers)
{
    int sum = 0;
    foreach (var n in numbers) sum += n;
}

// Span 기반 (스택 할당)
void NewWay(params ReadOnlySpan<int> numbers)
{
    int sum = 0;
    foreach (var n in numbers) sum += n;
}

// 성능 측정
var sw = Stopwatch.StartNew();
for (int i = 0; i < 1000000; i++)
{
    OldWay(1, 2, 3, 4, 5);
}
Console.WriteLine($"배열: {sw.ElapsedMilliseconds}ms");

sw.Restart();
for (int i = 0; i < 1000000; i++)
{
    NewWay(1, 2, 3, 4, 5);
}
Console.WriteLine($"Span: {sw.ElapsedMilliseconds}ms");
// Span이 훨씬 빠름!
```

---

## 26.21 New Lock Type and Semantics (새로운 lock 타입)

C# 13은 `System.Threading.Lock` 타입을 도입하여 더 효율적이고 안전한 동기화를 제공합니다. 기존의 `lock(object)` 패턴보다 성능이 우수하고 더 명확한 의도를 표현합니다.

**전통적인 lock:**

```csharp
public class Counter
{
    private readonly object _lock = new object();
    private int _count;
    
    public void Increment()
    {
        lock (_lock)
        {
            _count++;
        }
    }
    
    public int GetCount()
    {
        lock (_lock)
        {
            return _count;
        }
    }
}
```

**C# 13의 새로운 Lock 타입:**

```csharp
using System.Threading;

public class Counter
{
    private readonly Lock _lock = new();
    private int _count;
    
    public void Increment()
    {
        lock (_lock)
        {
            _count++;
        }
    }
    
    public int GetCount()
    {
        lock (_lock)
        {
            return _count;
        }
    }
}
```

**Lock.EnterScope() 사용:**

```csharp
public class BankAccount
{
    private readonly Lock _lock = new();
    private decimal _balance;
    
    public void Deposit(decimal amount)
    {
        using (_lock.EnterScope())
        {
            _balance += amount;
            Console.WriteLine($"입금: {amount:C}, 잔액: {_balance:C}");
        }
    }
    
    public void Withdraw(decimal amount)
    {
        using (_lock.EnterScope())
        {
            if (_balance >= amount)
            {
                _balance -= amount;
                Console.WriteLine($"출금: {amount:C}, 잔액: {_balance:C}");
            }
            else
            {
                Console.WriteLine("잔액 부족");
            }
        }
    }
    
    public decimal GetBalance()
    {
        using (_lock.EnterScope())
        {
            return _balance;
        }
    }
}

// 사용
var account = new BankAccount();
account.Deposit(1000);
account.Withdraw(300);
Console.WriteLine($"최종 잔액: {account.GetBalance():C}");
```

**성능 이점:**

```csharp
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

public class LockComparison
{
    private readonly object _objectLock = new();
    private readonly Lock _newLock = new();
    private int _counter;
    
    public void TestObjectLock()
    {
        var tasks = new Task[10];
        _counter = 0;
        
        var sw = Stopwatch.StartNew();
        for (int i = 0; i < 10; i++)
        {
            tasks[i] = Task.Run(() =>
            {
                for (int j = 0; j < 100000; j++)
                {
                    lock (_objectLock)
                    {
                        _counter++;
                    }
                }
            });
        }
        Task.WaitAll(tasks);
        Console.WriteLine($"Object lock: {sw.ElapsedMilliseconds}ms");
    }
    
    public void TestNewLock()
    {
        var tasks = new Task[10];
        _counter = 0;
        
        var sw = Stopwatch.StartNew();
        for (int i = 0; i < 10; i++)
        {
            tasks[i] = Task.Run(() =>
            {
                for (int j = 0; j < 100000; j++)
                {
                    lock (_newLock)
                    {
                        _counter++;
                    }
                }
            });
        }
        Task.WaitAll(tasks);
        Console.WriteLine($"New Lock: {sw.ElapsedMilliseconds}ms");
    }
}

// 성능 비교
var comparison = new LockComparison();
comparison.TestObjectLock();
comparison.TestNewLock();
```

---

## 26.22 New Escape Sequence: \e

C# 13은 ANSI 이스케이프 코드를 나타내는 `\e` 이스케이프 시퀀스를 추가했습니다. 이는 터미널에서 색상과 스타일을 제어하는 데 유용합니다.

**기본 사용법:**

```csharp
// 기존 방식
string redText = "\x1b[31m빨간색 텍스트\x1b[0m";
string greenText = "\u001b[32m초록색 텍스트\u001b[0m";

// C# 13 - 더 명확한 \e
string blueText = "\e[34m파란색 텍스트\e[0m";
string boldText = "\e[1m굵은 텍스트\e[0m";

Console.WriteLine(redText);
Console.WriteLine(greenText);
Console.WriteLine(blueText);
Console.WriteLine(boldText);
```

**실전 예제 - 터미널 출력 헬퍼:**

```csharp
public static class TerminalColor
{
    public const string Reset = "\e[0m";
    
    // 전경색
    public const string Black = "\e[30m";
    public const string Red = "\e[31m";
    public const string Green = "\e[32m";
    public const string Yellow = "\e[33m";
    public const string Blue = "\e[34m";
    public const string Magenta = "\e[35m";
    public const string Cyan = "\e[36m";
    public const string White = "\e[37m";
    
    // 배경색
    public const string BgBlack = "\e[40m";
    public const string BgRed = "\e[41m";
    public const string BgGreen = "\e[42m";
    public const string BgYellow = "\e[43m";
    
    // 스타일
    public const string Bold = "\e[1m";
    public const string Underline = "\e[4m";
    
    public static void WriteSuccess(string message)
    {
        Console.WriteLine($"{Green}✓ {message}{Reset}");
    }
    
    public static void WriteError(string message)
    {
        Console.WriteLine($"{Red}✗ {message}{Reset}");
    }
    
    public static void WriteWarning(string message)
    {
        Console.WriteLine($"{Yellow}⚠ {message}{Reset}");
    }
    
    public static void WriteInfo(string message)
    {
        Console.WriteLine($"{Cyan}ℹ {message}{Reset}");
    }
}

// 사용
TerminalColor.WriteSuccess("작업이 성공적으로 완료되었습니다.");
TerminalColor.WriteError("오류가 발생했습니다.");
TerminalColor.WriteWarning("경고: 확인이 필요합니다.");
TerminalColor.WriteInfo("정보: 처리 중입니다.");
```

---

## 26.23 ref struct Types Implement Interfaces

C# 13에서는 `ref struct` 타입이 인터페이스를 구현할 수 있게 되었습니다. 이는 타입 안전성과 코드 재사용성을 크게 향상시킵니다.

**기본 사용법:**

```csharp
public interface IProcessor
{
    void Process();
}

// ref struct가 인터페이스 구현
public ref struct DataProcessor : IProcessor
{
    private Span<byte> _data;
    
    public DataProcessor(Span<byte> data)
    {
        _data = data;
    }
    
    public void Process()
    {
        for (int i = 0; i < _data.Length; i++)
        {
            _data[i] *= 2;
        }
    }
}

// 사용
Span<byte> data = stackalloc byte[] { 1, 2, 3, 4, 5 };
var processor = new DataProcessor(data);
processor.Process();

foreach (byte b in data)
{
    Console.Write($"{b} ");  // 2 4 6 8 10
}
```

**실전 예제 - 범용 스트림 처리:**

```csharp
public interface IStreamReader<T>
{
    T Read();
    bool HasMore();
}

public ref struct SpanStreamReader : IStreamReader<int>
{
    private ReadOnlySpan<int> _span;
    private int _position;
    
    public SpanStreamReader(ReadOnlySpan<int> span)
    {
        _span = span;
        _position = 0;
    }
    
    public int Read()
    {
        if (_position >= _span.Length)
            throw new InvalidOperationException("스트림 끝");
        
        return _span[_position++];
    }
    
    public bool HasMore() => _position < _span.Length;
}

// 범용 처리 함수
void ProcessStream<TReader>(TReader reader) where TReader : IStreamReader<int>
{
    while (reader.HasMore())
    {
        int value = reader.Read();
        Console.WriteLine($"읽은 값: {value}");
    }
}

// 사용
ReadOnlySpan<int> numbers = stackalloc int[] { 10, 20, 30, 40 };
var reader = new SpanStreamReader(numbers);
ProcessStream(reader);
```

---

## 26.24 Allow ref and unsafe in Async and Iterator Methods

C# 13은 async 메서드와 iterator에서 `ref` 지역 변수와 `unsafe` 컨텍스트를 제한적으로 사용할 수 있게 합니다.

**ref locals in async methods:**

```csharp
async Task<int> ProcessDataAsync(int[] array)
{
    // ref 지역 변수 사용 가능 (await 전에)
    ref int first = ref array[0];
    first = 100;
    
    // await 이후에는 ref 사용 불가
    await Task.Delay(100);
    
    return array.Sum();
}

// 사용
int[] numbers = { 1, 2, 3, 4, 5 };
int result = await ProcessDataAsync(numbers);
Console.WriteLine($"결과: {result}");  // 109 (100 + 2 + 3 + 4 + 5)
```

**Iterator with ref locals:**

```csharp
IEnumerable<int> GenerateNumbers(int[] array)
{
    // ref 지역 변수 사용
    ref int current = ref array[0];
    current = 0;
    
    for (int i = 0; i < array.Length; i++)
    {
        yield return array[i] * 2;
    }
}

// 사용
int[] data = { 1, 2, 3 };
foreach (var num in GenerateNumbers(data))
{
    Console.WriteLine(num);  // 0, 4, 6
}
```

---

## 26.25 Implicit Indexer Access in Object Initializers

객체 초기화 구문에서 인덱서를 암시적으로 사용할 수 있게 되었습니다.

**기본 사용법:**

```csharp
public class Matrix
{
    private int[,] _data = new int[3, 3];
    
    public int this[int row, int col]
    {
        get => _data[row, col];
        set => _data[row, col] = value;
    }
}

// C# 13 - 암시적 인덱서 접근
var matrix = new Matrix
{
    [0, 0] = 1,
    [0, 1] = 2,
    [0, 2] = 3,
    [1, 0] = 4,
    [1, 1] = 5,
    [1, 2] = 6
};
```

**실전 예제 - 설정 딕셔너리:**

```csharp
public class Configuration
{
    private Dictionary<string, string> _settings = new();
    
    public string this[string key]
    {
        get => _settings.TryGetValue(key, out var value) ? value : "";
        set => _settings[key] = value;
    }
    
    public void Display()
    {
        foreach (var (key, value) in _settings)
        {
            Console.WriteLine($"{key}: {value}");
        }
    }
}

// 간편한 초기화
var config = new Configuration
{
    ["host"] = "localhost",
    ["port"] = "8080",
    ["timeout"] = "30",
    ["ssl"] = "true"
};

config.Display();
```

---

## 26.26 Overload Resolution Priority

`OverloadResolutionPriorityAttribute`를 사용하여 오버로드 해결 우선순위를 명시적으로 제어할 수 있습니다.

**기본 사용법:**

```csharp
using System.Runtime.CompilerServices;

public class DataProcessor
{
    // 기본 우선순위 (0)
    public void Process(int value)
    {
        Console.WriteLine($"int 처리: {value}");
    }
    
    // 낮은 우선순위
    [OverloadResolutionPriority(-1)]
    public void Process(object value)
    {
        Console.WriteLine($"object 처리: {value}");
    }
    
    // 높은 우선순위
    [OverloadResolutionPriority(1)]
    public void Process(string value)
    {
        Console.WriteLine($"string 처리: {value}");
    }
}

var processor = new DataProcessor();
processor.Process(42);        // int 처리
processor.Process("hello");   // string 처리 (우선순위 높음)
processor.Process(3.14);      // object 처리 (낮은 우선순위)
```

---

## 26.27 실전 종합 예제

```csharp
using System.Threading;

// C# 13 종합 예제: 고성능 데이터 처리 시스템
public ref struct DataBuffer : IDisposable
{
    private Span<byte> _buffer;
    private int _position;
    
    public DataBuffer(Span<byte> buffer)
    {
        _buffer = buffer;
        _position = 0;
    }
    
    public void Write(params ReadOnlySpan<byte> data)
    {
        data.CopyTo(_buffer.Slice(_position));
        _position += data.Length;
    }
    
    public ReadOnlySpan<byte> GetData() => _buffer.Slice(0, _position);
    
    public void Dispose()
    {
        _buffer.Clear();
    }
}

public class DataProcessor
{
    private readonly Lock _lock = new();
    private int _processedCount;
    
    [OverloadResolutionPriority(1)]
    public void Process(params ReadOnlySpan<byte> data)
    {
        using (_lock.EnterScope())
        {
            _processedCount += data.Length;
            TerminalColor.WriteSuccess($"처리됨: {data.Length} 바이트");
        }
    }
    
    [OverloadResolutionPriority(-1)]
    public void Process(object data)
    {
        TerminalColor.WriteWarning("일반 객체 처리");
    }
    
    public int GetProcessedCount()
    {
        using (_lock.EnterScope())
        {
            return _processedCount;
        }
    }
}

// 사용 예제
Span<byte> buffer = stackalloc byte[1024];
using (var dataBuffer = new DataBuffer(buffer))
{
    dataBuffer.Write(1, 2, 3, 4, 5);
    dataBuffer.Write(10, 20, 30);
    
    var processor = new DataProcessor();
    processor.Process(dataBuffer.GetData());
    
    Console.WriteLine($"총 처리: {processor.GetProcessedCount()} 바이트");
}
```

---


---

# 제5부: C# 14 프리뷰 기능

> **중요 고지사항**: 
> C# 14는 현재 프리뷰 단계에 있으며, 이 섹션의 내용은 변경될 수 있습니다. 프로덕션 코드에 사용하기 전에 반드시 최신 공식 문서를 확인하시기 바랍니다.

## 26.28 Field Keyword (필드 키워드)

`field` 키워드는 속성의 접근자 내에서 자동으로 생성되는 백킹 필드에 직접 접근할 수 있게 해주는 기능입니다.

**전통적인 방식:**

```csharp
// 명시적 백킹 필드
public class Person
{
    private string _name;
    
    public string Name
    {
        get => _name;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("이름은 필수입니다");
            _name = value.Trim();
        }
    }
}
```

**field 키워드 사용 (C# 14 프리뷰):**

```csharp
// field 키워드로 백킹 필드 자동 접근
public class Person
{
    public string Name
    {
        get => field;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("이름은 필수입니다");
            field = value.Trim();
        }
    }
}

// 사용
var person = new Person { Name = "  홍길동  " };
Console.WriteLine($"이름: '{person.Name}'");  // 이름: '홍길동'
```

**실전 예제 - 유효성 검사:**

```csharp
public class Product
{
    public string Name
    {
        get => field;
        set => field = string.IsNullOrWhiteSpace(value) 
            ? throw new ArgumentException("제품명은 필수입니다") 
            : value;
    }
    
    public decimal Price
    {
        get => field;
        set => field = value >= 0 
            ? value 
            : throw new ArgumentException("가격은 0 이상이어야 합니다");
    }
    
    public int Stock
    {
        get => field;
        set
        {
            if (value < 0)
                throw new ArgumentException("재고는 음수일 수 없습니다");
            
            if (field != value)
            {
                field = value;
                Console.WriteLine($"{Name}의 재고가 {value}로 변경되었습니다");
            }
        }
    }
}

// 사용
var product = new Product 
{ 
    Name = "노트북", 
    Price = 1500000, 
    Stock = 10 
};

product.Stock = 5;  // 재고 변경 알림
// product.Price = -100;  // 예외 발생!
```

**지연 초기화:**

```csharp
public class DataLoader
{
    private bool _isInitialized;
    
    public string Data
    {
        get
        {
            if (!_isInitialized)
            {
                field = LoadExpensiveData();
                _isInitialized = true;
            }
            return field;
        }
    }
    
    private string LoadExpensiveData()
    {
        Console.WriteLine("데이터 로딩 중...");
        System.Threading.Thread.Sleep(1000);
        return "비싼 데이터";
    }
}

// 처음 접근 시에만 로딩
var loader = new DataLoader();
Console.WriteLine(loader.Data);  // 데이터 로딩 중...
Console.WriteLine(loader.Data);  // 캐시된 데이터 사용
```

---

## 26.29 Extensions (확장 타입)

Extensions는 기존의 확장 메서드를 넘어서, 타입에 속성, 이벤트, 심지어 인터페이스 구현까지 추가할 수 있는 새로운 메커니즘입니다. (주의: 이 기능은 아직 설계 단계이며 변경될 수 있습니다)

**개념적 예제:**

```csharp
// 확장 타입 정의 (예상 문법)
public extension StringExtensions for string
{
    // 확장 속성
    public bool IsValidEmail => this.Contains("@") && this.Contains(".");
    
    // 확장 메서드
    public string Truncate(int maxLength)
    {
        return this.Length <= maxLength 
            ? this 
            : this.Substring(0, maxLength) + "...";
    }
    
    // 계산 속성
    public int WordCount => this.Split(' ', StringSplitOptions.RemoveEmptyEntries).Length;
}

// 사용
string email = "user@example.com";
Console.WriteLine(email.IsValidEmail);  // True

string text = "이것은 긴 텍스트입니다";
Console.WriteLine(text.Truncate(5));  // 이것은 긴...
Console.WriteLine(text.WordCount);  // 4
```

**확장 인터페이스 구현 (개념):**

```csharp
// 기존 타입에 인터페이스 구현 추가
public interface IValidatable
{
    bool IsValid();
}

public extension PersonExtension for Person : IValidatable
{
    public bool IsValid()
    {
        return !string.IsNullOrWhiteSpace(this.Name) && this.Age > 0;
    }
}

// 사용
Person person = new Person { Name = "홍길동", Age = 30 };
if (person is IValidatable validatable)
{
    Console.WriteLine($"유효성: {validatable.IsValid()}");
}
```

**참고**: Extensions 기능은 아직 C# 언어 설계 팀에서 논의 중이며, 최종 문법과 동작은 변경될 수 있습니다.

---

## 26.30 Discriminated Unions (판별된 합 타입)

Discriminated unions (또는 sum types)는 여러 타입 중 하나를 안전하게 표현할 수 있는 기능입니다. F#이나 Rust 같은 언어에서 이미 제공하는 기능으로, C# 14에서도 도입이 논의되고 있습니다.

**현재 방식의 한계:**

```csharp
// C# 12까지 - 상속과 패턴 매칭으로 흉내
public abstract record Result
{
    public record Success(string Value) : Result;
    public record Error(string Message) : Result;
}

Result ProcessData()
{
    if (Random.Shared.Next(2) == 0)
        return new Result.Success("성공!");
    return new Result.Error("실패!");
}

// 사용
var result = ProcessData();
string message = result switch
{
    Result.Success s => $"성공: {s.Value}",
    Result.Error e => $"오류: {e.Message}",
    _ => "알 수 없음"
};
```

**예상되는 Discriminated Union 문법:**

```csharp
// C# 14 예상 문법 (아직 확정되지 않음)
public union Result<T>
{
    Success(T value),
    Error(string message)
}

// 또는
public enum Result<T>
{
    case Success(T value);
    case Error(string message);
}

// 사용
Result<string> ProcessData()
{
    if (Random.Shared.Next(2) == 0)
        return Result.Success("데이터 처리 완료");
    return Result.Error("처리 실패");
}

// 패턴 매칭
var result = ProcessData();
string message = result switch
{
    Success(var value) => $"성공: {value}",
    Error(var msg) => $"오류: {msg}"
};
```

**실전 예제 (현재 패턴으로 구현):**

```csharp
// 현재 C#으로 구현 가능한 방식
public abstract record Option<T>
{
    public record Some(T Value) : Option<T>;
    public record None : Option<T>;
}

public static class OptionExtensions
{
    public static Option<T> ToOption<T>(this T? value) where T : class
    {
        return value is null ? new Option<T>.None() : new Option<T>.Some(value);
    }
    
    public static TResult Match<T, TResult>(
        this Option<T> option,
        Func<T, TResult> some,
        Func<TResult> none)
    {
        return option switch
        {
            Option<T>.Some s => some(s.Value),
            Option<T>.None => none(),
            _ => throw new InvalidOperationException()
        };
    }
}

// 사용 예제
Option<string> FindUser(int id)
{
    return id > 0 
        ? new Option<string>.Some($"사용자 {id}") 
        : new Option<string>.None();
}

var user = FindUser(1);
string message = user.Match(
    some: name => $"찾음: {name}",
    none: () => "사용자를 찾을 수 없습니다"
);

Console.WriteLine(message);
```

---

## 26.31 패턴 매칭 개선

C# 14에서는 패턴 매칭이 더욱 강화될 것으로 예상됩니다.

**예상되는 개선사항:**

```csharp
// 더 강력한 리스트 패턴
int[] numbers = { 1, 2, 3, 4, 5 };

// 조건부 패턴 (예상)
if (numbers is [var first, .. var middle, var last] when first > 0)
{
    Console.WriteLine($"첫 번째: {first}, 마지막: {last}");
    Console.WriteLine($"중간: {string.Join(", ", middle)}");
}

// 중첩 패턴 개선
record Person(string Name, Address Address);
record Address(string City, string Country);

Person person = new("홍길동", new Address("서울", "한국"));

// 더 간결한 중첩 패턴
if (person is { Address.City: "서울", Address.Country: var country })
{
    Console.WriteLine($"서울, {country}에 거주");
}
```

**실전 예제 - 복잡한 데이터 처리:**

```csharp
record Order(int Id, string Status, decimal Amount, Customer Customer);
record Customer(string Name, string Tier);

void ProcessOrder(Order order)
{
    string result = order switch
    {
        // 복잡한 중첩 패턴
        { Status: "pending", Amount: > 1000000, Customer.Tier: "VIP" } 
            => "VIP 대형 주문 우선 처리",
        
        { Status: "pending", Amount: > 100000 } 
            => "대형 주문 처리",
        
        { Status: "pending", Customer.Tier: "VIP" } 
            => "VIP 주문 처리",
        
        { Status: "cancelled" } 
            => "취소된 주문",
        
        _ => "일반 처리"
    };
    
    Console.WriteLine($"주문 {order.Id}: {result}");
}

// 사용
var orders = new[]
{
    new Order(1, "pending", 1500000, new Customer("홍길동", "VIP")),
    new Order(2, "pending", 50000, new Customer("김철수", "Regular")),
    new Order(3, "cancelled", 100000, new Customer("이영희", "VIP"))
};

foreach (var order in orders)
{
    ProcessOrder(order);
}
```

---

## 26.32 성능 최적화

C# 14는 계속해서 성능과 메모리 효율성을 개선할 것으로 예상됩니다.

**예상되는 개선 영역:**

1. **더 나은 인라이닝**: 컴파일러가 더 많은 메서드를 인라이닝하여 성능 향상
2. **개선된 구조체 처리**: 구조체 복사 최소화
3. **SIMD 최적화**: 벡터화된 연산 개선
4. **메모리 할당 감소**: 더 많은 상황에서 스택 할당 활용

**현재 사용 가능한 고성능 패턴:**

```csharp
using System.Runtime.CompilerServices;
using System.Runtime.Intrinsics;

// 고성능 벡터 연산
[MethodImpl(MethodImplOptions.AggressiveInlining)]
static void MultiplyArrays(Span<int> a, Span<int> b, Span<int> result)
{
    for (int i = 0; i < a.Length; i++)
    {
        result[i] = a[i] * b[i];
    }
}

// SIMD 벡터 사용
static void MultiplyArraysSimd(Span<int> a, Span<int> b, Span<int> result)
{
    int vectorSize = Vector<int>.Count;
    int i = 0;
    
    // 벡터화된 처리
    for (; i <= a.Length - vectorSize; i += vectorSize)
    {
        var va = new Vector<int>(a.Slice(i, vectorSize));
        var vb = new Vector<int>(b.Slice(i, vectorSize));
        (va * vb).CopyTo(result.Slice(i, vectorSize));
    }
    
    // 나머지 요소 처리
    for (; i < a.Length; i++)
    {
        result[i] = a[i] * b[i];
    }
}

// 성능 비교
using System.Diagnostics;

int size = 1000000;
int[] a = new int[size];
int[] b = new int[size];
int[] result = new int[size];

// 배열 초기화
for (int i = 0; i < size; i++)
{
    a[i] = i;
    b[i] = i * 2;
}

var sw = Stopwatch.StartNew();
MultiplyArrays(a, b, result);
Console.WriteLine($"일반: {sw.ElapsedMilliseconds}ms");

sw.Restart();
MultiplyArraysSimd(a, b, result);
Console.WriteLine($"SIMD: {sw.ElapsedMilliseconds}ms");
```

---

## 26.33 기타 예상 기능

### 더 나은 타입 추론

```csharp
// 더 스마트한 타입 추론 (예상)
var numbers = [1, 2, 3];  // List<int>로 추론
var mixed = [1, "text", 3.14];  // 공통 타입으로 추론?

// 람다 타입 추론 개선
var multiply = (x, y) => x * y;  // 제네릭 함수로 추론?
```

### UTF-8 문자열 개선

```csharp
// UTF-8 문자열 리터럴 개선 (예상)
ReadOnlySpan<byte> utf8String = "Hello"u8;
string text = "안녕하세요"u8.ToString();
```

### 개선된 nullable 분석

```csharp
// 더 스마트한 null 체크
string? GetValue() => Random.Shared.Next(2) == 0 ? "value" : null;

string value = GetValue();
if (value is not null)
{
    // C# 14: 컴파일러가 더 정확히 null이 아님을 인식
    ProcessNonNull(value);
}

void ProcessNonNull(string nonNullValue)
{
    Console.WriteLine(nonNullValue.Length);
}
```

---

## 26.34 실전 종합 예제

```csharp
// C# 14 프리뷰 기능을 활용한 예제
public class UserService
{
    public string Name
    {
        get => field;
        set => field = string.IsNullOrWhiteSpace(value)
            ? throw new ArgumentException("이름 필수")
            : value.Trim();
    }
    
    // Option 타입으로 안전한 반환
    public Option<User> FindUser(int id)
    {
        return id > 0 
            ? new Option<User>.Some(new User { Id = id, Name = $"사용자{id}" })
            : new Option<User>.None();
    }
    
    // 패턴 매칭으로 처리
    public string ProcessUser(int id)
    {
        return FindUser(id) switch
        {
            Option<User>.Some { Value: var user } => $"찾음: {user.Name}",
            Option<User>.None => "사용자 없음",
            _ => "오류"
        };
    }
}

public class User
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
}

// 사용
var service = new UserService { Name = "UserService" };
Console.WriteLine(service.ProcessUser(1));  // 찾음: 사용자1
Console.WriteLine(service.ProcessUser(-1)); // 사용자 없음
```

---


---

## 마치며

C# 10부터 14까지의 발전은 언어가 어떻게 지속적으로 진화하며 개발자의 요구를 수용하는지를 보여주는 훌륭한 사례입니다. 

**주요 성과:**

- **C# 10**: Global using과 file-scoped namespace로 보일러플레이트 코드 대폭 감소
- **C# 11**: Raw string literals로 문자열 처리 혁신, generic math로 제네릭 확장
- **C# 12**: Primary constructors와 collection expressions로 코드 간결성 극대화
- **C# 13**: params collections와 new Lock으로 성능 대폭 향상
- **C# 14**: Field keyword와 extensions로 더욱 표현력 있는 코드 작성 가능

이러한 기능들은 단순히 새로운 문법을 추가하는 것을 넘어, 더 안전하고, 더 간결하며, 더 성능이 좋은 코드를 작성할 수 있도록 돕습니다. C#은 계속 진화하고 있으며, 앞으로도 개발자 커뮤니티의 피드백을 반영하여 발전할 것입니다.

**학습한 내용 요약:**

**C# 10-11 (기초)**
- Global using, File-scoped namespace로 코드 정리
- Raw string literals로 복잡한 문자열 처리
- List patterns로 컬렉션 패턴 매칭
- Required members로 객체 초기화 안전성 강화

**C# 12-13 (중급)**
- Primary constructors로 클래스 정의 간소화
- Collection expressions로 컬렉션 초기화 통일
- params collections로 성능 최적화
- New Lock type으로 동기화 성능 향상

**C# 14 (고급, 프리뷰)**
- Field keyword로 속성 정의 간소화
- Extensions로 타입 확장 메커니즘 혁신
- Discriminated unions로 타입 안전성 강화 (예정)

**다음 단계:**
- 실제 프로젝트에 최신 C# 기능 적용
- .NET 6-10의 새로운 API 활용
- 성능 프로파일링과 최적화 기법 학습
- 오픈소스 프로젝트에서 최신 패턴 관찰

**참고 자료:**
- [C# 10의 새로운 기능](https://learn.microsoft.com/ko-kr/dotnet/csharp/whats-new/csharp-10)
- [C# 11의 새로운 기능](https://learn.microsoft.com/ko-kr/dotnet/csharp/whats-new/csharp-11)
- [C# 12의 새로운 기능](https://learn.microsoft.com/ko-kr/dotnet/csharp/whats-new/csharp-12)
- [C# 13의 새로운 기능](https://learn.microsoft.com/ko-kr/dotnet/csharp/whats-new/csharp-13)
- [C# 언어 디자인 GitHub](https://github.com/dotnet/csharplang)

계속 학습하고, 실험하고, 최신 기능을 활용하세요. Happy coding!
