# 23장. NuGet 패키지 관리

.NET 개발에서 외부 라이브러리를 사용하는 것은 필수적입니다. NuGet은 .NET 생태계의 공식 패키지 관리자로, 수십만 개의 오픈소스 라이브러리를 손쉽게 프로젝트에 추가하고 관리할 수 있게 해줍니다. 이 장에서는 NuGet의 기본 개념과 사용 방법, 그리고 실무에서 자주 사용되는 유용한 패키지들을 소개합니다.

## 이 장에서 배울 내용

- NuGet 패키지 관리자의 개념과 역할
- .NET CLI를 사용한 패키지 설치 및 제거
- Newtonsoft.Json, Serilog, Dapper 등 주요 패키지 활용법

---

## 23.1 NuGet이란?

NuGet(누겟)은 .NET 플랫폼의 공식 패키지 관리자입니다. JavaScript의 npm, Python의 pip, Ruby의 gem과 같은 역할을 하며, 개발자들이 만든 라이브러리를 쉽게 공유하고 사용할 수 있게 해줍니다.

### NuGet의 주요 개념

**패키지(Package):**
- 재사용 가능한 코드를 묶어놓은 단위
- `.nupkg` 확장자를 가진 압축 파일
- DLL 파일, 문서, 메타데이터 등을 포함

**NuGet 갤러리:**
- 공식 패키지 저장소: https://www.nuget.org
- 누구나 패키지를 게시하고 다운로드 가능
- 2024년 기준 50만 개 이상의 패키지 보유

**패키지 버전 관리:**
- 시맨틱 버저닝(Semantic Versioning) 사용: `Major.Minor.Patch`
- 예: `13.0.3` (주 버전.부 버전.패치 버전)
- 프로젝트별로 필요한 버전 명시 가능

### NuGet의 장점

1. **의존성 자동 관리**: 패키지가 필요로 하는 다른 패키지들을 자동으로 설치
2. **버전 관리**: 프로젝트별로 다른 버전의 패키지 사용 가능
3. **업데이트 용이성**: 명령어 하나로 최신 버전으로 업데이트
4. **표준화**: .NET 생태계 전체에서 사용하는 표준 도구

### .NET SDK에 포함된 NuGet

.NET SDK를 설치하면 NuGet이 자동으로 포함됩니다. `dotnet` 명령어를 통해 패키지를 관리할 수 있습니다.

```bash
# NuGet 버전 확인
dotnet --version
```

---

## 23.2 패키지 설치 및 제거

NuGet 패키지는 .NET CLI를 사용하여 쉽게 설치하고 제거할 수 있습니다. Visual Studio Code에서 터미널을 열어 프로젝트 폴더에서 명령어를 실행합니다.

### 패키지 검색

```bash
# NuGet 웹사이트에서 검색
# https://www.nuget.org에서 원하는 패키지 검색
```

### 패키지 설치

**.NET CLI로 패키지 설치:**

```bash
# 기본 설치 (최신 안정 버전)
dotnet add package 패키지이름

# 특정 버전 설치
dotnet add package 패키지이름 --version 버전번호

# 예: Newtonsoft.Json 설치
dotnet add package Newtonsoft.Json
```

**설치 후 변화:**

패키지를 설치하면 프로젝트 파일(`*.csproj`)에 자동으로 추가됩니다:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
  </ItemGroup>
</Project>
```

### 패키지 제거

```bash
# 패키지 제거
dotnet remove package 패키지이름

# 예: Newtonsoft.Json 제거
dotnet remove package Newtonsoft.Json
```

### 패키지 업데이트

```bash
# 모든 패키지 업데이트
dotnet restore

