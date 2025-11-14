# 15장. 고급 LINQ

LINQ(Language Integrated Query)는 2007년 C# 3.0과 함께 등장한 혁명적인 기능으로, 프로그래밍 언어에 쿼리 기능을 통합한 획기적인 개념입니다. 이는 단순히 데이터를 조회하는 문법을 추가한 것이 아니라, SQL의 선언적 쿼리 패러다임을 객체지향 언어에 융합한 언어 설계의 혁신입니다. Microsoft의 수석 아키텍트 Anders Hejlsberg가 이끈 이 프로젝트는, 1970년대 Edgar F. Codd가 제안한 관계 대수(Relational Algebra)의 수학적 기반을 현대 프로그래밍 언어에 구현하여, 개발자들이 메모리 상의 객체, 데이터베이스, XML 문서 등 다양한 데이터 소스를 일관된 방식으로 쿼리할 수 있게 만들었습니다.

14장에서 LINQ의 기본 연산자(`Where`, `Select`, `OrderBy`)를 다루었다면, 이 장에서는 SQL의 고급 기능에 대응하는 복잡한 연산자들과 LINQ의 실행 메커니즘을 심층적으로 탐구합니다. 조인(Join)은 관계형 데이터베이스 이론의 핵심 개념으로, E.F. Codd의 관계 대수에서 유래한 연산입니다. 그룹화(GroupBy)는 데이터 분석의 필수 도구로, OLAP(Online Analytical Processing) 큐브와 같은 다차원 분석의 기반이 됩니다. 집합 연산(Set Operations)은 Georg Cantor의 집합론(Set Theory)에 기반한 수학적 연산을 프로그래밍에 적용한 것입니다.

특히 LINQ의 **지연 실행(Deferred Execution)** 모델은 함수형 프로그래밍의 **지연 평가(Lazy Evaluation)** 개념을 구현한 것으로, Haskell이나 Scala 같은 함수형 언어의 철학을 C#에 도입했습니다. 이는 단순히 성능 최적화를 넘어서, 무한 시퀀스(Infinite Sequences)의 처리, 표현식 트리(Expression Trees)를 통한 쿼리 변환, 그리고 데이터베이스 쿼리의 최적화까지 가능하게 하는 강력한 메커니즘입니다.

**LINQ의 설계 철학과 이론적 배경:**

LINQ는 다음과 같은 컴퓨터 과학의 근본 원리들을 통합합니다:

1. **관계 대수(Relational Algebra)**: SQL의 이론적 기반으로, 집합론에 기반한 데이터 조작 연산
2. **함수형 프로그래밍(Functional Programming)**: 순수 함수, 고차 함수, 불변성, 함수 합성
3. **표현식 트리(Expression Trees)**: 코드를 데이터로 표현하여 런타임 분석 및 변환 가능
4. **모나드 패턴(Monad Pattern)**: 함수형 프로그래밍의 핵심 추상화, LINQ의 메서드 체이닝 기반
5. **반복자 패턴(Iterator Pattern)**: `IEnumerable<T>`와 `IEnumerator<T>`를 통한 지연 실행

**LINQ의 아키텍처 계층:**

```
┌─────────────────────────────────────────┐
│   LINQ Query Syntax (쿼리 구문)          │
│   from x in source where ... select ... │
├─────────────────────────────────────────┤
│   LINQ Method Syntax (메서드 구문)       │
│   source.Where(...).Select(...)         │
├─────────────────────────────────────────┤
│   Standard Query Operators              │
│   (표준 쿼리 연산자)                     │
├─────────────────────────────────────────┤
│   IEnumerable<T> / IQueryable<T>        │
├─────────────────────────────────────────┤
│   LINQ Providers                        │
│   LINQ to Objects / SQL / XML / ...    │
└─────────────────────────────────────────┘
```

LINQ의 진정한 힘은 **LINQ Provider** 아키텍처에 있습니다. 동일한 쿼리 구문이 다양한 데이터 소스에 대해 작동하며, 각 Provider는 쿼리를 해당 데이터 소스에 최적화된 형태로 변환합니다. LINQ to Objects는 메모리 상의 객체를 직접 조작하고, LINQ to SQL/Entity Framework는 SQL 쿼리로 변환하며, LINQ to XML은 XPath로 변환합니다. 이러한 추상화는 Gang of Four의 **전략 패턴(Strategy Pattern)**과 **방문자 패턴(Visitor Pattern)**을 응용한 우아한 설계입니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 LINQ의 고급 개념과 내부 메커니즘을 체계적으로 학습하게 됩니다:

- **조인 연산자의 심층 이해**: 관계 대수의 조인 이론부터 시작하여, `Join`(INNER JOIN)과 `GroupJoin`(LEFT OUTER JOIN)의 내부 구현을 학습합니다. 해시 조인(Hash Join) 알고리즘의 O(n+m) 시간 복잡도, 쿼리 구문과 메서드 구문의 차이, 그리고 복합 키를 사용한 다중 조인 패턴을 익힙니다.

- **그룹화의 원리와 활용**: SQL의 GROUP BY와 HAVING 절에 대응하는 `GroupBy` 연산자를 마스터합니다. `IGrouping<TKey, TElement>` 인터페이스의 구조, 키 선택자와 요소 선택자의 역할, 그리고 OLAP 큐브와 같은 다차원 그룹화 기법을 학습합니다.

- **집합 연산자와 집합론**: Cantor의 집합론에 기반한 `Distinct`, `Union`, `Intersect`, `Except` 연산자를 이해합니다. 내부적으로 사용되는 해시 세트(HashSet) 알고리즘, 동등성 비교자(IEqualityComparer)의 역할, 그리고 수학적 집합 연산의 프로그래밍적 구현을 학습합니다.

- **지연 실행 모델의 완벽 이해**: Haskell의 지연 평가에서 영향을 받은 LINQ의 실행 모델을 깊이 탐구합니다. 반복자 패턴, `yield return`의 내부 메커니즘, 상태 머신(State Machine) 변환, 그리고 표현식 트리를 통한 쿼리 최적화를 이해합니다.

- **즉시 실행과 성능 최적화**: `ToList()`, `ToArray()`, `Count()` 등 즉시 실행되는 연산자의 특성을 이해하고, 지연 실행과의 trade-off를 분석합니다. 쿼리 실행 계획(Query Execution Plan), 메모리 vs 계산의 균형, 그리고 실무에서의 최적화 전략을 학습합니다.

**학습 목표:**
- 관계 대수와 SQL 개념의 LINQ 구현 이해
- 지연 실행 모델의 내부 메커니즘과 성능 특성 분석
- 복잡한 데이터 변환 파이프라인 설계 능력 배양
- 표현식 트리와 LINQ Provider 아키텍처 이해
- 실무에서의 쿼리 최적화 및 디버깅 기법 습득

---

## 15.1 조인 연산자

조인(Join) 연산은 관계형 데이터베이스 이론의 핵심 개념으로, 1970년 Edgar F. Codd가 제안한 관계 대수(Relational Algebra)의 기본 연산 중 하나입니다. 수학적으로 조인은 두 개 이상의 관계(Relation)를 공통 속성(Common Attribute)을 기준으로 결합하여 새로운 관계를 생성하는 이항 연산(Binary Operation)입니다. LINQ의 조인 연산자는 이러한 수학적 개념을 C#에 구현한 것으로, SQL의 JOIN 절과 동일한 기능을 제공하면서도 강력한 타입 안정성(Type Safety)을 보장합니다.

**조인의 역사적 맥락:**

조인 연산의 개념은 데이터베이스 설계의 정규화(Normalization) 원칙과 밀접하게 연결되어 있습니다. Codd의 정규화 이론은 데이터의 중복을 최소화하고 무결성을 보장하기 위해 데이터를 여러 테이블로 분산 저장하도록 권장합니다. 이렇게 분산된 데이터를 다시 결합하여 의미 있는 정보를 도출하는 것이 조인의 역할입니다. LINQ는 이러한 관계형 개념을 객체 지향 프로그래밍에 적용하여, 메모리 상의 객체 컬렉션에 대해서도 동일한 조인 연산을 수행할 수 있게 합니다.

**조인의 내부 알고리즘과 성능 특성:**

LINQ to Objects의 `Join` 메서드는 내부적으로 **해시 조인(Hash Join)** 알고리즘을 사용합니다. 이는 데이터베이스 시스템에서도 널리 사용되는 효율적인 조인 알고리즘으로, 다음과 같이 작동합니다:

1. **빌드 단계(Build Phase)**: 작은 쪽 시퀀스(일반적으로 내부 시퀀스)의 키를 해시 테이블에 저장 (O(m))
2. **프로브 단계(Probe Phase)**: 큰 쪽 시퀀스의 각 요소에 대해 해시 테이블을 조회하여 일치하는 키 찾기 (O(n))
3. **결과 생성**: 일치하는 키를 가진 요소 쌍을 결과 선택자로 변환

전체 시간 복잡도는 **O(n + m)**으로, 중첩 루프 조인(Nested Loop Join)의 O(n × m)보다 훨씬 효율적입니다. 공간 복잡도는 O(m)으로, 내부 시퀀스를 해시 테이블에 저장하는 데 필요한 메모리입니다.

**조인 연산자의 종류와 SQL 대응:**

