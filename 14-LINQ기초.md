# 14장. LINQ 기초

LINQ(Language Integrated Query)는 .NET 3.5와 C# 3.0에서 2007년에 도입된 혁명적인 기능으로, 데이터 쿼리를 프로그래밍 언어에 직접 통합한 획기적인 시도입니다. Microsoft Research의 Erik Meijer가 주도한 이 프로젝트는 SQL의 선언적 쿼리 스타일과 함수형 프로그래밍의 강력함을 C#에 도입하여, 컬렉션, 데이터베이스, XML 등 다양한 데이터 소스를 통합된 방식으로 다룰 수 있게 했습니다.

**LINQ의 역사적 배경:**

LINQ 이전에는 각 데이터 소스마다 서로 다른 API를 사용해야 했습니다. 메모리 상의 컬렉션은 `foreach`와 반복문으로, 데이터베이스는 SQL 문자열로, XML은 DOM API로 처리했습니다. 이러한 불일치는 코드의 복잡성을 증가시키고, 타입 안정성(Type Safety)을 저해하며, 컴파일 타임 오류 검출을 어렵게 만들었습니다. LINQ는 이 모든 데이터 소스에 대해 동일한 쿼리 문법을 제공함으로써 이 문제를 해결했습니다.

LINQ의 핵심 철학은 다음과 같습니다:

1. **통합성(Integration)**: 쿼리가 언어의 일급 구성 요소(First-class Construct)
2. **타입 안정성(Type Safety)**: 컴파일 타임에 타입 검증
3. **선언적 프로그래밍(Declarative Programming)**: "무엇을(What)" 할지 명시하고, "어떻게(How)"는 추상화
4. **지연 실행(Deferred Execution)**: 필요한 시점에만 쿼리 실행

**LINQ의 기술적 기반:**

LINQ는 다음 C# 3.0의 혁신적인 기능들을 기반으로 구축되었습니다:

- **람다 식(Lambda Expressions)**: 간결한 익명 함수 표현
- **확장 메서드(Extension Methods)**: 기존 타입에 새 메서드 추가
- **익명 타입(Anonymous Types)**: 즉석에서 타입 정의
- **표현식 트리(Expression Trees)**: 코드를 데이터로 표현
- **타입 추론(Type Inference)**: `var` 키워드를 통한 간결한 코드

## 이 장에서 배울 내용

이 장에서는 LINQ의 기초를 체계적으로 학습합니다:

- **LINQ의 개념과 원리**: LINQ가 무엇이며 왜 중요한지, 어떤 문제를 해결하는지 이해합니다.
- **쿼리 구문과 메서드 구문**: LINQ의 두 가지 표현 방식과 각각의 특징을 배웁니다.
- **기본 LINQ 연산자**: 필터링(Where), 투영(Select), 정렬(OrderBy) 등 가장 많이 사용되는 연산자들을 익힙니다.
- **집계 연산자**: Count, Sum, Average, Min, Max, Aggregate 등 데이터를 요약하는 방법을 학습합니다.

---

## 14.1 LINQ란?

LINQ(Language Integrated Query, 링크)는 C#과 .NET에 내장된 강력한 쿼리 기능으로, 다양한 데이터 소스에 대해 일관된 방식으로 쿼리를 작성할 수 있게 해줍니다. LINQ를 사용하면 SQL 스타일의 선언적 구문으로 컬렉션, 데이터베이스, XML 등을 쉽게 조회하고 변환할 수 있습니다.

**LINQ의 주요 장점:**

1. **타입 안정성**: 컴파일 타임에 오류 검출
2. **IntelliSense 지원**: 개발 생산성 향상
3. **코드 간결성**: 복잡한 데이터 처리 로직을 간단히 표현
4. **재사용성**: 쿼리를 변수에 저장하고 조합 가능
5. **성능 최적화**: 지연 실행으로 불필요한 연산 방지

**LINQ의 종류:**

- **LINQ to Objects**: 메모리 상의 컬렉션(List, Array, IEnumerable 등) 쿼리
- **LINQ to SQL / Entity Framework**: 관계형 데이터베이스 쿼리
- **LINQ to XML**: XML 문서 쿼리
- **LINQ to Entities**: Entity Framework를 통한 데이터베이스 접근
- **Parallel LINQ (PLINQ)**: 병렬 처리를 위한 LINQ

이 장에서는 LINQ to Objects를 중심으로 학습하며, 이는 모든 LINQ의 기초가 됩니다.

**전통적인 방식 vs LINQ:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// 전통적인 방식: 명령형 프로그래밍 (Imperative)
List<int> evenNumbers = new List<int>();
foreach (int num in numbers)
{
    if (num % 2 == 0)
    {
        evenNumbers.Add(num);
    }
}
Console.WriteLine("짝수: " + string.Join(", ", evenNumbers));
// 출력: 짝수: 2, 4, 6, 8, 10

