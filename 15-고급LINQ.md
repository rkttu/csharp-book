# 15장. 고급 LINQ

LINQ(Language Integrated Query)는 C# 3.0에서 도입된 강력한 쿼리 언어로, 데이터 소스에 대한 쿼리를 일관된 방식으로 표현할 수 있게 해줍니다. 14장에서 LINQ의 기초를 다루었다면, 이 장에서는 더욱 고급스러운 LINQ 연산자들과 실행 모델에 대해 학습합니다.

고급 LINQ 연산자들은 복잡한 데이터 변환과 분석을 간결하게 표현할 수 있게 해주며, 특히 조인(Join), 그룹화(GroupBy), 집합 연산(Set Operations) 등은 데이터베이스 쿼리에서 익숙한 개념들을 C#에서 직접 사용할 수 있게 합니다. 또한 지연 실행(Deferred Execution)과 즉시 실행(Immediate Execution)의 차이를 이해하면, LINQ의 성능을 최적화하고 예상치 못한 동작을 방지할 수 있습니다.

## 이 장에서 배울 내용

- **조인 연산자**: 두 개 이상의 데이터 소스를 결합하는 Join과 GroupJoin의 사용법
- **그룹화**: 데이터를 특정 키를 기준으로 그룹화하는 GroupBy 연산
- **집합 연산자**: 중복 제거, 합집합, 교집합, 차집합 등의 집합 연산
- **지연 실행과 즉시 실행**: LINQ 쿼리의 실행 모델과 성능 최적화

---

## 15.1 조인 연산자

조인(Join) 연산은 두 개의 데이터 소스를 특정 키를 기준으로 결합하는 연산입니다. 관계형 데이터베이스의 SQL JOIN과 유사한 개념으로, LINQ에서는 `Join`과 `GroupJoin` 두 가지 조인 메서드를 제공합니다.

### 15.1.1 Join

`Join` 메서드는 두 시퀀스에서 키가 일치하는 요소들을 결합하여 새로운 시퀀스를 생성합니다. SQL의 INNER JOIN과 동일한 개념입니다.

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

**쿼리 구문:**

```csharp
// 쿼리 구문으로 표현
var studentCourses2 = from student in students
                      join course in courses
                      on student.CourseId equals course.Id
                      select new
                      {
                          StudentName = student.Name,
                          CourseName = course.CourseName
                      };
```

**복합 키를 사용한 Join:**

```csharp
// 복합 키로 조인하는 경우
var complexJoin = students.Join(
    courses,
    student => new { Key1 = student.CourseId, Key2 = student.Id },
    course => new { Key1 = course.Id, Key2 = 1 },  // 익명 타입의 속성 이름이 일치해야 함
    (student, course) => new { student.Name, course.CourseName }
);
```

**주요 특징:**
- INNER JOIN 방식으로 작동 (양쪽 모두에 일치하는 키가 있어야 결과에 포함)
- 시간 복잡도: O(n + m) (해시 조인 사용)
- 결과는 지연 실행됨

### 15.1.2 GroupJoin

`GroupJoin`은 외부 시퀀스의 각 요소에 대해 내부 시퀀스에서 일치하는 모든 요소를 그룹으로 결합합니다. SQL의 LEFT OUTER JOIN과 유사하며, 일치하는 요소가 없어도 외부 요소는 결과에 포함됩니다.

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

**Left Outer Join 구현:**

```csharp
// GroupJoin + SelectMany로 LEFT OUTER JOIN 구현
var leftOuterJoin = departments
    .GroupJoin(
        employees,
        dept => dept.Id,
        emp => emp.DeptId,
        (dept, empGroup) => new { dept, empGroup }
    )
    .SelectMany(
        x => x.empGroup.DefaultIfEmpty(),
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

**주요 특징:**
- LEFT OUTER JOIN 방식 (외부 시퀀스의 모든 요소가 결과에 포함)
- 결과 선택자는 외부 요소와 일치하는 내부 요소들의 시퀀스를 받음
- 일치하는 내부 요소가 없으면 빈 시퀀스가 전달됨

---

## 15.2 그룹화

그룹화는 데이터를 특정 기준에 따라 여러 그룹으로 분류하는 작업입니다. LINQ의 `GroupBy` 연산자는 SQL의 GROUP BY 절과 동일한 기능을 제공합니다.

### 15.2.1 GroupBy

`GroupBy`는 시퀀스의 요소들을 키 값에 따라 그룹으로 분류합니다. 결과는 `IGrouping<TKey, TElement>` 시퀀스로 반환되며, 각 그룹은 키와 해당 키를 가진 요소들의 컬렉션을 포함합니다.

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

**주요 특징:**
- 결과는 `IEnumerable<IGrouping<TKey, TElement>>` 타입
- 각 `IGrouping`은 키(Key)와 해당 키를 가진 요소들의 시퀀스
- 지연 실행됨 (쿼리 정의 시점이 아닌 열거 시점에 실행)
- 시간 복잡도: O(n)

---

## 15.3 집합 연산자

집합 연산자는 시퀀스를 집합으로 취급하여 수학적 집합 연산을 수행합니다. LINQ는 중복 제거, 합집합, 교집합, 차집합 등의 집합 연산을 제공합니다.

### 15.3.1 Distinct

`Distinct`는 시퀀스에서 중복된 요소를 제거하고 고유한 요소만 반환합니다. 내부적으로 해시 세트를 사용하여 O(n) 시간 복잡도로 동작합니다.

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

이 연산자들은 두 시퀀스 간의 집합 연산을 수행합니다.

**Union - 합집합:**

두 시퀀스의 모든 고유한 요소를 반환합니다 (중복 제거).

```csharp
List<int> numbers1 = new List<int> { 1, 2, 3, 4, 5 };
List<int> numbers2 = new List<int> { 4, 5, 6, 7, 8 };

var union = numbers1.Union(numbers2);

Console.WriteLine("합집합: " + string.Join(", ", union));
// 출력: 합집합: 1, 2, 3, 4, 5, 6, 7, 8
```

**Intersect - 교집합:**

두 시퀀스에 모두 존재하는 요소만 반환합니다.

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