| LINQ 메서드 | SQL 조인 | 특성 | 사용 시나리오 |
|------------|---------|------|--------------|
| `Join` | INNER JOIN | 양쪽 모두 일치하는 키가 있어야 결과에 포함 | 필수 관계 (학생-과목 등록) |
| `GroupJoin` | LEFT OUTER JOIN | 외부 시퀀스의 모든 요소 포함 | 선택적 관계 (부서-직원) |
| `SelectMany` + `DefaultIfEmpty` | RIGHT/FULL OUTER JOIN | 양쪽 또는 한쪽 시퀀스의 모든 요소 포함 | 복잡한 관계 분석 |

LINQ는 INNER JOIN과 LEFT OUTER JOIN을 직접 지원하며, RIGHT/FULL OUTER JOIN은 시퀀스 순서를 바꾸거나 조합하여 구현할 수 있습니다.

### 15.1.1 Join

`Join` 메서드는 두 시퀀스에서 키가 일치하는 요소들을 결합하여 새로운 시퀀스를 생성합니다. 이는 SQL의 **INNER JOIN**과 동일한 개념으로, 관계 대수의 **자연 조인(Natural Join)** 또는 **동등 조인(Equi-Join)**에 해당합니다. 수학적으로는 두 집합의 카티전 곱(Cartesian Product)에서 조인 조건을 만족하는 튜플만 선택하는 연산입니다.

**기본 구조:**

```csharp
var result = outer.Join(
    inner,                          // 결합할 두 번째 시퀀스
    outerKeySelector,               // 외부 시퀀스의 키 선택자
    innerKeySelector,               // 내부 시퀀스의 키 선택자
    resultSelector                  // 결과 선택자
);
```

**간단한 예제:**

```csharp
// 학생 정보
class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int CourseId { get; set; }
}

// 과목 정보
class Course
{
    public int Id { get; set; }
    public string CourseName { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Id = 1, Name = "김철수", CourseId = 101 },
    new Student { Id = 2, Name = "이영희", CourseId = 102 },
    new Student { Id = 3, Name = "박민수", CourseId = 101 }
};

List<Course> courses = new List<Course>
{
    new Course { Id = 101, CourseName = "C# 프로그래밍" },
    new Course { Id = 102, CourseName = "데이터베이스" },
    new Course { Id = 103, CourseName = "웹 개발" }
};

// Join 사용: 학생과 과목을 결합
var studentCourses = students.Join(
    courses,
    student => student.CourseId,    // 외부 키
    course => course.Id,            // 내부 키
    (student, course) => new        // 결과 생성
    {
        StudentName = student.Name,
        CourseName = course.CourseName
    }
);

foreach (var sc in studentCourses)
{
    Console.WriteLine($"{sc.StudentName} - {sc.CourseName}");
}
// 출력:
// 김철수 - C# 프로그래밍
// 이영희 - 데이터베이스
// 박민수 - C# 프로그래밍
```

**Join의 내부 메커니즘 상세 분석:**

`Join` 메서드는 지연 실행(Deferred Execution)되는 연산자입니다. 실제로 쿼리가 열거될 때, 다음과 같은 단계로 실행됩니다:

1. **해시 테이블 구축**: 내부 시퀀스(`courses`)의 모든 요소를 열거하며, 각 요소의 키를 해시 함수로 계산하여 `Lookup<TKey, TElement>` 구조에 저장합니다. 이는 `Dictionary<TKey, List<TElement>>`와 유사하지만, 동일한 키에 여러 값이 연결될 수 있는 다대다 매핑을 지원합니다.

2. **프로브 및 결과 생성**: 외부 시퀀스(`students`)의 각 요소를 순회하면서, 해당 요소의 키로 Lookup을 조회합니다. 일치하는 내부 요소가 있으면 결과 선택자를 호출하여 결과 객체를 생성하고 yield return합니다.

이러한 메커니즘 덕분에 Join은 선형 시간 복잡도 O(n+m)를 달성할 수 있습니다. 만약 중첩 루프로 구현했다면 O(n×m)이 되어, 데이터가 커질수록 성능이 급격히 저하됩니다.

**쿼리 구문과 메서드 구문의 컴파일:**

LINQ 쿼리 구문은 컴파일 시점에 메서드 구문으로 변환됩니다. 이는 C# 컴파일러의 **쿼리 표현식 변환(Query Expression Translation)** 단계에서 수행됩니다:

```csharp
// 쿼리 구문 (개발자가 작성)
var studentCourses2 = from student in students
                      join course in courses
                      on student.CourseId equals course.Id
                      select new
                      {
                          StudentName = student.Name,
                          CourseName = course.CourseName
                      };

// 컴파일러가 변환한 메서드 구문 (실제 IL 코드)
var studentCourses2 = students.Join(
    courses,
    student => student.CourseId,
    course => course.Id,
    (student, course) => new { StudentName = student.Name, CourseName = course.CourseName }
);
```

쿼리 구문의 장점은 SQL과 유사한 가독성이지만, 메서드 구문은 더 많은 LINQ 연산자를 사용할 수 있고 람다 식의 강력함을 활용할 수 있습니다. 실무에서는 상황에 따라 적절히 선택합니다.

**복합 키를 사용한 조인:**

여러 속성을 조합하여 조인 키를 만들 수 있습니다. 이를 위해 익명 타입(Anonymous Type)을 사용하며, 컴파일러는 자동으로 `Equals()`와 `GetHashCode()`를 오버라이드하여 구조적 동등성(Structural Equality)을 제공합니다.

```csharp
class Order
{
    public int CustomerId { get; set; }
    public int ProductId { get; set; }
    public int Quantity { get; set; }
}

class OrderDetail
{
    public int CustomerId { get; set; }
    public int ProductId { get; set; }
    public decimal Price { get; set; }
}

List<Order> orders = new List<Order>
{
    new Order { CustomerId = 1, ProductId = 101, Quantity = 5 },
    new Order { CustomerId = 1, ProductId = 102, Quantity = 3 },
    new Order { CustomerId = 2, ProductId = 101, Quantity = 2 }
};

List<OrderDetail> orderDetails = new List<OrderDetail>
{
    new OrderDetail { CustomerId = 1, ProductId = 101, Price = 1000 },
    new OrderDetail { CustomerId = 1, ProductId = 102, Price = 2000 },
    new OrderDetail { CustomerId = 2, ProductId = 101, Price = 1000 }
};

// 복합 키로 조인 (CustomerId와 ProductId 모두 일치해야 함)
var ordersWithPrices = orders.Join(
    orderDetails,
    order => new { order.CustomerId, order.ProductId },        // 복합 키
    detail => new { detail.CustomerId, detail.ProductId },     // 복합 키
    (order, detail) => new
    {
        order.CustomerId,
        order.ProductId,
        order.Quantity,
        detail.Price,
        TotalPrice = order.Quantity * detail.Price
    }
);

foreach (var item in ordersWithPrices)
{
    Console.WriteLine($"고객 {item.CustomerId}, 제품 {item.ProductId}: " +
                      $"{item.Quantity}개 × ₩{item.Price:N0} = ₩{item.TotalPrice:N0}");
}
// 출력:
// 고객 1, 제품 101: 5개 × ₩1,000 = ₩5,000
// 고객 1, 제품 102: 3개 × ₩2,000 = ₩6,000
// 고객 2, 제품 101: 2개 × ₩1,000 = ₩2,000
```

**복합 키의 내부 동작:**

익명 타입으로 생성된 복합 키는 다음과 같이 동등성을 비교합니다:

```csharp
// 컴파일러가 생성하는 익명 타입의 Equals 메서드 (개념적 표현)
public override bool Equals(object obj)
{
    var other = obj as <>AnonymousType;
    return other != null 
        && this.CustomerId == other.CustomerId 
        && this.ProductId == other.ProductId;
}

public override int GetHashCode()
{
    // HashCode.Combine은 .NET Core 2.1+ 에서 제공
    return HashCode.Combine(CustomerId, ProductId);
}
```

이러한 구조적 동등성 덕분에 복합 키가 해시 기반 조인에서 올바르게 작동합니다.

**다중 조인 (Multi-way Join):**

세 개 이상의 시퀀스를 조인해야 하는 경우, 여러 `Join` 호출을 체이닝하거나 쿼리 구문에서 여러 `join` 절을 사용할 수 있습니다.

```csharp
class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int ClassId { get; set; }
}

class Class
{
    public int Id { get; set; }
    public string ClassName { get; set; }
    public int TeacherId { get; set; }
}

class Teacher
{
    public int Id { get; set; }
    public string TeacherName { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Id = 1, Name = "김철수", ClassId = 10 },
    new Student { Id = 2, Name = "이영희", ClassId = 20 }
};

List<Class> classes = new List<Class>
{
    new Class { Id = 10, ClassName = "수학", TeacherId = 100 },
    new Class { Id = 20, ClassName = "영어", TeacherId = 101 }
};

List<Teacher> teachers = new List<Teacher>
{
    new Teacher { Id = 100, TeacherName = "박교수" },
    new Teacher { Id = 101, TeacherName = "최교수" }
};

// 방법 1: 메서드 체이닝
var result1 = students
    .Join(classes, s => s.ClassId, c => c.Id, (s, c) => new { s, c })
    .Join(teachers, sc => sc.c.TeacherId, t => t.Id, (sc, t) => new
    {
        StudentName = sc.s.Name,
        ClassName = sc.c.ClassName,
        TeacherName = t.TeacherName
    });

// 방법 2: 쿼리 구문 (더 가독성 좋음)
var result2 = from student in students
              join cls in classes on student.ClassId equals cls.Id
              join teacher in teachers on cls.TeacherId equals teacher.Id
              select new
              {
                  StudentName = student.Name,
                  ClassName = cls.ClassName,
                  TeacherName = teacher.TeacherName
              };

foreach (var item in result2)
{
    Console.WriteLine($"{item.StudentName}는 {item.TeacherName} 선생님의 {item.ClassName} 수업을 듣습니다.");
}
// 출력:
// 김철수는 박교수 선생님의 수학 수업을 듣습니다.
// 이영희는 최교수 선생님의 영어 수업을 듣습니다.
```