// LINQ 방식: 선언적 프로그래밍 (Declarative)
var evenNumbersLinq = numbers.Where(n => n % 2 == 0);
Console.WriteLine("짝수: " + string.Join(", ", evenNumbersLinq));
// 출력: 짝수: 2, 4, 6, 8, 10
```

**지연 실행(Deferred Execution)의 이해:**

LINQ의 중요한 특성 중 하나는 대부분의 쿼리 연산자가 지연 실행됩니다. 쿼리를 정의하는 시점이 아닌, 실제로 결과를 사용하는 시점에 실행됩니다.

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 쿼리 정의 시점 - 아직 실행되지 않음
var query = numbers.Where(n => n > 3);

// 원본 데이터 수정
numbers.Add(6);
numbers.Add(7);

// 실제 실행 시점 - foreach로 열거할 때
foreach (int num in query)
{
    Console.Write($"{num} ");
}
// 출력: 4 5 6 7 (수정된 데이터 반영)

// 즉시 실행하려면 ToList(), ToArray(), Count() 등 사용
List<int> immediateResult = numbers.Where(n => n > 3).ToList();
```

**기본 예제:**

```csharp
string[] names = { "김철수", "이영희", "박민수", "최지혜", "정다은" };

// 이름 길이가 3자인 사람 찾기
var threeCharNames = names.Where(name => name.Length == 3);
Console.WriteLine("3자 이름: " + string.Join(", ", threeCharNames));
// 출력: 3자 이름: 김철수, 이영희, 박민수, 최지혜, 정다은

// '이'로 시작하는 이름 찾기
var leeNames = names.Where(name => name.StartsWith("이"));
Console.WriteLine("'이'로 시작: " + string.Join(", ", leeNames));
// 출력: '이'로 시작: 이영희
```

---

## 14.2 쿼리 구문 vs 메서드 구문

LINQ는 동일한 쿼리를 작성하는 두 가지 문법을 제공합니다: **쿼리 구문(Query Syntax)**과 **메서드 구문(Method Syntax)**입니다. 두 방식은 컴파일 시점에 동일한 코드로 변환되므로 성능 차이는 없으며, 개발자의 선호도와 상황에 따라 선택할 수 있습니다.

**쿼리 구문(Query Syntax):**

SQL과 유사한 선언적 문법으로, `from`, `where`, `select` 등의 키워드를 사용합니다. SQL에 익숙한 개발자에게 직관적이며, 복잡한 쿼리를 가독성 있게 표현할 수 있습니다.

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// 쿼리 구문
var evenNumbers = from n in numbers
                  where n % 2 == 0
                  select n;

Console.WriteLine("짝수: " + string.Join(", ", evenNumbers));
// 출력: 짝수: 2, 4, 6, 8, 10
```

**메서드 구문(Method Syntax):**

확장 메서드와 람다 식을 사용하는 함수형 스타일의 문법입니다. 더 간결하고 유연하며, 모든 LINQ 연산자를 사용할 수 있습니다.

```csharp
// 메서드 구문 (위 쿼리 구문과 동일한 결과)
var evenNumbers = numbers.Where(n => n % 2 == 0);

Console.WriteLine("짝수: " + string.Join(", ", evenNumbers));
// 출력: 짝수: 2, 4, 6, 8, 10
```

**두 구문의 비교:**

| 측면 | 쿼리 구문 | 메서드 구문 |
|------|-----------|-------------|
| 가독성 | SQL 스타일, 익숙함 | 함수형 스타일, 간결함 |
| 표현력 | 일부 연산자만 지원 | 모든 LINQ 연산자 지원 |
| 복잡한 쿼리 | 가독성 우수 | 메서드 체이닝으로 표현 |
| IntelliSense | 제한적 | 완벽한 지원 |
| 권장 사용 | 복잡한 조인, 그룹화 | 단순 쿼리, 체이닝 |

**쿼리 구문의 구조:**

```csharp
List<string> fruits = new List<string> { "사과", "바나나", "오렌지", "포도", "딸기" };

// 쿼리 구문의 기본 구조
// from: 데이터 소스 지정
// where: 필터링 조건
// orderby: 정렬
// select: 결과 투영
var query = from fruit in fruits
            where fruit.Contains("과")
            orderby fruit
            select fruit;

Console.WriteLine("'과' 포함 과일: " + string.Join(", ", query));
// 출력: '과' 포함 과일: 사과, 포도
```

**메서드 구문의 구조:**

```csharp
// 메서드 구문 (위와 동일한 쿼리)
var query = fruits
    .Where(fruit => fruit.Contains("과"))
    .OrderBy(fruit => fruit);

Console.WriteLine("'과' 포함 과일: " + string.Join(", ", query));
// 출력: '과' 포함 과일: 사과, 포도
```

**혼합 사용:**

실무에서는 두 구문을 혼합하여 사용하는 경우가 많습니다. 쿼리 구문으로 시작하여 메서드 구문으로 추가 연산을 수행할 수 있습니다.

```csharp
List<int> scores = new List<int> { 85, 92, 78, 95, 88, 73, 91 };