# 특정 패키지를 최신 버전으로 업데이트
dotnet add package 패키지이름
```

### 설치된 패키지 확인

```bash
# 프로젝트의 패키지 목록 보기
dotnet list package
```

### 패키지 복원

프로젝트를 다른 컴퓨터로 옮기거나 Git에서 클론했을 때, `obj/`와 `bin/` 폴더는 제외되어 있습니다. 패키지를 다시 다운로드하려면:

```bash
# 프로젝트 파일에 명시된 모든 패키지 복원
dotnet restore
```

이 명령은 `dotnet build`나 `dotnet run` 실행 시 자동으로 호출됩니다.

---

## 23.3 유용한 NuGet 패키지 소개

실무에서 자주 사용되는 세 가지 핵심 패키지를 소개합니다. 이 패키지들은 JSON 처리, 로깅, 데이터베이스 접근 등 일반적인 작업을 크게 단순화해줍니다.

### 23.3.1 Newtonsoft.Json

**Newtonsoft.Json**(Json.NET)은 .NET에서 가장 인기 있는 JSON 직렬화 라이브러리입니다. 복잡한 JSON 데이터를 C# 객체로 변환하거나 그 반대 작업을 수행할 때 사용합니다.

**설치:**

```bash
dotnet add package Newtonsoft.Json
```

**기본 사용법:**

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;

// C# 객체 정의
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public List<string> Hobbies { get; set; }
}

// 객체를 JSON 문자열로 변환 (직렬화)
Person person = new Person
{
    Name = "홍길동",
    Age = 30,
    Hobbies = new List<string> { "독서", "코딩", "여행" }
};

string json = JsonConvert.SerializeObject(person, Formatting.Indented);
Console.WriteLine("JSON 출력:");
Console.WriteLine(json);

// JSON 문자열을 객체로 변환 (역직렬화)
string jsonString = @"
{
  ""Name"": ""김철수"",
  ""Age"": 25,
  ""Hobbies"": [""게임"", ""운동""]
}";

Person deserializedPerson = JsonConvert.DeserializeObject<Person>(jsonString);
Console.WriteLine($"\n역직렬화 결과: {deserializedPerson.Name}, {deserializedPerson.Age}세");

// 출력:
// JSON 출력:
// {
//   "Name": "홍길동",
//   "Age": 30,
//   "Hobbies": [
//     "독서",
//     "코딩",
//     "여행"
//   ]
// }
// 
// 역직렬화 결과: 김철수, 25세
```

**주요 기능:**

- **직렬화(Serialization)**: C# 객체 → JSON 문자열
- **역직렬화(Deserialization)**: JSON 문자열 → C# 객체
- **다양한 설정**: 날짜 형식, 들여쓰기, null 처리 등
- **LINQ to JSON**: JSON 데이터를 쿼리 가능

**System.Text.Json과의 비교:**

.NET Core 3.0부터 내장된 `System.Text.Json`도 있지만, Newtonsoft.Json은:
- 더 많은 기능과 유연성 제공
- 레거시 코드와의 호환성
- 복잡한 JSON 처리에 유리

### 23.3.2 Serilog

**Serilog**는 구조화된 로깅(Structured Logging)을 제공하는 강력한 로깅 라이브러리입니다. 애플리케이션의 실행 상태, 오류, 정보 메시지 등을 체계적으로 기록할 수 있습니다.

**설치:**

```bash
# Serilog 코어
dotnet add package Serilog

# 콘솔 출력을 위한 Sink
dotnet add package Serilog.Sinks.Console

# 파일 출력을 위한 Sink
dotnet add package Serilog.Sinks.File
```

**기본 사용법:**

```csharp
using Serilog;
using System;

// Serilog 설정
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .WriteTo.Console()
    .WriteTo.File("logs/myapp.txt", rollingInterval: RollingInterval.Day)
    .CreateLogger();

// 로그 작성
Log.Information("애플리케이션 시작됨");
Log.Debug("디버그 메시지: 변수 값 확인");
Log.Warning("경고: 리소스 사용량이 높습니다");
Log.Error("오류 발생: 파일을 찾을 수 없습니다");

// 구조화된 로깅 (변수를 포함)
string userName = "홍길동";
int loginAttempts = 3;
Log.Information("사용자 {UserName}이(가) {Attempts}번 로그인 시도", userName, loginAttempts);

// 예외 로깅
try
{
    int result = 10 / 0;
}
catch (Exception ex)
{
    Log.Error(ex, "계산 중 오류 발생");
}

// 로그 종료
Log.CloseAndFlush();

// 출력 예시:
// [10:30:15 INF] 애플리케이션 시작됨
// [10:30:15 DBG] 디버그 메시지: 변수 값 확인
// [10:30:15 WRN] 경고: 리소스 사용량이 높습니다
// [10:30:15 ERR] 오류 발생: 파일을 찾을 수 없습니다
// [10:30:15 INF] 사용자 "홍길동"이(가) 3번 로그인 시도
// [10:30:15 ERR] 계산 중 오류 발생
// System.DivideByZeroException: Attempted to divide by zero.
```

**주요 개념:**

- **로그 레벨**: Verbose, Debug, Information, Warning, Error, Fatal
- **Sink**: 로그를 출력할 대상 (콘솔, 파일, 데이터베이스 등)
- **구조화된 로깅**: 문자열 보간이 아닌 템플릿 사용으로 검색과 분석 용이

**Serilog의 장점:**

- 강력한 필터링 및 포맷팅
- 다양한 출력 대상 지원
- 성능 최적화
- 풍부한 생태계

### 23.3.3 Dapper

**Dapper**는 경량 ORM(Object-Relational Mapper)으로, SQL 데이터베이스 작업을 간소화해줍니다. Entity Framework보다 가볍고 빠르면서도, ADO.NET보다 사용하기 쉽습니다.

**설치:**

```bash
dotnet add package Dapper
dotnet add package Microsoft.Data.Sqlite
```

**기본 사용법:**