**성능 고려사항과 최적화:**

1. **작은 시퀀스를 내부로**: 해시 테이블은 내부 시퀀스로 구축되므로, 작은 쪽을 내부로 배치하면 메모리 효율이 좋습니다.

2. **키 선택자의 복잡도**: 키 선택자는 각 요소마다 호출되므로, 복잡한 계산이 있다면 미리 투영(projection)하는 것이 좋습니다.

3. **조인 전 필터링**: `Where`로 먼저 필터링한 후 조인하면 처리할 데이터가 줄어듭니다.

```csharp
// 비효율적
var result = largeTable.Join(smallTable, ...)
                       .Where(x => x.SomeCondition);

// 효율적
var result = largeTable.Where(x => x.SomeCondition)
                       .Join(smallTable, ...);
```

**주요 특징 요약:**

| 특성 | 설명 | 비고 |
|------|------|------|
| **조인 타입** | INNER JOIN (교집합) | 양쪽 모두 키 존재 필수 |
| **시간 복잡도** | O(n + m) | 해시 조인 알고리즘 |
| **공간 복잡도** | O(m) | 내부 시퀀스의 Lookup 구조 |
| **실행 모델** | 지연 실행 | 열거 시점에 실행 |
| **다중 키** | 익명 타입 지원 | 구조적 동등성 |
| **반환 타입** | `IEnumerable<TResult>` | 지연 실행 시퀀스 |

### 15.1.2 GroupJoin

`GroupJoin`은 외부 시퀀스의 각 요소에 대해 내부 시퀀스에서 일치하는 모든 요소를 그룹으로 결합합니다. 이는 SQL의 **LEFT OUTER JOIN**에 해당하는 연산으로, 관계 대수에서는 **외부 조인(Outer Join)** 또는 **좌측 외부 조인(Left Outer Join)**으로 알려져 있습니다. 수학적으로는 외부 시퀀스의 모든 요소를 보존하면서, 각 요소에 대해 내부 시퀀스에서 일치하는 요소들의 부분집합을 연결하는 연산입니다.

**GroupJoin의 이론적 배경:**

LEFT OUTER JOIN은 관계형 데이터베이스에서 매우 중요한 개념으로, 선택적 관계(Optional Relationship)를 표현하는 데 사용됩니다. 예를 들어, "모든 부서와 각 부서의 직원들"을 조회할 때, 직원이 없는 부서도 결과에 포함되어야 합니다. 이는 데이터 분석에서 "모든 카테고리를 보여주되, 데이터가 없는 카테고리는 0으로 표시"하는 패턴에 해당합니다.

**GroupJoin과 Join의 근본적 차이:**

| 측면 | Join (INNER) | GroupJoin (LEFT OUTER) |
|------|-------------|------------------------|
| **결과 포함** | 양쪽 모두 일치하는 키만 | 외부 시퀀스의 모든 요소 |
| **일치 없을 때** | 결과에서 제외됨 | 빈 그룹으로 포함됨 |
| **결과 선택자** | `(outer, inner) => result` | `(outer, IEnumerable<inner>) => result` |
| **SQL 대응** | INNER JOIN | LEFT OUTER JOIN |
| **사용 사례** | 필수 관계 | 선택적 관계 |

**GroupJoin의 내부 메커니즘:**

`GroupJoin`도 `Join`과 마찬가지로 해시 기반 알고리즘을 사용하지만, 결과 생성 방식이 다릅니다:

1. **Lookup 구축**: 내부 시퀀스를 `Lookup<TKey, TElement>` 구조로 변환 (O(m))
2. **외부 시퀀스 순회**: 각 외부 요소에 대해:
   - 해당 키로 Lookup 조회
   - 일치하는 요소들의 `IEnumerable`을 결과 선택자에 전달
   - 일치하는 요소가 없으면 **빈 시퀀스**를 전달 (이것이 핵심!)
3. **지연 평가**: 결과 선택자가 반환하는 그룹 시퀀스도 지연 평가됨

시간 복잡도는 O(n+m)으로 Join과 동일하지만, 메모리 사용 패턴은 약간 다릅니다. 각 그룹이 `IEnumerable`로 반환되므로, 그룹을 여러 번 열거하면 매번 Lookup 조회가 발생할 수 있습니다.

**기본 구조:**

```csharp
var result = outer.GroupJoin(
    inner,
    outerKeySelector,
    innerKeySelector,
    resultSelector                  // (외부요소, 일치하는내부요소들) => 결과
);
```

**예제:**

```csharp
// 부서 정보
class Department
{
    public int Id { get; set; }
    public string DeptName { get; set; }
}

// 직원 정보
class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int DeptId { get; set; }
}

List<Department> departments = new List<Department>
{
    new Department { Id = 1, DeptName = "개발팀" },
    new Department { Id = 2, DeptName = "영업팀" },
    new Department { Id = 3, DeptName = "인사팀" }
};

List<Employee> employees = new List<Employee>
{
    new Employee { Id = 1, Name = "김철수", DeptId = 1 },
    new Employee { Id = 2, Name = "이영희", DeptId = 1 },
    new Employee { Id = 3, Name = "박민수", DeptId = 2 }
};

// GroupJoin: 각 부서별로 소속 직원들을 그룹화
var deptEmployees = departments.GroupJoin(
    employees,
    dept => dept.Id,
    emp => emp.DeptId,
    (dept, empGroup) => new
    {
        Department = dept.DeptName,
        Employees = empGroup,
        Count = empGroup.Count()
    }
);

foreach (var de in deptEmployees)
{
    Console.WriteLine($"{de.Department} (직원 수: {de.Count}):");
    foreach (var emp in de.Employees)
    {
        Console.WriteLine($"  - {emp.Name}");
    }
}
// 출력:
// 개발팀 (직원 수: 2):
//   - 김철수
//   - 이영희
// 영업팀 (직원 수: 1):
//   - 박민수
// 인사팀 (직원 수: 0):
```

**GroupJoin의 실전 활용 패턴:**

GroupJoin은 "부모-자식" 관계를 표현하는 데 매우 유용합니다. 각 부모 요소에 대해 자식 요소들의 컬렉션을 연결하여, 계층적(Hierarchical) 데이터 구조를 생성할 수 있습니다.

```csharp
// 고객과 주문의 관계
class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string City { get; set; }
}

class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public decimal Amount { get; set; }
    public DateTime OrderDate { get; set; }
}

List<Customer> customers = new List<Customer>
{
    new Customer { Id = 1, Name = "김철수", City = "서울" },
    new Customer { Id = 2, Name = "이영희", City = "부산" },
    new Customer { Id = 3, Name = "박민수", City = "대구" }
};

List<Order> orders = new List<Order>
{
    new Order { Id = 101, CustomerId = 1, Amount = 50000, OrderDate = new DateTime(2024, 1, 15) },
    new Order { Id = 102, CustomerId = 1, Amount = 75000, OrderDate = new DateTime(2024, 2, 20) },
    new Order { Id = 103, CustomerId = 2, Amount = 30000, OrderDate = new DateTime(2024, 1, 10) }
    // 주의: 고객 3(박민수)은 주문이 없음
};

// 각 고객별 주문 내역 조회
var customerOrders = customers.GroupJoin(
    orders,
    customer => customer.Id,
    order => order.CustomerId,
    (customer, customerOrderGroup) => new
    {
        Customer = customer,
        Orders = customerOrderGroup.ToList(),  // ToList()로 즉시 평가
        OrderCount = customerOrderGroup.Count(),
        TotalAmount = customerOrderGroup.Sum(o => o.Amount)
    }
);

foreach (var co in customerOrders)
{
    Console.WriteLine($"\n{co.Customer.Name} ({co.Customer.City})");
    Console.WriteLine($"  주문 건수: {co.OrderCount}");
    Console.WriteLine($"  총 구매액: {co.TotalAmount:C0}");
    
    if (co.OrderCount > 0)
    {
        Console.WriteLine("  주문 목록:");
        foreach (var order in co.Orders)
        {
            Console.WriteLine($"    - 주문 #{order.Id}: {order.Amount:C0} ({order.OrderDate:yyyy-MM-dd})");
        }
    }
    else
    {
        Console.WriteLine("  주문 내역 없음");
    }
}
// 출력:
// 김철수 (서울)
//   주문 건수: 2
//   총 구매액: ₩125,000
//   주문 목록:
//     - 주문 #101: ₩50,000 (2024-01-15)
//     - 주문 #102: ₩75,000 (2024-02-20)
//
// 이영희 (부산)
//   주문 건수: 1
//   총 구매액: ₩30,000
//   주문 목록:
//     - 주문 #103: ₩30,000 (2024-01-10)
//
// 박민수 (대구)
//   주문 건수: 0
//   총 구매액: ₩0
//   주문 내역 없음
```