// 쿼리 구문 + 메서드 구문 혼합
var topScores = (from score in scores
                 where score >= 80
                 orderby score descending
                 select score)
                .Take(3);  // 메서드 구문 추가

Console.WriteLine("상위 3개 점수: " + string.Join(", ", topScores));
// 출력: 상위 3개 점수: 95, 92, 91
```

**복잡한 예제 - 객체 컬렉션:**

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
    public double GPA { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Name = "김철수", Age = 20, GPA = 3.5 },
    new Student { Name = "이영희", Age = 22, GPA = 4.0 },
    new Student { Name = "박민수", Age = 21, GPA = 3.8 },
    new Student { Name = "최지혜", Age = 19, GPA = 3.9 }
};

// 쿼리 구문
var query1 = from s in students
             where s.GPA >= 3.8
             orderby s.GPA descending
             select s.Name;

Console.WriteLine("우수 학생 (쿼리 구문): " + string.Join(", ", query1));
// 출력: 우수 학생 (쿼리 구문): 이영희, 최지혜, 박민수

// 메서드 구문
var query2 = students
    .Where(s => s.GPA >= 3.8)
    .OrderByDescending(s => s.GPA)
    .Select(s => s.Name);

Console.WriteLine("우수 학생 (메서드 구문): " + string.Join(", ", query2));
// 출력: 우수 학생 (메서드 구문): 이영희, 최지혜, 박민수
```

**선택 가이드:**

- **쿼리 구문 사용 권장**:
  - 복잡한 조인(join) 연산
  - 여러 데이터 소스를 다루는 경우
  - SQL에 익숙한 팀원들과 협업
  - 쿼리의 의도를 명확히 표현하고 싶을 때

- **메서드 구문 사용 권장**:
  - 단순한 필터링, 정렬, 투영
  - 메서드 체이닝으로 가독성을 높일 수 있는 경우
  - `Take`, `Skip`, `Distinct` 같이 쿼리 구문에 없는 연산자 사용
  - 람다 식에 익숙한 경우

---

## 14.3 기본 LINQ 연산자

LINQ의 강력함은 풍부한 연산자(Operator) 세트에서 나옵니다. 이 섹션에서는 가장 기본적이면서도 자주 사용되는 연산자들을 학습합니다. 모든 LINQ 연산자는 `IEnumerable<T>` 또는 `IQueryable<T>`에 대한 확장 메서드로 구현되어 있습니다.

### 14.3.1 Where (필터링)

`Where` 연산자는 조건(Predicate)을 만족하는 요소만 선택하는 필터링 연산자입니다. SQL의 `WHERE` 절과 동일한 역할을 하며, 가장 많이 사용되는 LINQ 연산자 중 하나입니다.

**시그니처:**
```csharp
IEnumerable<T> Where<T>(this IEnumerable<T> source, Func<T, bool> predicate)
```

**기본 예제:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// 짝수만 선택
var evenNumbers = numbers.Where(n => n % 2 == 0);
Console.WriteLine("짝수: " + string.Join(", ", evenNumbers));
// 출력: 짝수: 2, 4, 6, 8, 10

// 5보다 큰 수
var greaterThanFive = numbers.Where(n => n > 5);
Console.WriteLine("5보다 큰 수: " + string.Join(", ", greaterThanFive));
// 출력: 5보다 큰 수: 6, 7, 8, 9, 10

// 3의 배수
var multiplesOfThree = numbers.Where(n => n % 3 == 0);
Console.WriteLine("3의 배수: " + string.Join(", ", multiplesOfThree));
// 출력: 3의 배수: 3, 6, 9
```

**인덱스를 사용하는 Where:**

`Where`는 오버로드를 통해 요소의 인덱스도 함께 받을 수 있습니다.

```csharp
string[] words = { "사과", "바나나", "오렌지", "포도", "딸기" };

// 인덱스가 짝수인 위치의 요소만 선택
var evenIndexWords = words.Where((word, index) => index % 2 == 0);
Console.WriteLine("짝수 인덱스: " + string.Join(", ", evenIndexWords));
// 출력: 짝수 인덱스: 사과, 오렌지, 딸기
```

**복잡한 조건:**

```csharp
class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Stock { get; set; }
}

List<Product> products = new List<Product>
{
    new Product { Name = "노트북", Price = 1500000, Stock = 5 },
    new Product { Name = "마우스", Price = 35000, Stock = 0 },
    new Product { Name = "키보드", Price = 120000, Stock = 3 },
    new Product { Name = "모니터", Price = 300000, Stock = 2 }
};