```csharp
using Dapper;
using Microsoft.Data.Sqlite;
using System;
using System.Collections.Generic;
using System.Linq;

// 데이터 모델
class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// SQLite 데이터베이스 연결
string connectionString = "Data Source=products.db";

using (var connection = new SqliteConnection(connectionString))
{
    connection.Open();
    
    // 테이블 생성
    connection.Execute(@"
        CREATE TABLE IF NOT EXISTS Products (
            Id INTEGER PRIMARY KEY AUTOINCREMENT,
            Name TEXT NOT NULL,
            Price REAL NOT NULL
        )
    ");
    
    // 데이터 삽입
    var newProduct = new Product { Name = "노트북", Price = 1500000 };
    connection.Execute(
        "INSERT INTO Products (Name, Price) VALUES (@Name, @Price)",
        newProduct
    );
    
    Console.WriteLine("제품 추가됨");
    
    // 데이터 조회 (단일 객체)
    var product = connection.QueryFirstOrDefault<Product>(
        "SELECT * FROM Products WHERE Id = @Id",
        new { Id = 1 }
    );
    
    if (product != null)
    {
        Console.WriteLine($"조회된 제품: {product.Name}, {product.Price:C}");
    }
    
    // 데이터 조회 (여러 객체)
    var products = connection.Query<Product>(
        "SELECT * FROM Products WHERE Price > @MinPrice",
        new { MinPrice = 1000000 }
    ).ToList();
    
    Console.WriteLine($"\n100만원 이상 제품 ({products.Count}개):");
    foreach (var p in products)
    {
        Console.WriteLine($"- {p.Name}: {p.Price:C}");
    }
    
    // 데이터 업데이트
    connection.Execute(
        "UPDATE Products SET Price = @Price WHERE Id = @Id",
        new { Id = 1, Price = 1450000 }
    );
    
    Console.WriteLine("\n가격 업데이트됨");
    
    // 데이터 삭제
    connection.Execute(
        "DELETE FROM Products WHERE Id = @Id",
        new { Id = 1 }
    );
    
    Console.WriteLine("제품 삭제됨");
}

// 출력:
// 제품 추가됨
// 조회된 제품: 노트북, ₩1,500,000.00
// 
// 100만원 이상 제품 (1개):
// - 노트북: ₩1,500,000.00
// 
// 가격 업데이트됨
// 제품 삭제됨
```

**주요 기능:**

- **Query<T>**: SQL 쿼리 결과를 객체 리스트로 변환
- **QueryFirstOrDefault<T>**: 첫 번째 결과 또는 기본값 반환
- **Execute**: INSERT, UPDATE, DELETE 등 실행
- **매개변수 바인딩**: SQL Injection 방지

**Dapper의 특징:**

- **Micro ORM**: 최소한의 오버헤드
- **SQL 제어**: 직접 SQL 작성으로 성능 최적화 가능
- **유연성**: 복잡한 쿼리 지원
- **빠른 속도**: Entity Framework보다 훨씬 빠름

**언제 Dapper를 사용할까?**

- 성능이 중요한 경우
- SQL을 직접 제어하고 싶을 때
- 간단한 CRUD 작업
- Entity Framework의 무거움을 피하고 싶을 때

---

## 23장 정리

이 장에서는 NuGet 패키지 관리자를 통해 .NET 생태계의 풍부한 라이브러리를 활용하는 방법을 배웠습니다.

**핵심 요약:**

✅ **NuGet의 이해**: .NET의 공식 패키지 관리자로 외부 라이브러리를 쉽게 관리

✅ **패키지 관리**: `dotnet add/remove package` 명령으로 설치와 제거

✅ **Newtonsoft.Json**: JSON 데이터 직렬화/역직렬화의 강력한 도구

✅ **Serilog**: 구조화된 로깅으로 애플리케이션 상태 추적

✅ **Dapper**: 경량 ORM으로 효율적인 데이터베이스 작업

**다음 단계:**

24장에서는 단위 테스트를 다루며, xUnit과 Moq를 사용하여 코드의 품질과 신뢰성을 높이는 방법을 학습합니다. 테스트 가능한 코드 작성은 전문 개발자의 필수 역량입니다.

**추가 학습 자료:**

- NuGet 공식 웹사이트: https://www.nuget.org
- Newtonsoft.Json 문서: https://www.newtonsoft.com/json/help/html/Introduction.htm
- Serilog 문서: https://serilog.net
- Dapper 문서: https://github.com/DapperLib/Dapper

**실습 과제:**

1. 간단한 콘솔 애플리케이션을 만들어 Newtonsoft.Json으로 JSON 파일을 읽고 쓰기
2. Serilog를 사용하여 애플리케이션의 실행 로그를 파일로 저장
3. Dapper로 간단한 도서 관리 시스템 만들기 (추가, 조회, 수정, 삭제)