**쿼리 구문:**

```csharp
var deptEmployees2 = from dept in departments
                     join emp in employees
                     on dept.Id equals emp.DeptId into empGroup
                     select new
                     {
                         Department = dept.DeptName,
                         Employees = empGroup
                     };
```

**`into` 키워드의 의미:**

쿼리 구문에서 `into` 키워드는 GroupJoin을 나타내는 특별한 문법입니다. `into empGroup`은 일치하는 내부 요소들을 그룹으로 수집하여 새로운 범위 변수(Range Variable)로 만듭니다. 이는 SQL의 서브쿼리(Subquery)나 CTE(Common Table Expression)와 유사한 개념으로, 중간 결과를 명명된 변수로 참조할 수 있게 합니다.

컴파일러는 `into` 키워드를 만나면 자동으로 `GroupJoin` 메서드 호출로 변환합니다. 이는 LINQ 쿼리 표현식의 핵심 변환 규칙 중 하나입니다.

**LEFT OUTER JOIN의 완전한 구현:**

GroupJoin만으로는 SQL의 LEFT OUTER JOIN과 완전히 동일한 결과를 얻을 수 없습니다. GroupJoin은 그룹을 반환하지만, 전통적인 LEFT OUTER JOIN은 평탄화된(flattened) 결과를 반환합니다. 이를 구현하려면 `SelectMany`와 `DefaultIfEmpty`를 조합해야 합니다.

**Left Outer Join 구현:**

```csharp
// GroupJoin + SelectMany + DefaultIfEmpty로 LEFT OUTER JOIN 구현
var leftOuterJoin = departments
    .GroupJoin(
        employees,
        dept => dept.Id,
        emp => emp.DeptId,
        (dept, empGroup) => new { dept, empGroup }
    )
    .SelectMany(
        x => x.empGroup.DefaultIfEmpty(),  // 빈 그룹은 null 요소 하나로 변환
        (x, emp) => new
        {
            DeptName = x.dept.DeptName,
            EmployeeName = emp?.Name ?? "(없음)"
        }
    );

foreach (var item in leftOuterJoin)
{
    Console.WriteLine($"{item.DeptName}: {item.EmployeeName}");
}
// 출력:
// 개발팀: 김철수
// 개발팀: 이영희
// 영업팀: 박민수
// 인사팀: (없음)
```

**Left Outer Join 패턴의 상세 분석:**

이 패턴은 세 가지 핵심 연산자의 조합입니다:

1. **GroupJoin**: 각 부서에 대해 소속 직원들의 그룹을 생성
   ```
   { dept: 개발팀, empGroup: [김철수, 이영희] }
   { dept: 영업팀, empGroup: [박민수] }
   { dept: 인사팀, empGroup: [] }  ← 빈 그룹
   ```

2. **DefaultIfEmpty()**: 빈 그룹을 `null` 요소 하나로 대체
   ```
   { dept: 인사팀, empGroup: [null] }  ← null로 채움
   ```

3. **SelectMany**: 그룹을 평탄화하여 각 직원마다 하나의 행 생성
   ```
   개발팀: 김철수
   개발팀: 이영희
   영업팀: 박민수
   인사팀: null → "(없음)"으로 표시
   ```

`SelectMany`의 두 번째 오버로드는 `collectionSelector`와 `resultSelector` 두 개의 람다를 받습니다:
- `collectionSelector`: `x => x.empGroup.DefaultIfEmpty()` - 각 그룹을 평탄화할 시퀀스로 변환
- `resultSelector`: `(x, emp) => ...` - 외부 요소(x)와 내부 요소(emp)를 결합하여 결과 생성

**쿼리 구문으로 LEFT OUTER JOIN 표현:**

```csharp
var leftOuterJoinQuery = from dept in departments
                         join emp in employees
                         on dept.Id equals emp.DeptId into empGroup
                         from emp in empGroup.DefaultIfEmpty()
                         select new
                         {
                             DeptName = dept.DeptName,
                             EmployeeName = emp?.Name ?? "(없음)"
                         };
```

두 번째 `from` 절이 `SelectMany`로 변환되며, 이것이 LEFT OUTER JOIN의 평탄화를 수행합니다. 이는 SQL의 구조와 매우 유사합니다:

```sql
-- SQL 동등 코드
SELECT d.DeptName, COALESCE(e.Name, '(없음)') AS EmployeeName
FROM Departments d
LEFT OUTER JOIN Employees e ON d.Id = e.DeptId
```

**Right Outer Join과 Full Outer Join 구현:**

LINQ는 Right Outer Join과 Full Outer Join을 직접 지원하지 않지만, 시퀀스 순서를 바꾸거나 조합하여 구현할 수 있습니다:

```csharp
// Right Outer Join: 시퀀스 순서를 바꿔서 Left Outer Join 수행
var rightOuterJoin = employees
    .GroupJoin(departments, e => e.DeptId, d => d.Id, (e, depts) => new { e, depts })
    .SelectMany(x => x.depts.DefaultIfEmpty(), (x, dept) => new
    {
        EmployeeName = x.e.Name,
        DeptName = dept?.DeptName ?? "(부서 없음)"
    });

// Full Outer Join: Left + Right에서 Left를 제외
var fullOuterJoin = leftOuterJoin
    .Concat(rightOuterJoin.Where(r => r.DeptName == "(부서 없음)"))
    .Distinct();  // 중복 제거
```

**성능 고려사항:**

GroupJoin은 Join과 동일한 O(n+m) 시간 복잡도를 가지지만, 몇 가지 차이점이 있습니다:

1. **메모리 압력**: 결과 선택자가 `IEnumerable` 그룹을 받으므로, 그룹을 즉시 평가(`ToList()`)하지 않으면 지연 평가로 인한 메모리 이점이 있지만, 여러 번 열거하면 성능 저하가 발생할 수 있습니다.

2. **빈 그룹 처리**: 외부 시퀀스의 모든 요소를 처리하므로, 일치하지 않는 요소가 많으면 Join보다 더 많은 결과 객체를 생성합니다.

3. **집계 쿼리 최적화**: 그룹별 집계(Count, Sum 등)가 필요한 경우, GroupJoin이 Join보다 효율적입니다.

**주요 특징 요약:**

| 특성 | 설명 | 비고 |
|------|------|------|
| **조인 타입** | LEFT OUTER JOIN | 외부 시퀀스 보존 |
| **결과 선택자** | `(outer, IEnumerable<inner>) => result` | 그룹 시퀀스 전달 |
| **빈 그룹** | 빈 `IEnumerable` | `DefaultIfEmpty()` 필요 |
| **시간 복잡도** | O(n + m) | 해시 기반 |
| **실행 모델** | 지연 실행 | 그룹도 지연 |
| **SQL 대응** | LEFT OUTER JOIN | `into` 키워드 |

---

## 15.2 그룹화

그룹화는 데이터를 특정 기준에 따라 여러 그룹으로 분류하는 작업으로, 데이터 분석과 집계의 핵심 연산입니다. LINQ의 `GroupBy` 연산자는 SQL의 **GROUP BY 절**과 동일한 기능을 제공하며, 관계 대수의 **투영(Projection)**과 **집계(Aggregation)**를 결합한 연산입니다. 이는 OLAP(Online Analytical Processing) 큐브, 피벗 테이블(Pivot Table), 그리고 다차원 데이터 분석의 기반이 되는 개념입니다.

**그룹화의 이론적 배경:**

그룹화는 집합론의 **분할(Partition)** 개념에 기반합니다. 분할은 집합을 서로소(disjoint) 부분집합들로 나누는 것으로, 다음 조건을 만족합니다:

1. 모든 부분집합의 합집합은 원래 집합과 동일
2. 어떤 두 부분집합도 공통 요소를 갖지 않음
3. 빈 부분집합은 포함하지 않음

수학적으로 표현하면: 집합 S의 분할 P = {S₁, S₂, ..., Sₙ}에 대해
- S₁ ∪ S₂ ∪ ... ∪ Sₙ = S (완전성)
- Sᵢ ∩ Sⱼ = ∅ (i ≠ j인 모든 i, j에 대해) (상호 배타성)

`GroupBy`는 키 선택자 함수를 기준으로 이러한 분할을 생성합니다. 동일한 키를 가진 요소들이 하나의 부분집합(그룹)을 형성합니다.

**GROUP BY의 역사와 SQL:**

GROUP BY 절은 1970년대 후반 SQL에 추가되었으며, 관계형 데이터베이스의 집계 쿼리를 위한 필수 기능이 되었습니다. IBM의 System R 프로젝트에서 처음 구현되었고, 이후 ANSI SQL 표준에 포함되었습니다. LINQ의 `GroupBy`는 이러한 SQL 개념을 객체 지향 환경에 적용하면서도, 더 강력한 타입 안정성과 표현력을 제공합니다.

**GroupBy의 내부 메커니즘:**

`GroupBy`는 내부적으로 다음과 같이 동작합니다:

1. **해시 테이블 구축**: 각 요소의 키를 계산하여 `Lookup<TKey, TElement>` 구조에 저장 (O(n))
2. **그룹 생성**: 동일한 키를 가진 요소들을 `IGrouping<TKey, TElement>` 객체로 묶음
3. **지연 반환**: `IEnumerable<IGrouping<TKey, TElement>>`를 지연 실행으로 반환