// 여러 조건 결합
var affordableInStock = products.Where(p => p.Price < 500000 && p.Stock > 0);
foreach (var product in affordableInStock)
{
    Console.WriteLine($"{product.Name}: {product.Price:C0} (재고: {product.Stock})");
}
// 출력:
// 마우스: ₩35,000 (재고: 0) // 제외됨 - Stock이 0
// 키보드: ₩120,000 (재고: 3)
// 모니터: ₩300,000 (재고: 2)
```

**메서드 추출을 통한 재사용:**

```csharp
List<int> scores = new List<int> { 85, 92, 78, 95, 88, 73, 91, 67 };

// 술어(Predicate) 메서드로 분리
bool IsHighScore(int score) => score >= 90;
bool IsPassingScore(int score) => score >= 60;

var highScores = scores.Where(IsHighScore);
var passingScores = scores.Where(IsPassingScore);

Console.WriteLine("고득점: " + string.Join(", ", highScores));
// 출력: 고득점: 92, 95, 91

Console.WriteLine("합격 점수: " + string.Join(", ", passingScores));
// 출력: 합격 점수: 85, 92, 78, 95, 88, 73, 91, 67
```

### 14.3.2 Select (투영)

`Select` 연산자는 각 요소를 변환(Transform)하는 투영(Projection) 연산자입니다. SQL의 `SELECT` 절과 유사하며, 데이터의 형태를 변경하거나 일부만 추출할 때 사용합니다.

**시그니처:**
```csharp
IEnumerable<TResult> Select<T, TResult>(this IEnumerable<T> source, Func<T, TResult> selector)
```

**기본 예제:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 각 숫자를 제곱
var squares = numbers.Select(n => n * n);
Console.WriteLine("제곱: " + string.Join(", ", squares));
// 출력: 제곱: 1, 4, 9, 16, 25

// 각 숫자를 문자열로 변환
var strings = numbers.Select(n => n.ToString());
Console.WriteLine("문자열: " + string.Join(", ", strings));
// 출력: 문자열: 1, 2, 3, 4, 5

// 각 숫자에 단위 추가
var withUnit = numbers.Select(n => $"{n}개");
Console.WriteLine("단위 추가: " + string.Join(", ", withUnit));
// 출력: 단위 추가: 1개, 2개, 3개, 4개, 5개
```

**객체의 특정 속성 선택:**

```csharp
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string City { get; set; }
}

List<Person> people = new List<Person>
{
    new Person { Name = "김철수", Age = 25, City = "서울" },
    new Person { Name = "이영희", Age = 30, City = "부산" },
    new Person { Name = "박민수", Age = 28, City = "대구" }
};

// 이름만 추출
var names = people.Select(p => p.Name);
Console.WriteLine("이름: " + string.Join(", ", names));
// 출력: 이름: 김철수, 이영희, 박민수

// 나이만 추출
var ages = people.Select(p => p.Age);
Console.WriteLine("나이: " + string.Join(", ", ages));
// 출력: 나이: 25, 30, 28
```

**익명 타입으로 투영:**

```csharp
// 여러 속성을 선택하여 새 객체 생성
var summary = people.Select(p => new
{
    FullName = p.Name,
    Info = $"{p.Age}세, {p.City}"
});

foreach (var item in summary)
{
    Console.WriteLine($"{item.FullName}: {item.Info}");
}
// 출력:
// 김철수: 25세, 서울
// 이영희: 30세, 부산
// 박민수: 28세, 대구
```

**인덱스를 사용하는 Select:**

```csharp
string[] fruits = { "사과", "바나나", "오렌지" };

// 인덱스와 함께 변환
var indexed = fruits.Select((fruit, index) => $"{index + 1}. {fruit}");
Console.WriteLine(string.Join("\n", indexed));
// 출력:
// 1. 사과
// 2. 바나나
// 3. 오렌지
```

**Where와 Select 조합:**

```csharp
List<int> scores = new List<int> { 85, 92, 78, 95, 88, 73, 91, 67 };

// 90점 이상인 점수를 등급으로 변환
var highGrades = scores
    .Where(s => s >= 90)
    .Select(s => $"{s}점 (A등급)");

Console.WriteLine(string.Join("\n", highGrades));
// 출력:
// 92점 (A등급)
// 95점 (A등급)
// 91점 (A등급)
```

**복잡한 변환:**

```csharp
class Order
{
    public int OrderId { get; set; }
    public string Product { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
}

List<Order> orders = new List<Order>
{
    new Order { OrderId = 1, Product = "노트북", Quantity = 2, UnitPrice = 1500000 },
    new Order { OrderId = 2, Product = "마우스", Quantity = 5, UnitPrice = 35000 },
    new Order { OrderId = 3, Product = "키보드", Quantity = 3, UnitPrice = 120000 }
};

// 총액 계산 및 새 형태로 투영
var orderSummary = orders.Select(o => new
{
    o.OrderId,
    o.Product,
    TotalPrice = o.Quantity * o.UnitPrice
});

foreach (var order in orderSummary)
{
    Console.WriteLine($"주문 #{order.OrderId}: {order.Product} - {order.TotalPrice:C0}");
}
// 출력:
// 주문 #1: 노트북 - ₩3,000,000
// 주문 #2: 마우스 - ₩175,000
// 주문 #3: 키보드 - ₩360,000
```

### 14.3.3 OrderBy / OrderByDescending (정렬)

`OrderBy`와 `OrderByDescending` 연산자는 요소를 정렬하는 연산자입니다. SQL의 `ORDER BY` 절과 동일한 역할을 하며, 안정 정렬(Stable Sort) 알고리즘을 사용합니다.

**시그니처:**
```csharp
IOrderedEnumerable<T> OrderBy<T, TKey>(this IEnumerable<T> source, Func<T, TKey> keySelector)
IOrderedEnumerable<T> OrderByDescending<T, TKey>(this IEnumerable<T> source, Func<T, TKey> keySelector)
```

**기본 예제:**

```csharp
List<int> numbers = new List<int> { 5, 2, 8, 1, 9, 3, 7, 4, 6 };

// 오름차순 정렬
var ascending = numbers.OrderBy(n => n);
Console.WriteLine("오름차순: " + string.Join(", ", ascending));
// 출력: 오름차순: 1, 2, 3, 4, 5, 6, 7, 8, 9

// 내림차순 정렬
var descending = numbers.OrderByDescending(n => n);
Console.WriteLine("내림차순: " + string.Join(", ", descending));
// 출력: 내림차순: 9, 8, 7, 6, 5, 4, 3, 2, 1
```

**문자열 정렬:**

```csharp
string[] names = { "이영희", "김철수", "박민수", "최지혜" };

// 가나다순 정렬
var sorted = names.OrderBy(name => name);
Console.WriteLine("가나다순: " + string.Join(", ", sorted));
// 출력: 가나다순: 김철수, 박민수, 이영희, 최지혜

// 길이순 정렬
var byLength = names.OrderBy(name => name.Length);
Console.WriteLine("길이순: " + string.Join(", ", byLength));
// 출력: 길이순: 김철수, 이영희, 박민수, 최지혜
```

**객체 속성으로 정렬:**

```csharp
class Student
{
    public string Name { get; set; }
    public int Score { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Name = "김철수", Score = 85 },
    new Student { Name = "이영희", Score = 92 },
    new Student { Name = "박민수", Score = 78 },
    new Student { Name = "최지혜", Score = 95 }
};

// 점수순 정렬
var byScore = students.OrderByDescending(s => s.Score);
Console.WriteLine("점수순:");
foreach (var student in byScore)
{
    Console.WriteLine($"{student.Name}: {student.Score}점");
}
// 출력:
// 최지혜: 95점
// 이영희: 92점
// 김철수: 85점
// 박민수: 78점

// 이름순 정렬
var byName = students.OrderBy(s => s.Name);
Console.WriteLine("\n이름순:");
foreach (var student in byName)
{
    Console.WriteLine($"{student.Name}: {student.Score}점");
}
// 출력:
// 김철수: 85점
// 박민수: 78점
// 이영희: 92점
// 최지혜: 95점
```

**다중 정렬 (ThenBy / ThenByDescending):**

복잡한 정렬 규칙을 적용할 때는 `ThenBy`와 `ThenByDescending`을 사용합니다.

```csharp
class Employee
{
    public string Department { get; set; }
    public string Name { get; set; }
    public int Salary { get; set; }
}

List<Employee> employees = new List<Employee>
{
    new Employee { Department = "개발", Name = "김철수", Salary = 5000 },
    new Employee { Department = "개발", Name = "이영희", Salary = 6000 },
    new Employee { Department = "영업", Name = "박민수", Salary = 4500 },
    new Employee { Department = "영업", Name = "최지혜", Salary = 5500 },
    new Employee { Department = "개발", Name = "정다은", Salary = 5000 }
};

// 부서별로 먼저 정렬하고, 같은 부서 내에서는 연봉순으로 정렬
var sorted = employees
    .OrderBy(e => e.Department)
    .ThenByDescending(e => e.Salary)
    .ThenBy(e => e.Name);  // 연봉이 같으면 이름순

Console.WriteLine("부서별 > 연봉순 > 이름순:");
foreach (var emp in sorted)
{
    Console.WriteLine($"{emp.Department} - {emp.Name}: {emp.Salary:C0}");
}
// 출력:
// 개발 - 이영희: ₩6,000
// 개발 - 김철수: ₩5,000
// 개발 - 정다은: ₩5,000
// 영업 - 최지혜: ₩5,500
// 영업 - 박민수: ₩4,500
```

**안정 정렬의 중요성:**

LINQ의 `OrderBy`는 안정 정렬(Stable Sort)을 보장합니다. 즉, 키 값이 같은 요소들의 상대적 순서가 보존됩니다.