`IGrouping<TKey, TElement>` 인터페이스는 `IEnumerable<TElement>`를 상속하며, 추가로 `Key` 속성을 제공합니다:

```csharp
public interface IGrouping<out TKey, out TElement> : IEnumerable<TElement>
{
    TKey Key { get; }
}
```

이 설계는 각 그룹이 키를 가진 시퀀스임을 명확히 표현하며, LINQ의 함수형 프로그래밍 철학을 반영합니다.

### 15.2.1 GroupBy

`GroupBy`는 시퀀스의 요소들을 키 값에 따라 그룹으로 분류합니다. 결과는 `IEnumerable<IGrouping<TKey, TElement>>` 타입으로 반환되며, 각 `IGrouping`은 키와 해당 키를 가진 요소들의 컬렉션을 포함합니다. 이는 마치 `Dictionary<TKey, List<TElement>>`와 유사하지만, 더 함수형 프로그래밍 스타일을 따릅니다.

**기본 구조:**

```csharp
var groups = sequence.GroupBy(keySelector);
var groups = sequence.GroupBy(keySelector, elementSelector);
var groups = sequence.GroupBy(keySelector, resultSelector);
```

**간단한 예제:**

```csharp
List<string> fruits = new List<string>
{
    "Apple", "Apricot", "Banana", "Blueberry", 
    "Cherry", "Coconut", "Date"
};

// 첫 글자로 그룹화
var fruitsByLetter = fruits.GroupBy(fruit => fruit[0]);

foreach (var group in fruitsByLetter)
{
    Console.WriteLine($"[{group.Key}]");
    foreach (var fruit in group)
    {
        Console.WriteLine($"  - {fruit}");
    }
}
// 출력:
// [A]
//   - Apple
//   - Apricot
// [B]
//   - Banana
//   - Blueberry
// [C]
//   - Cherry
//   - Coconut
// [D]
//   - Date
```

**복잡한 객체 그룹화:**

```csharp
class Product
{
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
}

List<Product> products = new List<Product>
{
    new Product { Name = "노트북", Category = "전자제품", Price = 1200000 },
    new Product { Name = "마우스", Category = "전자제품", Price = 25000 },
    new Product { Name = "책상", Category = "가구", Price = 150000 },
    new Product { Name = "의자", Category = "가구", Price = 80000 },
    new Product { Name = "키보드", Category = "전자제품", Price = 75000 }
};

// 카테고리별로 그룹화
var productsByCategory = products.GroupBy(p => p.Category);

foreach (var group in productsByCategory)
{
    Console.WriteLine($"\n카테고리: {group.Key}");
    Console.WriteLine($"제품 수: {group.Count()}");
    Console.WriteLine($"총 가격: {group.Sum(p => p.Price):C0}");
    Console.WriteLine("제품 목록:");
    foreach (var product in group)
    {
        Console.WriteLine($"  - {product.Name}: {product.Price:C0}");
    }
}
// 출력:
// 카테고리: 전자제품
// 제품 수: 3
// 총 가격: ₩1,300,000
// 제품 목록:
//   - 노트북: ₩1,200,000
//   - 마우스: ₩25,000
//   - 키보드: ₩75,000
//
// 카테고리: 가구
// 제품 수: 2
// 총 가격: ₩230,000
// 제품 목록:
//   - 책상: ₩150,000
//   - 의자: ₩80,000
```

**결과 선택자 사용:**

```csharp
// 그룹화와 동시에 결과 형태 지정
var categorySummary = products.GroupBy(
    p => p.Category,
    (category, items) => new
    {
        Category = category,
        Count = items.Count(),
        TotalPrice = items.Sum(p => p.Price),
        AvgPrice = items.Average(p => p.Price)
    }
);

foreach (var summary in categorySummary)
{
    Console.WriteLine($"{summary.Category}: {summary.Count}개, " +
                      $"평균 {summary.AvgPrice:C0}");
}
// 출력:
// 전자제품: 3개, 평균 ₩433,333
// 가구: 2개, 평균 ₩115,000
```

**요소 선택자 사용:**

```csharp
// 그룹화할 때 요소를 변환
var pricesByCategory = products.GroupBy(
    p => p.Category,      // 키 선택자
    p => p.Price          // 요소 선택자 (가격만 그룹화)
);

foreach (var group in pricesByCategory)
{
    Console.WriteLine($"{group.Key}: {string.Join(", ", group.Select(p => $"₩{p:N0}"))}");
}
// 출력:
// 전자제품: ₩1,200,000, ₩25,000, ₩75,000
// 가구: ₩150,000, ₩80,000
```

**쿼리 구문:**

```csharp
var grouped = from p in products
              group p by p.Category into categoryGroup
              select new
              {
                  Category = categoryGroup.Key,
                  Products = categoryGroup
              };
```

**다중 키 그룹화:**

```csharp
class Sale
{
    public string Product { get; set; }
    public string Region { get; set; }
    public int Year { get; set; }
    public decimal Amount { get; set; }
}

List<Sale> sales = new List<Sale>
{
    new Sale { Product = "노트북", Region = "서울", Year = 2023, Amount = 1000000 },
    new Sale { Product = "노트북", Region = "서울", Year = 2024, Amount = 1200000 },
    new Sale { Product = "마우스", Region = "서울", Year = 2023, Amount = 50000 },
    new Sale { Product = "노트북", Region = "부산", Year = 2023, Amount = 800000 }
};

// 지역과 연도로 그룹화 (익명 타입 사용)
var salesByRegionYear = sales.GroupBy(s => new { s.Region, s.Year });

foreach (var group in salesByRegionYear)
{
    Console.WriteLine($"{group.Key.Region} - {group.Key.Year}년:");
    foreach (var sale in group)
    {
        Console.WriteLine($"  {sale.Product}: {sale.Amount:C0}");
    }
    Console.WriteLine($"  합계: {group.Sum(s => s.Amount):C0}\n");
}
// 출력:
// 서울 - 2023년:
//   노트북: ₩1,000,000
//   마우스: ₩50,000
//   합계: ₩1,050,000
//
// 서울 - 2024년:
//   노트북: ₩1,200,000
//   합계: ₩1,200,000
//
// 부산 - 2023년:
//   노트북: ₩800,000
//   합계: ₩800,000
```

**다중 키 그룹화의 이론과 실무:**

다중 키 그룹화는 OLAP(Online Analytical Processing)의 **다차원 분석(Multidimensional Analysis)**과 직접 연결됩니다. 데이터 웨어하우스에서 사용되는 큐브(Cube) 개념을 LINQ로 구현하는 것입니다. 익명 타입으로 생성된 복합 키는 구조적 동등성을 제공하여, 모든 속성이 일치해야 같은 그룹으로 인식됩니다.

**계층적 그룹화(Hierarchical Grouping):**

실무에서는 여러 수준의 그룹화가 필요할 때가 많습니다. 예를 들어, "지역별 → 연도별 → 제품별" 계층 구조입니다. 이는 중첩된 `GroupBy`로 구현할 수 있습니다:

```csharp
// 지역별로 먼저 그룹화, 그 안에서 다시 연도별로 그룹화
var hierarchicalGroups = sales
    .GroupBy(s => s.Region)
    .Select(regionGroup => new
    {
        Region = regionGroup.Key,
        YearGroups = regionGroup
            .GroupBy(s => s.Year)
            .Select(yearGroup => new
            {
                Year = yearGroup.Key,
                Products = yearGroup.ToList(),
                TotalSales = yearGroup.Sum(s => s.Amount)
            })
            .OrderBy(yg => yg.Year)
    });

foreach (var region in hierarchicalGroups)
{
    Console.WriteLine($"\n=== {region.Region} ===");
    foreach (var year in region.YearGroups)
    {
        Console.WriteLine($"  {year.Year}년: {year.TotalSales:C0}");
        foreach (var sale in year.Products)
        {
            Console.WriteLine($"    - {sale.Product}: {sale.Amount:C0}");
        }
    }
}
```

**GroupBy with HAVING (SQL의 HAVING 절 구현):**

SQL의 `HAVING` 절은 그룹을 필터링하는 데 사용됩니다. LINQ에서는 `GroupBy` 후 `Where`를 사용하여 동일한 기능을 구현합니다:

```csharp
// SQL: SELECT Category, COUNT(*) 
//      FROM Products 
//      GROUP BY Category 
//      HAVING COUNT(*) > 1

var categoriesWithMultipleProducts = products
    .GroupBy(p => p.Category)
    .Where(g => g.Count() > 1)  // HAVING 절에 해당
    .Select(g => new
    {
        Category = g.Key,
        Count = g.Count(),
        Products = g.Select(p => p.Name).ToList()
    });

foreach (var cat in categoriesWithMultipleProducts)
{
    Console.WriteLine($"{cat.Category} ({cat.Count}개): {string.Join(", ", cat.Products)}");
}
// 출력: 전자제품 (3개): 노트북, 마우스, 키보드
// (가구는 2개이므로 > 1 조건을 만족하지만, 예제 데이터에서는 제외)
```

**GroupBy의 오버로드 패턴:**

`GroupBy`는 8가지 오버로드를 제공하며, 각각 다른 조합의 선택자를 받습니다:

```csharp
// 1. 키 선택자만
GroupBy(keySelector)
// → IEnumerable<IGrouping<TKey, TSource>>

// 2. 키 + 요소 선택자
GroupBy(keySelector, elementSelector)
// → IEnumerable<IGrouping<TKey, TElement>>

// 3. 키 + 결과 선택자
GroupBy(keySelector, resultSelector)
// → IEnumerable<TResult>

// 4. 키 + 요소 + 결과 선택자
GroupBy(keySelector, elementSelector, resultSelector)
// → IEnumerable<TResult>

// 각각 + IEqualityComparer<TKey> 버전 4가지 추가 = 총 8가지
```

**성능 최적화 전략:**

1. **즉시 집계**: 그룹을 여러 번 열거하지 않도록 결과 선택자에서 즉시 집계
```csharp
// 비효율적: 그룹을 여러 번 열거
var bad = products.GroupBy(p => p.Category);
foreach (var g in bad)
{
    g.Count();  // 첫 번째 열거
    g.Sum(p => p.Price);  // 두 번째 열거
}

// 효율적: 결과 선택자에서 한 번에 집계
var good = products.GroupBy(
    p => p.Category,
    (key, items) => new { Key = key, Count = items.Count(), Sum = items.Sum(p => p.Price) }
);
```

2. **메모리 고려**: 큰 그룹이 예상되면 스트리밍 처리 고려
3. **키 선택자 복잡도**: 키 계산이 복잡하면 미리 투영(projection)

**주요 특징 요약:**

| 특성 | 설명 | 비고 |
|------|------|------|
| **결과 타입** | `IEnumerable<IGrouping<TKey, TElement>>` | 그룹의 시퀀스 |
| **시간 복잡도** | O(n) | 해시 테이블 사용 |
| **공간 복잡도** | O(n) | 모든 요소 저장 |
| **실행 모델** | 지연 실행 | 그룹 내부도 지연 |
| **다중 키** | 익명 타입 지원 | 구조적 동등성 |
| **SQL 대응** | GROUP BY, HAVING | `into` + `Where` |

---

## 15.3 집합 연산자

집합 연산자는 시퀀스를 수학적 집합(Mathematical Set)으로 취급하여 Georg Cantor의 **집합론(Set Theory)**에 기반한 연산을 수행합니다. 이는 19세기 후반 Cantor가 정립한 집합론의 기본 개념들을 프로그래밍에 직접 적용한 것으로, 중복 제거, 합집합(Union), 교집합(Intersection), 차집합(Difference) 등의 연산을 제공합니다.

**집합론의 기초와 LINQ:**

집합론에서 집합(Set)은 서로 다른 객체들의 모임으로 정의되며, 다음과 같은 기본 특성을 가집니다:

1. **유일성(Uniqueness)**: 집합의 각 원소는 단 한 번만 나타남 (중복 불허)
2. **순서 무관(Unordered)**: 원소의 나열 순서는 집합의 동일성에 영향을 주지 않음
3. **멤버십(Membership)**: 원소가 집합에 속하는지 여부만 중요

LINQ의 집합 연산자는 이러한 수학적 정의를 구현하면서도, 실용적인 프로그래밍을 위해 약간의 변형을 가합니다. 특히 **순서 보존(Order Preservation)**을 제공하여, 첫 번째 등장 순서를 유지합니다.

**집합 연산의 내부 알고리즘:**

모든 LINQ 집합 연산자는 내부적으로 **해시 세트(HashSet)**를 사용하여 O(n) 또는 O(n+m) 시간 복잡도를 달성합니다. 이는 다음과 같은 단계로 작동합니다:

1. **해시 함수**: 각 요소의 `GetHashCode()`를 계산하여 버킷 인덱스 결정
2. **동등성 비교**: 같은 버킷 내에서 `Equals()` 메서드로 실제 동등성 검사
3. **중복 방지**: 이미 처리된 요소는 해시 세트에 기록하여 재처리 방지

이러한 해시 기반 접근은 선형 검색 O(n²)보다 훨씬 효율적이며, 대용량 데이터 처리에 필수적입니다.

**집합 연산자와 수학적 표기법:**

| LINQ 연산자 | 수학 기호 | 수학적 정의 | 시간 복잡도 |
|------------|----------|------------|------------|
| `Distinct` | - | 중복 제거 | O(n) |
| `Union` | A ∪ B | {x : x ∈ A ∨ x ∈ B} | O(n + m) |
| `Intersect` | A ∩ B | {x : x ∈ A ∧ x ∈ B} | O(n + m) |
| `Except` | A \ B | {x : x ∈ A ∧ x ∉ B} | O(n + m) |

LINQ는 이러한 집합 연산을 타입 안전하게 제공하며, 사용자 정의 동등성 비교자를 통해 유연성을 제공합니다.

### 15.3.1 Distinct

`Distinct`는 시퀀스에서 중복된 요소를 제거하고 고유한 요소만 반환합니다. 수학적으로는 다중집합(Multiset 또는 Bag)을 집합(Set)으로 변환하는 연산입니다. 내부적으로 `HashSet<T>`를 사용하여 O(n) 시간 복잡도로 동작하며, 각 요소의 첫 번째 등장 순서를 보존합니다.

**Distinct의 알고리즘 상세:**

```
알고리즘 Distinct(sequence):
    seen ← empty HashSet
    for each element in sequence:
        if element not in seen:
            seen.add(element)
            yield element
```

이 알고리즘은 스트리밍 방식으로 작동하여 메모리 효율적이며, 지연 실행을 지원합니다.

**기본 예제:**

```csharp
List<int> numbers = new List<int> { 1, 2, 2, 3, 4, 4, 4, 5, 1, 3 };

var uniqueNumbers = numbers.Distinct();

Console.WriteLine("중복 제거: " + string.Join(", ", uniqueNumbers));
// 출력: 중복 제거: 1, 2, 3, 4, 5
```

**문자열에서 중복 제거:**

```csharp
List<string> names = new List<string>
{
    "Alice", "Bob", "alice", "Charlie", "BOB", "David"
};

// 대소문자 구분
var distinctNames = names.Distinct();
Console.WriteLine("대소문자 구분: " + string.Join(", ", distinctNames));
// 출력: 대소문자 구분: Alice, Bob, alice, Charlie, BOB, David

// 대소문자 무시
var distinctNamesIgnoreCase = names.Distinct(StringComparer.OrdinalIgnoreCase);
Console.WriteLine("대소문자 무시: " + string.Join(", ", distinctNamesIgnoreCase));
// 출력: 대소문자 무시: Alice, Bob, Charlie, David
```

**복잡한 객체의 중복 제거:**

```csharp
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

List<Person> people = new List<Person>
{
    new Person { Name = "김철수", Age = 25 },
    new Person { Name = "이영희", Age = 30 },
    new Person { Name = "김철수", Age = 25 },
    new Person { Name = "박민수", Age = 28 }
};

// 사용자 정의 비교자 사용
class PersonComparer : IEqualityComparer<Person>
{
    public bool Equals(Person x, Person y)
    {
        return x.Name == y.Name && x.Age == y.Age;
    }

    public int GetHashCode(Person obj)
    {
        return HashCode.Combine(obj.Name, obj.Age);
    }
}

var uniquePeople = people.Distinct(new PersonComparer());

foreach (var person in uniquePeople)
{
    Console.WriteLine($"{person.Name} ({person.Age}세)");
}
// 출력:
// 김철수 (25세)
// 이영희 (30세)
// 박민수 (28세)
```