```csharp
var data = new[]
{
    new { Name = "Alice", Score = 85 },
    new { Name = "Bob", Score = 90 },
    new { Name = "Charlie", Score = 85 },
    new { Name = "David", Score = 90 }
};

// Score로 정렬하되, 같은 점수 내에서는 원래 순서 유지
var sorted = data.OrderBy(x => x.Score);
foreach (var item in sorted)
{
    Console.WriteLine($"{item.Name}: {item.Score}");
}
// 출력:
// Alice: 85
// Charlie: 85    // Alice와 Charlie의 원래 순서 유지
// Bob: 90
// David: 90      // Bob과 David의 원래 순서 유지
```

---

## 14.4 집계 연산자

집계 연산자(Aggregation Operators)는 컬렉션의 데이터를 하나의 값으로 요약하는 연산자들입니다. SQL의 집계 함수(Aggregate Functions)와 동일한 역할을 하며, 통계 분석과 데이터 요약에 필수적입니다. 이러한 연산자들은 즉시 실행(Immediate Execution)되어 결과를 반환합니다.

### 14.4.1 Count, Sum, Average

**Count - 요소 개수 세기:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// 전체 개수
int totalCount = numbers.Count();
Console.WriteLine($"전체 개수: {totalCount}");
// 출력: 전체 개수: 10

// 조건을 만족하는 요소 개수
int evenCount = numbers.Count(n => n % 2 == 0);
Console.WriteLine($"짝수 개수: {evenCount}");
// 출력: 짝수 개수: 5

int greaterThanFive = numbers.Count(n => n > 5);
Console.WriteLine($"5보다 큰 수 개수: {greaterThanFive}");
// 출력: 5보다 큰 수 개수: 5
```

**Sum - 합계 계산:**

```csharp
// 전체 합계
int sum = numbers.Sum();
Console.WriteLine($"전체 합계: {sum}");
// 출력: 전체 합계: 55

// 조건을 만족하는 요소의 합계
int evenSum = numbers.Where(n => n % 2 == 0).Sum();
Console.WriteLine($"짝수 합계: {evenSum}");
// 출력: 짝수 합계: 30
```

**Average - 평균 계산:**

```csharp
// 전체 평균
double average = numbers.Average();
Console.WriteLine($"전체 평균: {average}");
// 출력: 전체 평균: 5.5

// 조건을 만족하는 요소의 평균
double oddAverage = numbers.Where(n => n % 2 != 0).Average();
Console.WriteLine($"홀수 평균: {oddAverage}");
// 출력: 홀수 평균: 5
```

**객체 컬렉션에서의 집계:**

```csharp
class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Quantity { get; set; }
}

List<Product> products = new List<Product>
{
    new Product { Name = "노트북", Price = 1500000, Quantity = 3 },
    new Product { Name = "마우스", Price = 35000, Quantity = 10 },
    new Product { Name = "키보드", Price = 120000, Quantity = 5 }
};

// 상품 개수
int productCount = products.Count();
Console.WriteLine($"총 상품 종류: {productCount}");
// 출력: 총 상품 종류: 3

// 총 재고 수량
int totalQuantity = products.Sum(p => p.Quantity);
Console.WriteLine($"총 재고: {totalQuantity}개");
// 출력: 총 재고: 18개

// 평균 가격
decimal avgPrice = products.Average(p => p.Price);
Console.WriteLine($"평균 가격: {avgPrice:C0}");
// 출력: 평균 가격: ₩551,667

// 총 재고 가치 (가격 × 수량의 합)
decimal totalValue = products.Sum(p => p.Price * p.Quantity);
Console.WriteLine($"총 재고 가치: {totalValue:C0}");
// 출력: 총 재고 가치: ₩5,450,000
```

**LongCount - 큰 컬렉션:**

매우 큰 컬렉션(int 범위 초과)의 경우 `LongCount`를 사용합니다.

```csharp
// int 범위: -2,147,483,648 ~ 2,147,483,647
// long 범위: -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807

long hugeCount = Enumerable.Range(1, int.MaxValue / 2).LongCount();
Console.WriteLine($"큰 컬렉션 개수: {hugeCount:N0}");
// 출력: 큰 컬렉션 개수: 1,073,741,823
```

### 14.4.2 Min, Max

`Min`과 `Max` 연산자는 컬렉션에서 최소값과 최대값을 찾습니다.

**기본 예제:**

```csharp
List<int> numbers = new List<int> { 5, 2, 8, 1, 9, 3, 7 };

// 최소값
int min = numbers.Min();
Console.WriteLine($"최소값: {min}");
// 출력: 최소값: 1

// 최대값
int max = numbers.Max();
Console.WriteLine($"최대값: {max}");
// 출력: 최대값: 9
```

**객체 속성의 Min, Max:**

```csharp
class Student
{
    public string Name { get; set; }
    public int Score { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Name = "김철수", Score = 85 },
    new Student { Name = "이영희", Score = 92 },
    new Student { Name = "박민수", Score = 78 },
    new Student { Name = "최지혜", Score = 95 }
};

// 최고 점수
int maxScore = students.Max(s => s.Score);
Console.WriteLine($"최고 점수: {maxScore}");
// 출력: 최고 점수: 95

// 최저 점수
int minScore = students.Min(s => s.Score);
Console.WriteLine($"최저 점수: {minScore}");
// 출력: 최저 점수: 78

// 최고 점수를 받은 학생 찾기
var topStudent = students.First(s => s.Score == maxScore);
Console.WriteLine($"최우수 학생: {topStudent.Name} ({topStudent.Score}점)");
// 출력: 최우수 학생: 최지혜 (95점)

// 또는 OrderBy로 찾기
var topStudent2 = students.OrderByDescending(s => s.Score).First();
Console.WriteLine($"최우수 학생: {topStudent2.Name} ({topStudent2.Score}점)");
// 출력: 최우수 학생: 최지혜 (95점)
```

**날짜 비교:**

```csharp
class Event
{
    public string Name { get; set; }
    public DateTime Date { get; set; }
}

List<Event> events = new List<Event>
{
    new Event { Name = "회의", Date = new DateTime(2024, 11, 15) },
    new Event { Name = "세미나", Date = new DateTime(2024, 11, 10) },
    new Event { Name = "워크샵", Date = new DateTime(2024, 11, 20) }
};

// 가장 빠른 날짜
DateTime earliest = events.Min(e => e.Date);
Console.WriteLine($"가장 빠른 일정: {earliest:yyyy-MM-dd}");
// 출력: 가장 빠른 일정: 2024-11-10

// 가장 늦은 날짜
DateTime latest = events.Max(e => e.Date);
Console.WriteLine($"가장 늦은 일정: {latest:yyyy-MM-dd}");
// 출력: 가장 늦은 일정: 2024-11-20

// 가장 빠른 일정 찾기
var earliestEvent = events.First(e => e.Date == earliest);
Console.WriteLine($"다가오는 일정: {earliestEvent.Name} ({earliestEvent.Date:yyyy-MM-dd})");
// 출력: 다가오는 일정: 세미나 (2024-11-10)
```

### 14.4.3 Aggregate

`Aggregate` 연산자는 가장 범용적인 집계 연산자로, 사용자 정의 집계 로직을 구현할 수 있습니다. 함수형 프로그래밍의 `reduce` 또는 `fold` 연산과 동일한 개념입니다.

**시그니처:**
```csharp
TSource Aggregate<TSource>(this IEnumerable<TSource> source, Func<TSource, TSource, TAccumulate> func)
TAccumulate Aggregate<TSource, TAccumulate>(this IEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func)
```

**기본 예제 - 합계:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// Aggregate로 합계 계산
int sum = numbers.Aggregate((acc, n) => acc + n);
Console.WriteLine($"합계 (Aggregate): {sum}");
// 출력: 합계 (Aggregate): 15

// 동작 과정:
// 1단계: 1 + 2 = 3
// 2단계: 3 + 3 = 6
// 3단계: 6 + 4 = 10
// 4단계: 10 + 5 = 15
```

**초기값(Seed) 사용:**

```csharp
// 초기값 100부터 시작
int sumWithSeed = numbers.Aggregate(100, (acc, n) => acc + n);
Console.WriteLine($"초기값 100 + 합계: {sumWithSeed}");
// 출력: 초기값 100 + 합계: 115

// 곱셈
int product = numbers.Aggregate(1, (acc, n) => acc * n);
Console.WriteLine($"곱셈: {product}");
// 출력: 곱셈: 120 (1 × 2 × 3 × 4 × 5)
```

**문자열 결합:**

```csharp
string[] words = { "LINQ", "는", "강력하다" };

// 단어 결합
string sentence = words.Aggregate((acc, word) => acc + " " + word);
Console.WriteLine(sentence);
// 출력: LINQ 는 강력하다

// 초기값과 함께
string prefixed = words.Aggregate("C#의 ", (acc, word) => acc + word + " ");
Console.WriteLine(prefixed);
// 출력: C#의 LINQ 는 강력하다
```

**복잡한 계산:**

```csharp
class Transaction
{
    public string Type { get; set; }  // "입금" 또는 "출금"
    public decimal Amount { get; set; }
}

List<Transaction> transactions = new List<Transaction>
{
    new Transaction { Type = "입금", Amount = 100000 },
    new Transaction { Type = "출금", Amount = 30000 },
    new Transaction { Type = "입금", Amount = 50000 },
    new Transaction { Type = "출금", Amount = 20000 }
};

// 최종 잔액 계산
decimal balance = transactions.Aggregate(
    0m,  // 초기 잔액
    (acc, t) => t.Type == "입금" ? acc + t.Amount : acc - t.Amount
);

Console.WriteLine($"최종 잔액: {balance:C0}");
// 출력: 최종 잔액: ₩100,000

// 동작 과정:
// 초기: 0
// 1. 입금 100000: 0 + 100000 = 100000
// 2. 출금 30000: 100000 - 30000 = 70000
// 3. 입금 50000: 70000 + 50000 = 120000
// 4. 출금 20000: 120000 - 20000 = 100000
```