**DistinctBy (C# 10.0+, .NET 6+):**

```csharp
// 특정 속성을 기준으로 중복 제거
var distinctByName = people.DistinctBy(p => p.Name);

foreach (var person in distinctByName)
{
    Console.WriteLine($"{person.Name} ({person.Age}세)");
}
```

### 15.3.2 Union, Intersect, Except

이 세 연산자는 두 시퀀스 간의 집합 연산을 수행하는 이항 연산자(Binary Operators)로, Cantor의 집합론에서 정의된 핵심 연산들을 구현합니다. 각 연산자는 수학적으로 엄밀하게 정의된 의미를 가지며, 실무에서 데이터 비교, 차이 분석, 데이터 병합 등에 광범위하게 사용됩니다.

**집합 연산의 수학적 정의와 구현:**

1. **Union (A ∪ B)**: 합집합 - 적어도 하나의 집합에 속하는 모든 원소
   - ∀x : x ∈ (A ∪ B) ⟺ (x ∈ A ∨ x ∈ B)
   - 알고리즘: 첫 번째 시퀀스의 모든 고유 요소 + 두 번째 시퀀스의 고유한 새 요소

2. **Intersect (A ∩ B)**: 교집합 - 두 집합 모두에 속하는 원소만
   - ∀x : x ∈ (A ∩ B) ⟺ (x ∈ A ∧ x ∈ B)
   - 알고리즘: 두 번째 시퀀스를 해시 세트로 변환 후, 첫 번째 시퀀스에서 해시 세트에 있는 요소만 선택

3. **Except (A \ B)**: 차집합 (상대 여집합) - 첫 번째 집합에만 속하는 원소
   - ∀x : x ∈ (A \ B) ⟺ (x ∈ A ∧ x ∉ B)
   - 알고리즘: 두 번째 시퀀스를 해시 세트로 변환 후, 첫 번째 시퀀스에서 해시 세트에 없는 요소만 선택

**집합 연산의 대칭성과 비대칭성:**

중요한 수학적 성질:
- **Union과 Intersect는 교환법칙 성립**: A ∪ B = B ∪ A, A ∩ B = B ∩ A
- **Except는 교환법칙 불성립**: A \ B ≠ B \ A (일반적으로)
- **결합법칙**: (A ∪ B) ∪ C = A ∪ (B ∪ C)
- **분배법칙**: A ∪ (B ∩ C) = (A ∪ B) ∩ (A ∪ C)

LINQ의 집합 연산자는 이러한 수학적 성질을 보존하면서도, 순서 보존이라는 추가 기능을 제공합니다.

**Union - 합집합:**

두 시퀀스의 모든 고유한 요소를 반환합니다. 내부적으로 두 시퀀스를 순차적으로 열거하면서 해시 세트에 없는 요소만 yield합니다.

```csharp
List<int> numbers1 = new List<int> { 1, 2, 3, 4, 5 };
List<int> numbers2 = new List<int> { 4, 5, 6, 7, 8 };

var union = numbers1.Union(numbers2);

Console.WriteLine("합집합: " + string.Join(", ", union));
// 출력: 합집합: 1, 2, 3, 4, 5, 6, 7, 8
```

**Union의 내부 동작 분석:**

```
알고리즘 Union(first, second):
    seen ← empty HashSet
    // 첫 번째 시퀀스 처리
    for each element in first:
        if seen.Add(element):  // Add는 새 요소면 true 반환
            yield element
    // 두 번째 시퀀스 처리
    for each element in second:
        if seen.Add(element):  // 이미 있는 요소는 건너뜀
            yield element
```

**Intersect - 교집합:**

두 시퀀스에 모두 존재하는 요소만 반환합니다. 두 번째 시퀀스를 먼저 해시 세트로 구축한 후, 첫 번째 시퀀스를 순회하며 일치하는 요소를 찾습니다.

```csharp
var intersect = numbers1.Intersect(numbers2);

Console.WriteLine("교집합: " + string.Join(", ", intersect));
// 출력: 교집합: 4, 5
```

**Except - 차집합:**

첫 번째 시퀀스에는 있지만 두 번째 시퀀스에는 없는 요소를 반환합니다.

```csharp
var except1 = numbers1.Except(numbers2);
Console.WriteLine("차집합 (1 - 2): " + string.Join(", ", except1));
// 출력: 차집합 (1 - 2): 1, 2, 3

var except2 = numbers2.Except(numbers1);
Console.WriteLine("차집합 (2 - 1): " + string.Join(", ", except2));
// 출력: 차집합 (2 - 1): 6, 7, 8
```

**문자열 집합 연산:**

```csharp
List<string> group1 = new List<string> { "Alice", "Bob", "Charlie" };
List<string> group2 = new List<string> { "Bob", "David", "Eve" };

Console.WriteLine("그룹1: " + string.Join(", ", group1));
Console.WriteLine("그룹2: " + string.Join(", ", group2));

var unionGroups = group1.Union(group2);
Console.WriteLine("합집합: " + string.Join(", ", unionGroups));
// 출력: 합집합: Alice, Bob, Charlie, David, Eve

var intersectGroups = group1.Intersect(group2);
Console.WriteLine("교집합: " + string.Join(", ", intersectGroups));
// 출력: 교집합: Bob

var exceptGroups = group1.Except(group2);
Console.WriteLine("그룹1에만 있는 사람: " + string.Join(", ", exceptGroups));
// 출력: 그룹1에만 있는 사람: Alice, Charlie
```

**복잡한 객체의 집합 연산:**

```csharp
class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
}

List<Student> class1 = new List<Student>
{
    new Student { Id = 1, Name = "김철수" },
    new Student { Id = 2, Name = "이영희" },
    new Student { Id = 3, Name = "박민수" }
};

List<Student> class2 = new List<Student>
{
    new Student { Id = 2, Name = "이영희" },
    new Student { Id = 3, Name = "박민수" },
    new Student { Id = 4, Name = "최지혜" }
};

// 비교자 정의
class StudentComparer : IEqualityComparer<Student>
{
    public bool Equals(Student x, Student y) => x.Id == y.Id;
    public int GetHashCode(Student obj) => obj.Id.GetHashCode();
}

var comparer = new StudentComparer();

// 두 반의 전체 학생 (중복 제거)
var allStudents = class1.Union(class2, comparer);
Console.WriteLine("전체 학생:");
foreach (var s in allStudents)
{
    Console.WriteLine($"  {s.Name}");
}
// 출력:
// 전체 학생:
//   김철수
//   이영희
//   박민수
//   최지혜

// 두 반 모두에 속한 학생
var bothClasses = class1.Intersect(class2, comparer);
Console.WriteLine("\n두 반 모두 수강:");
foreach (var s in bothClasses)
{
    Console.WriteLine($"  {s.Name}");
}
// 출력:
// 두 반 모두 수강:
//   이영희
//   박민수

// 1반에만 속한 학생
var onlyClass1 = class1.Except(class2, comparer);
Console.WriteLine("\n1반만 수강:");
foreach (var s in onlyClass1)
{
    Console.WriteLine($"  {s.Name}");
}
// 출력:
// 1반만 수강:
//   김철수
```

**주요 특징:**
- 모든 집합 연산자는 중복을 제거합니다
- 내부적으로 해시 세트를 사용하여 O(n + m) 시간 복잡도
- 지연 실행됨
- `IEqualityComparer<T>`를 사용하여 사용자 정의 비교 가능

---

## 15.4 지연 실행 (Deferred Execution)

지연 실행(Deferred Execution)은 LINQ의 가장 중요한 특성 중 하나로, 쿼리를 정의하는 시점이 아닌 실제로 결과를 사용하는 시점에 쿼리가 실행되는 것을 의미합니다.

**지연 실행의 원리:**

LINQ의 대부분의 표준 쿼리 연산자(`Where`, `Select`, `OrderBy`, `GroupBy`, `Join` 등)는 지연 실행됩니다. 이는 쿼리 정의와 실행이 분리되어 있음을 의미합니다.

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 쿼리 정의 (이 시점에는 실행되지 않음)
var query = numbers.Where(n => n > 2);

Console.WriteLine("쿼리 정의 완료");

// 원본 데이터 수정
numbers.Add(6);
numbers.Add(7);

// 실제 실행 (이 시점에 쿼리 실행)
Console.WriteLine("결과:");
foreach (var n in query)  // 여기서 실행됨!
{
    Console.WriteLine(n);
}
// 출력:
// 쿼리 정의 완료
// 결과:
// 3
// 4
// 5
// 6
// 7
```

**지연 실행의 장점:**

1. **최적화 기회**: 여러 연산을 결합하여 한 번에 실행 가능
2. **메모리 효율성**: 필요한 만큼만 처리
3. **최신 데이터**: 쿼리 실행 시점의 데이터 반영

```csharp
List<string> names = new List<string> { "Alice", "Bob", "Charlie" };

// 여러 연산을 체이닝
var query = names
    .Where(n => n.Length > 3)      // 지연 실행
    .Select(n => n.ToUpper())       // 지연 실행
    .OrderBy(n => n);               // 지연 실행

Console.WriteLine("쿼리 정의됨");

// 원본 데이터 변경
names.Add("David");
names.Remove("Bob");

// 이제 실행됨 (변경된 데이터로)
foreach (var name in query)
{
    Console.WriteLine(name);
}
// 출력:
// 쿼리 정의됨
// ALICE
// CHARLIE
// DAVID
```

**재실행 특성:**

지연 실행 쿼리는 열거할 때마다 다시 실행됩니다.

```csharp
List<int> source = new List<int> { 1, 2, 3 };
var query = source.Where(n => n > 1);

Console.WriteLine("첫 번째 실행:");
foreach (var n in query)
{
    Console.WriteLine(n);
}
// 출력:
// 2
// 3

// 원본 변경
source.Add(4);
source.Add(5);

Console.WriteLine("\n두 번째 실행:");
foreach (var n in query)
{
    Console.WriteLine(n);  // 새로 추가된 4, 5도 포함됨
}
// 출력:
// 2
// 3
// 4
// 5
```

**주의사항:**

지연 실행은 예상치 못한 동작을 유발할 수 있습니다.

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
var query = numbers.Where(n => n > 2);

// 원본 데이터를 모두 제거
numbers.Clear();

// 쿼리 실행 시 원본이 비어있음
Console.WriteLine("결과 개수: " + query.Count());
// 출력: 결과 개수: 0
```

**지연 실행을 사용하는 연산자:**

- 필터링: `Where`, `OfType`
- 투영: `Select`, `SelectMany`
- 정렬: `OrderBy`, `ThenBy`, `Reverse`
- 조인: `Join`, `GroupJoin`
- 그룹화: `GroupBy`
- 집합: `Distinct`, `Union`, `Intersect`, `Except`
- 생성: `Range`, `Repeat`, `Empty`
- 분할: `Skip`, `Take`, `SkipWhile`, `TakeWhile`

---

## 15.5 즉시 실행 (Immediate Execution)

즉시 실행(Immediate Execution)은 쿼리를 정의하는 즉시 실행하여 결과를 반환하는 것을 의미합니다. LINQ의 일부 연산자는 즉시 실행되며, 주로 집계 연산이나 컬렉션 변환 연산이 여기에 해당합니다.

**즉시 실행 연산자:**

**집계 연산자:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 모두 즉시 실행됨
int count = numbers.Count();            // 5
int sum = numbers.Sum();                // 15
double avg = numbers.Average();         // 3
int max = numbers.Max();                // 5
int min = numbers.Min();                // 1

Console.WriteLine($"개수: {count}, 합계: {sum}, 평균: {avg}");
Console.WriteLine($"최대: {max}, 최소: {min}");
// 출력:
// 개수: 5, 합계: 15, 평균: 3
// 최대: 5, 최소: 1

// 원본 변경해도 결과는 변하지 않음
numbers.Add(10);
Console.WriteLine($"개수(변경 후에도 동일): {count}");  // 여전히 5
```

**컬렉션 변환 연산자:**

```csharp
List<int> source = new List<int> { 1, 2, 3, 4, 5 };

// 즉시 실행 - 결과가 컬렉션으로 변환됨
List<int> list = source.Where(n => n > 2).ToList();
int[] array = source.Where(n => n > 2).ToArray();
Dictionary<int, string> dict = source.ToDictionary(n => n, n => $"Number{n}");

Console.WriteLine("ToList(): " + string.Join(", ", list));
Console.WriteLine("ToArray(): " + string.Join(", ", array));
// 출력:
// ToList(): 3, 4, 5
// ToArray(): 3, 4, 5

// 원본 변경해도 결과는 영향받지 않음
source.Add(6);
source.Add(7);

Console.WriteLine("원본 변경 후 list: " + string.Join(", ", list));
// 출력: 원본 변경 후 list: 3, 4, 5 (변경되지 않음)
```

**ToLookup:**

```csharp
class Student
{
    public string Name { get; set; }
    public string Grade { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Name = "김철수", Grade = "A" },
    new Student { Name = "이영희", Grade = "B" },
    new Student { Name = "박민수", Grade = "A" },
    new Student { Name = "최지혜", Grade = "C" }
};

// ToLookup - 즉시 실행되어 ILookup<TKey, TElement> 반환
var lookup = students.ToLookup(s => s.Grade);

// Lookup은 불변이며 읽기 전용
foreach (var gradeGroup in lookup)
{
    Console.WriteLine($"등급 {gradeGroup.Key}:");
    foreach (var student in gradeGroup)
    {
        Console.WriteLine($"  - {student.Name}");
    }
}
// 출력:
// 등급 A:
//   - 김철수
//   - 박민수
// 등급 B:
//   - 이영희
// 등급 C:
//   - 최지혜
```

**단일 요소 반환 연산자:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 모두 즉시 실행
int first = numbers.First();                        // 1
int firstGreater = numbers.First(n => n > 3);      // 4
int last = numbers.Last();                         // 5
int single = new List<int> { 42 }.Single();        // 42

// OrDefault 버전 - 요소가 없으면 기본값 반환
int firstOrDefault = new List<int>().FirstOrDefault();  // 0
int lastOrDefault = new List<int>().LastOrDefault();    // 0

Console.WriteLine($"First: {first}, Last: {last}");
// 출력: First: 1, Last: 5
```

**Element 연산자:**

```csharp
List<string> names = new List<string> { "Alice", "Bob", "Charlie", "David" };

// ElementAt - 특정 인덱스의 요소 반환
string second = names.ElementAt(1);
Console.WriteLine($"두 번째: {second}");  // 출력: 두 번째: Bob

// ElementAtOrDefault - 인덱스가 범위를 벗어나면 기본값
string outOfRange = names.ElementAtOrDefault(10);
Console.WriteLine($"범위 밖: '{outOfRange}'");  // 출력: 범위 밖: ''
```

**조건 확인 연산자:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 즉시 실행되어 bool 반환
bool hasEven = numbers.Any(n => n % 2 == 0);        // true
bool allPositive = numbers.All(n => n > 0);         // true
bool containsZero = numbers.Contains(0);            // false

Console.WriteLine($"짝수 있음: {hasEven}");
Console.WriteLine($"모두 양수: {allPositive}");
Console.WriteLine($"0 포함: {containsZero}");
// 출력:
// 짝수 있음: True
// 모두 양수: True
// 0 포함: False
```

**지연 실행을 즉시 실행으로 변환:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 지연 실행 쿼리
var deferredQuery = numbers.Where(n => n > 2).Select(n => n * 2);

// 즉시 실행으로 변환 (스냅샷 생성)
var immediateList = deferredQuery.ToList();
var immediateArray = deferredQuery.ToArray();

// 원본 변경
numbers.Add(10);

// 지연 실행 쿼리는 새 데이터 포함
Console.WriteLine("지연 실행: " + string.Join(", ", deferredQuery));
// 출력: 지연 실행: 6, 8, 10, 20

// 즉시 실행 결과는 변경 없음
Console.WriteLine("즉시 실행: " + string.Join(", ", immediateList));
// 출력: 즉시 실행: 6, 8, 10
```

**즉시 실행을 사용하는 연산자:**

- 집계: `Count`, `Sum`, `Average`, `Min`, `Max`, `Aggregate`
- 변환: `ToList`, `ToArray`, `ToDictionary`, `ToLookup`
- 단일 요소: `First`, `FirstOrDefault`, `Last`, `LastOrDefault`, `Single`, `SingleOrDefault`
- 인덱스: `ElementAt`, `ElementAtOrDefault`
- 조건: `Any`, `All`, `Contains`, `SequenceEqual`

**성능 고려사항:**

```csharp
List<int> largeList = Enumerable.Range(1, 1000000).ToList();

// 비효율적: Count()를 여러 번 호출 (매번 재계산)
for (int i = 0; i < 10; i++)
{
    int count = largeList.Where(n => n > 500000).Count();  // 매번 O(n)
}

// 효율적: 한 번만 계산하고 재사용
int cachedCount = largeList.Where(n => n > 500000).Count();
for (int i = 0; i < 10; i++)
{
    // cachedCount 사용
}

// 또는 ToList()로 스냅샷
var snapshot = largeList.Where(n => n > 500000).ToList();
for (int i = 0; i < 10; i++)
{
    int count = snapshot.Count;  // O(1)
}
```

**실행 시점 비교:**

```csharp
List<int> numbers = new List<int> { 1, 2, 3 };

Console.WriteLine("=== 지연 실행 ===");
var deferred = numbers.Where(n => n > 1);  // 아직 실행 안 됨
Console.WriteLine("쿼리 정의됨");
numbers.Add(4);
foreach (var n in deferred)  // 여기서 실행
{
    Console.WriteLine(n);
}
// 출력:
// === 지연 실행 ===
// 쿼리 정의됨
// 2
// 3
// 4

numbers = new List<int> { 1, 2, 3 };

Console.WriteLine("\n=== 즉시 실행 ===");
var immediate = numbers.Where(n => n > 1).ToList();  // 즉시 실행
Console.WriteLine("쿼리 실행됨");
numbers.Add(4);
foreach (var n in immediate)
{
    Console.WriteLine(n);
}
// 출력:
// === 즉시 실행 ===
// 쿼리 실행됨
// 2
// 3
```

---

## 마무리

이 장에서는 LINQ의 고급 연산자들과 실행 모델에 대해 학습했습니다.

**주요 학습 내용:**

✅ **조인 연산자**: `Join`과 `GroupJoin`을 사용한 데이터 결합
✅ **그룹화**: `GroupBy`를 활용한 데이터 분류와 집계
✅ **집합 연산자**: `Distinct`, `Union`, `Intersect`, `Except`를 통한 집합 연산
✅ **지연 실행**: 쿼리 정의와 실행의 분리, 최적화와 메모리 효율성
✅ **즉시 실행**: 집계 연산과 컬렉션 변환의 즉각적 실행

**실무 활용 가이드:**

1. **Join 선택**: 데이터 관계가 명확한 경우 Join 사용, LEFT OUTER JOIN이 필요하면 GroupJoin + SelectMany
2. **GroupBy 활용**: 집계 쿼리나 카테고리별 분석에 필수적
3. **집합 연산**: 중복 제거, 데이터 병합, 차이 분석 등에 활용
4. **실행 모델 이해**: 성능 최적화와 예기치 않은 동작 방지를 위해 필수

**성능 최적화 팁:**

- 지연 실행 쿼리를 여러 번 열거하는 경우 `ToList()`로 캐싱
- 큰 데이터셋에서는 집합 연산자의 해시 기반 알고리즘 활용
- `Count()` 대신 `Any()`로 존재 여부만 확인하여 조기 종료
- 복잡한 쿼리는 단계별로 나누어 디버깅과 최적화 용이

**다음 단계:**

16장에서는 함수형 프로그래밍의 개념들을 더 깊이 탐구하며, 불변성, 순수 함수, 고차 함수 등의 원칙을 학습하게 됩니다. LINQ는 함수형 프로그래밍의 실용적 구현이므로, 이 장에서 배운 내용이 다음 장의 기반이 됩니다.

**실습 과제:**

1. 학생-과목-성적 데이터로 Join과 GroupBy를 조합하여 학생별 평균 성적 계산
2. 두 개의 파일 목록에서 중복 파일 찾기 (Intersect 활용)
3. 지연 실행과 즉시 실행의 성능 차이 측정 및 비교
4. 복잡한 비즈니스 규칙을 LINQ 쿼리로 표현하는 연습