**결과 변환 (Result Selector):**

```csharp
List<int> nums = new List<int> { 2, 3, 4 };

// 곱셈 후 문자열로 변환
string result = nums.Aggregate(
    1,                                    // 초기값
    (acc, n) => acc * n,                  // 집계 함수
    finalResult => $"결과: {finalResult}" // 결과 변환
);

Console.WriteLine(result);
// 출력: 결과: 24
```

**실용적인 예제 - 통계 계산:**

```csharp
List<int> scores = new List<int> { 85, 92, 78, 95, 88, 73, 91, 67 };

// 여러 통계를 한 번에 계산
var stats = scores.Aggregate(
    new { Count = 0, Sum = 0, Min = int.MaxValue, Max = int.MinValue },
    (acc, score) => new
    {
        Count = acc.Count + 1,
        Sum = acc.Sum + score,
        Min = Math.Min(acc.Min, score),
        Max = Math.Max(acc.Max, score)
    },
    acc => new
    {
        acc.Count,
        acc.Sum,
        acc.Min,
        acc.Max,
        Average = (double)acc.Sum / acc.Count
    }
);

Console.WriteLine($"개수: {stats.Count}");
Console.WriteLine($"합계: {stats.Sum}");
Console.WriteLine($"최소: {stats.Min}");
Console.WriteLine($"최대: {stats.Max}");
Console.WriteLine($"평균: {stats.Average:F2}");
// 출력:
// 개수: 8
// 합계: 669
// 최소: 67
// 최대: 95
// 평균: 83.63
```

---

## 마무리

이 장에서는 LINQ의 기초를 체계적으로 학습했습니다. LINQ는 C# 프로그래밍에서 데이터를 다루는 방식을 혁신적으로 변화시킨 기능으로, 선언적 프로그래밍 스타일로 복잡한 데이터 처리를 간결하게 표현할 수 있게 해줍니다.

**학습 요약:**

✅ **LINQ의 개념**: Language Integrated Query의 의미와 중요성, 다양한 데이터 소스를 통합된 방식으로 다루는 방법

✅ **쿼리 구문과 메서드 구문**: 두 가지 LINQ 표현 방식의 차이점과 적절한 사용 시나리오

✅ **기본 연산자**: 
- `Where`: 조건에 맞는 요소 필터링
- `Select`: 요소를 다른 형태로 변환 (투영)
- `OrderBy` / `OrderByDescending`: 요소 정렬

✅ **집계 연산자**:
- `Count`: 요소 개수 세기
- `Sum`: 합계 계산
- `Average`: 평균 계산
- `Min` / `Max`: 최소값과 최대값
- `Aggregate`: 사용자 정의 집계 로직

**실무 활용 포인트:**

1. **지연 실행의 이해**: 대부분의 LINQ 연산은 지연 실행되므로, 실제로 결과를 사용할 때까지 실행되지 않습니다. `ToList()`, `ToArray()`, `Count()` 등으로 즉시 실행할 수 있습니다.

2. **메서드 체이닝**: LINQ 연산자들을 연속적으로 연결하여 복잡한 데이터 처리 파이프라인을 구축할 수 있습니다.

3. **가독성과 유지보수**: LINQ는 코드의 의도를 명확하게 표현하여 가독성과 유지보수성을 향상시킵니다.

4. **성능 고려**: 복잡한 LINQ 쿼리는 성능에 영향을 줄 수 있으므로, 필요시 프로파일링하고 최적화해야 합니다.

**다음 단계:**

15장에서는 LINQ의 고급 기능들을 학습하게 됩니다:
- 조인 연산자 (Join, GroupJoin)
- 그룹화 (GroupBy)
- 집합 연산자 (Distinct, Union, Intersect, Except)
- 지연 실행과 즉시 실행의 심화 이해

LINQ는 현대 C# 프로그래밍의 핵심 기술이므로, 이 장에서 배운 기초를 확실히 익히고 다음 장으로 나아가시기 바랍니다.

**실습 과제:**

1. 정수 리스트에서 짝수만 선택하여 제곱한 후, 큰 순서대로 정렬하는 LINQ 쿼리 작성
2. 문자열 배열에서 길이가 5 이상인 단어의 개수와 평균 길이 계산
3. 학생 리스트에서 평균 점수가 80점 이상인 학생들의 이름을 알파벳순으로 정렬
4. `Aggregate`를 사용하여 숫자 리스트의 팩토리얼 계산 구현
