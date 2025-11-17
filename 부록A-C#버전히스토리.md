# 부록 A. C# 버전 히스토리

프로그래밍 언어의 진화는 단순히 새로운 문법을 추가하는 것 이상의 의미를 갖습니다. 언어의 발전은 소프트웨어 개발 패러다임의 변화, 하드웨어 기술의 진보, 그리고 개발자 커뮤니티의 경험과 요구사항이 누적되어 만들어진 결과물입니다. C#은 2000년 첫 출시 이후 20년이 넘는 시간 동안 지속적으로 발전해온 현대적인 프로그래밍 언어로, Microsoft의 Anders Hejlsberg가 이끄는 언어 설계팀이 개발자 커뮤니티의 피드백을 세심하게 반영하며 진화시켜 왔습니다.

**C#의 탄생 배경:**

C#은 1990년대 후반의 특별한 시대적 요구에서 탄생했습니다. Anders Hejlsberg는 Borland에서 Turbo Pascal과 Delphi를 개발한 경험을 바탕으로 Microsoft에 합류했으며, Java의 인기와 함께 나타난 새로운 플랫폼 독립적 프로그래밍 언어에 대한 수요를 목격했습니다. Microsoft는 .NET Framework와 함께 작동할 현대적이고 객체지향적이며, 타입 안전성을 보장하는 새로운 언어가 필요했습니다. C#은 C++의 강력함, Java의 생산성, 그리고 Component Pascal의 우아함을 결합하여 탄생했습니다.

**언어 설계의 핵심 철학:**

C# 언어 설계팀은 처음부터 몇 가지 핵심 원칙을 고수해 왔습니다:

1. **타입 안정성(Type Safety)**: 컴파일 타임에 최대한 많은 오류를 잡아내어 런타임 오류를 줄입니다.
2. **생산성(Productivity)**: 개발자가 비즈니스 로직에 집중할 수 있도록 보일러플레이트 코드를 최소화합니다.
3. **호환성(Compatibility)**: 기존 코드를 깨지 않으면서 새로운 기능을 추가합니다.
4. **성능(Performance)**: 언어 수준에서 성능 최적화를 지원하며, 불필요한 오버헤드를 제거합니다.
5. **표현력(Expressiveness)**: 개발자의 의도를 명확하게 표현할 수 있는 문법을 제공합니다.

**진화의 여정:**

C#의 발전은 크게 다섯 단계로 구분할 수 있습니다:

- **1단계 (C# 1.0-2.0)**: 객체지향 프로그래밍의 기반 구축과 제네릭 도입
- **2단계 (C# 3.0-4.0)**: LINQ를 통한 함수형 프로그래밍 개념 통합과 동적 프로그래밍 지원
- **3단계 (C# 5.0-6.0)**: 비동기 프로그래밍의 혁신과 문법적 간결성 개선
- **4단계 (C# 7.0-8.0)**: 패턴 매칭, 튜플, null 안전성 강화
- **5단계 (C# 9.0-10)**: 불변 데이터 모델과 minimal 코드 스타일 추구

이 부록에서는 각 버전에서 도입된 주요 기능들을 역사적 맥락과 함께 살펴보며, 왜 그러한 기능이 필요했는지, 어떤 문제를 해결했는지, 그리고 개발 패턴에 어떤 영향을 미쳤는지 깊이 있게 탐구합니다.

---

## A.1 C# 1.0 ~ 5.0

C#의 첫 10년은 객체지향 프로그래밍의 기반을 다지고, 제네릭을 통해 타입 안정성을 강화하며, LINQ를 통해 함수형 프로그래밍을 통합하고, 마침내 async/await로 비동기 프로그래밍을 혁신한 시기였습니다. 이 기간 동안 C#은 단순한 "Java의 대안"에서 벗어나 독자적인 정체성을 확립했습니다.

### C# 1.0 (2002년) - 객체지향의 시작

C# 1.0은 2002년 2월 .NET Framework 1.0과 함께 출시된 첫 번째 버전입니다. Anders Hejlsberg가 설계를 주도했으며, Java와 C++의 장점을 결합하면서도 단점을 보완하려는 철학으로 만들어졌습니다. 당시 Microsoft는 Windows 플랫폼을 위한 현대적인 개발 환경을 구축하고자 했으며, C#은 .NET Framework의 주력 언어로 선택되었습니다.

**시대적 배경과 설계 철학:**

2000년대 초반, 소프트웨어 개발 세계는 큰 변화를 겪고 있었습니다:

- **Java의 부상**: Java는 "Write Once, Run Anywhere" 철학으로 큰 인기를 얻었지만, Microsoft와의 라이선스 분쟁으로 인해 Windows 플랫폼에서의 미래가 불확실했습니다.
- **C++의 복잡성**: C++는 강력했지만 메모리 관리, 포인터, 다중 상속 등의 복잡성으로 인해 생산성이 낮았습니다.
- **COM의 한계**: Windows 플랫폼의 Component Object Model(COM)은 언어 간 상호 운용성을 제공했지만, 사용하기 복잡하고 오류가 발생하기 쉬웠습니다.

C# 1.0은 이러한 문제들을 해결하기 위해 다음과 같은 핵심 설계 목표를 설정했습니다:

1. **단순성과 현대성**: Java처럼 배우기 쉬우면서도 현대적인 언어 기능 제공
2. **타입 안정성**: 컴파일 타임 타입 검사를 통한 오류 감소
3. **자동 메모리 관리**: 가비지 컬렉션을 통한 메모리 누수 방지
4. **컴포넌트 지향**: 속성, 이벤트, 특성을 언어 수준에서 지원
5. **플랫폼 통합**: .NET Framework와 긴밀한 통합

**핵심 기능 상세 분석:**

**1. 클래스 기반 객체지향 프로그래밍**

C# 1.0은 순수한 객체지향 언어로 설계되었습니다. 모든 코드는 클래스 안에 있어야 했으며, 단일 상속과 다중 인터페이스 구현을 지원했습니다.

```csharp
// 클래스 정의와 상속
public class Animal
{
    protected string name;
    
    public Animal(string name)
    {
        this.name = name;
    }
    
    public virtual void MakeSound()
    {
        Console.WriteLine("Some generic animal sound");
    }
}

public class Dog : Animal
{
    public Dog(string name) : base(name)
    {
    }
    
    public override void MakeSound()
    {
        Console.WriteLine("Woof! My name is " + name);
    }
}
```

**2. 속성(Properties) - Java와의 차별화**

C#의 가장 독특한 특징 중 하나는 속성을 언어 수준에서 지원한다는 점입니다. Java는 getter/setter 메서드를 명시적으로 작성해야 했지만, C#은 속성 문법을 제공했습니다.

```csharp
// C# 1.0의 속성
public class Person
{
    private string name;
    private int age;
    
    // 읽기/쓰기 속성
    public string Name
    {
        get { return name; }
        set 
        {
            if (value != null && value.Length > 0)
                name = value;
            else
                throw new ArgumentException("Name cannot be empty");
        }
    }
    
    // 읽기 전용 속성
    public int Age
    {
        get { return age; }
    }
    
    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
}

// 사용
Person person = new Person("Alice", 30);
Console.WriteLine(person.Name);  // 필드처럼 보이지만 실제로는 메서드 호출
person.Name = "Bob";  // setter 호출
```

속성은 단순히 문법적 편의를 넘어서, 데이터 바인딩, 리플렉션, 직렬화 등에서 중요한 역할을 합니다.

**3. 델리게이트(Delegates) - 타입 안전한 함수 포인터**

델리게이트는 C/C++의 함수 포인터를 타입 안전하게 구현한 것으로, 이벤트 기반 프로그래밍과 콜백 패턴의 기반이 되었습니다.

```csharp
// 델리게이트 정의
public delegate int MathOperation(int x, int y);

public class Calculator
{
    public int Calculate(int a, int b, MathOperation operation)
    {
        return operation(a, b);
    }
}

// 사용
class Program
{
    static int Add(int x, int y)
    {
        return x + y;
    }
    
    static int Multiply(int x, int y)
    {
        return x * y;
    }
    
    static void Main()
    {
        Calculator calc = new Calculator();
        
        MathOperation op = new MathOperation(Add);
        Console.WriteLine(calc.Calculate(5, 3, op));  // 8
        
        op = new MathOperation(Multiply);
        Console.WriteLine(calc.Calculate(5, 3, op));  // 15
    }
}
```

**4. 이벤트(Events) - 관찰자 패턴의 언어 수준 지원**

이벤트는 델리게이트를 기반으로 하며, GUI 프로그래밍과 느슨한 결합(Loose Coupling)을 위한 핵심 메커니즘입니다.

```csharp
// 이벤트 정의
public class Button
{
    // 이벤트 선언
    public event EventHandler Click;
    
    public void OnClick()
    {
        // 이벤트 발생
        if (Click != null)
        {
            Click(this, EventArgs.Empty);
        }
    }
}

// 사용
class Program
{
    static void Main()
    {
        Button button = new Button();
        
        // 이벤트 핸들러 등록
        button.Click += new EventHandler(Button_Click);
        
        // 버튼 클릭 시뮬레이션
        button.OnClick();
    }
    
    static void Button_Click(object sender, EventArgs e)
    {
        Console.WriteLine("Button was clicked!");
    }
}
```

**5. 특성(Attributes) - 메타데이터 프로그래밍**

특성은 코드에 선언적 정보를 추가하는 강력한 메커니즘으로, 리플렉션과 결합하여 프레임워크 수준의 기능을 구현할 수 있습니다.

```csharp
// 커스텀 특성 정의
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class AuthorAttribute : Attribute
{
    public string Name { get; set; }
    public string Date { get; set; }
    
    public AuthorAttribute(string name)
    {
        Name = name;
    }
}

// 특성 사용
[Author("Alice", Date = "2002-01-15")]
public class MyClass
{
    [Author("Bob", Date = "2002-02-20")]
    public void MyMethod()
    {
        // 메서드 구현
    }
}
```

**6. 가비지 컬렉션 - 메모리 관리의 혁신**

C# 1.0은 자동 메모리 관리를 제공하여 개발자가 메모리 할당/해제를 수동으로 관리할 필요가 없어졌습니다.

```csharp
public class ResourceManager
{
    public void ProcessData()
    {
        // 객체 생성 - new로 힙에 할당
        StringBuilder builder = new StringBuilder();
        builder.Append("Hello");
        builder.Append(" World");
        
        string result = builder.ToString();
        Console.WriteLine(result);
        
        // builder는 자동으로 가비지 컬렉션됨
        // 명시적 해제 불필요!
    }
}
```

**C# 1.0의 한계와 향후 과제:**

C# 1.0은 훌륭한 시작이었지만, 몇 가지 한계가 있었습니다:

1. **타입 재사용성 부족**: `ArrayList`는 `object`를 저장하므로 타입 안전성이 없었습니다
2. **보일러플레이트 코드**: 간단한 작업에도 많은 코드가 필요했습니다
3. **비동기 프로그래밍의 복잡성**: 비동기 패턴이 매우 복잡하고 오류가 발생하기 쉬웠습니다

이러한 한계들은 이후 버전에서 체계적으로 해결되었습니다.

### C# 2.0 (2005년) - 제네릭의 도입

C# 2.0은 2005년 11월 .NET Framework 2.0과 함께 출시되었으며, 언어 역사상 가장 중요한 기능 중 하나인 **제네릭(Generics)**을 도입했습니다. 제네릭은 C# 1.0의 가장 큰 문제점이었던 타입 안정성과 성능 문제를 동시에 해결했으며, 이는 C# 언어의 표현력을 획기적으로 향상시킨 중요한 변화였습니다.

**제네릭 이전의 문제점:**

C# 1.0에서는 다양한 타입을 담을 수 있는 컬렉션을 만들기 위해 `object` 타입을 사용해야 했습니다. 이는 심각한 문제를 야기했습니다:

```csharp
// C# 1.0의 ArrayList - 타입 안전하지 않음
ArrayList numbers = new ArrayList();
numbers.Add(1);        // int를 object로 박싱
numbers.Add(2);
numbers.Add("three");  // 컴파일 오류 없음! 런타임에 문제 발생

int sum = 0;
foreach (object obj in numbers)
{
    sum += (int)obj;  // "three"에서 InvalidCastException 발생!
}

// 문제점:
// 1. 타입 안전성 없음 - 잘못된 타입을 추가해도 컴파일 오류가 없음
// 2. 박싱/언박싱 오버헤드 - int 같은 값 타입이 object로 변환되며 성능 저하
// 3. 명시적 캐스팅 필요 - 꺼낼 때마다 캐스팅 필요
// 4. IntelliSense 지원 부족 - IDE가 타입을 추론할 수 없음
```

**제네릭의 혁명:**

제네릭은 이러한 모든 문제를 우아하게 해결했습니다:

```csharp
// C# 2.0의 List<T> - 타입 안전함
List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
// numbers.Add("three");  // 컴파일 오류! 타입 불일치

int sum = 0;
foreach (int num in numbers)  // 캐스팅 불필요
{
    sum += num;  // 안전하고 빠름
}

// 장점:
// 1. 컴파일 타임 타입 안정성 - 잘못된 타입은 컴파일 시점에 발견
// 2. 박싱 없음 - 값 타입이 힙에 할당되지 않음
// 3. 캐스팅 불필요 - 코드가 더 깔끔하고 안전
// 4. IntelliSense 완벽 지원 - IDE가 정확한 타입 제공
```

**제네릭 클래스와 메서드:**

```csharp
// 제네릭 클래스
public class Stack<T>
{
    private T[] items;
    private int count;
    
    public Stack(int capacity)
    {
        items = new T[capacity];
        count = 0;
    }
    
    public void Push(T item)
    {
        if (count < items.Length)
        {
            items[count++] = item;
        }
    }
    
    public T Pop()
    {
        if (count > 0)
        {
            return items[--count];
        }
        throw new InvalidOperationException("Stack is empty");
    }
    
    public int Count
    {
        get { return count; }
    }
}

// 사용
Stack<string> stringStack = new Stack<string>(10);
stringStack.Push("Hello");
stringStack.Push("World");
string value = stringStack.Pop();  // "World", 캐스팅 불필요

Stack<int> intStack = new Stack<int>(10);
intStack.Push(42);
int number = intStack.Pop();  // 42, 박싱/언박싱 없음
```

**제네릭 제약 조건(Generic Constraints):**

제네릭 타입에 제약을 추가하여 특정 기능을 보장할 수 있습니다:

```csharp
// where 절로 제약 조건 지정
public class Repository<T> where T : class, new()
{
    public T CreateNew()
    {
        return new T();  // new() 제약이 있어야 생성자 호출 가능
    }
}

// IComparable 제약 - 비교 가능한 타입만 허용
public class Sorter<T> where T : IComparable<T>
{
    public void Sort(T[] array)
    {
        for (int i = 0; i < array.Length - 1; i++)
        {
            for (int j = i + 1; j < array.Length; j++)
            {
                if (array[i].CompareTo(array[j]) > 0)
                {
                    T temp = array[i];
                    array[i] = array[j];
                    array[j] = temp;
                }
            }
        }
    }
}
```

**익명 메서드(Anonymous Methods):**

C# 2.0은 또한 익명 메서드를 도입하여, 델리게이트를 더 간결하게 작성할 수 있게 했습니다. 이는 C# 3.0의 람다 식의 전신이 되었습니다.

```csharp
// C# 1.0 방식 - 명명된 메서드 필요
button.Click += new EventHandler(Button_Click);

void Button_Click(object sender, EventArgs e)
{
    Console.WriteLine("Clicked");
}

// C# 2.0 - 익명 메서드
button.Click += delegate(object sender, EventArgs e)
{
    Console.WriteLine("Clicked");
};

// List의 Find 메서드와 함께
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
int firstEven = numbers.Find(delegate(int n) 
{
    return n % 2 == 0;
});
```

**Nullable 값 타입:**

값 타입(int, bool 등)에 null을 할당할 수 있게 되었습니다. 이는 데이터베이스 프로그래밍에서 특히 유용했습니다.

```csharp
// int는 null이 될 수 없음
// int age = null;  // 컴파일 오류

// Nullable<int> 또는 int?
int? age = null;
bool? isActive = null;

// HasValue와 Value 속성
if (age.HasValue)
{
    Console.WriteLine("Age: " + age.Value);
}
else
{
    Console.WriteLine("Age is unknown");
}

// Null 병합 연산자와 함께
int actualAge = age ?? 0;  // null이면 0 사용

// 데이터베이스 쿼리 결과 표현
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public DateTime? DiscontinuedDate { get; set; }  // null 가능
}
```

**반복자(Iterators)와 yield:**

`yield return` 키워드를 통해 커스텀 반복자를 쉽게 구현할 수 있게 되었습니다. 이는 게으른 평가(Lazy Evaluation)의 기반이 되었습니다.

```csharp
// yield return을 사용한 반복자
public class NumberGenerator
{
    public IEnumerable<int> GetNumbers(int start, int count)
    {
        for (int i = 0; i < count; i++)
        {
            yield return start + i;  // 값을 하나씩 반환하고 상태 유지
        }
    }
    
    public IEnumerable<int> GetEvenNumbers(int max)
    {
        for (int i = 0; i <= max; i++)
        {
            if (i % 2 == 0)
            {
                yield return i;
            }
        }
    }
}

// 사용 - 실제로 필요할 때만 값이 생성됨 (지연 실행)
NumberGenerator generator = new NumberGenerator();
foreach (int num in generator.GetNumbers(10, 5))
{
    Console.WriteLine(num);  // 10, 11, 12, 13, 14
    if (num > 12) break;  // 나머지는 생성되지 않음!
}

// 무한 시퀀스도 가능
public IEnumerable<int> GetFibonacci()
{
    int a = 0, b = 1;
    while (true)
    {
        yield return a;
        int temp = a;
        a = b;
        b = temp + b;
    }
}
```

**Partial 클래스:**

클래스 정의를 여러 파일로 분할할 수 있게 되었습니다. 이는 코드 생성기와 디자이너 도구에서 특히 유용합니다.

```csharp
// File1.cs
public partial class Employee
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    
    public string GetFullName()
    {
        return FirstName + " " + LastName;
    }
}

// File2.cs
public partial class Employee
{
    public decimal Salary { get; set; }
    public DateTime HireDate { get; set; }
    
    public int GetYearsOfService()
    {
        return DateTime.Now.Year - HireDate.Year;
    }
}

// 컴파일러는 두 파일을 하나의 클래스로 결합
```

**영향과 의의:**

C# 2.0의 제네릭 도입은 .NET 생태계를 근본적으로 변화시켰습니다:

- **BCL의 재설계**: `List<T>`, `Dictionary<TKey, TValue>` 등 제네릭 컬렉션이 표준이 됨
- **성능 향상**: 박싱/언박싱 제거로 값 타입 처리 성능이 크게 개선됨
- **타입 안전성**: 컴파일 타임 오류 검출로 런타임 버그 감소
- **코드 재사용성**: 하나의 제네릭 클래스로 모든 타입 처리 가능

### C# 3.0 (2007년) - LINQ와 함수형 프로그래밍의 통합

C# 3.0은 2007년 11월 .NET Framework 3.5와 함께 출시되었으며, **LINQ(Language Integrated Query)**라는 혁명적 기능을 도입했습니다. LINQ는 C# 역사상 가장 중요한 기능 중 하나로, 데이터 쿼리를 언어의 일급 시민으로 만들고 함수형 프로그래밍 개념을 C#에 통합했습니다. Erik Meijer가 주도한 이 프로젝트는 SQL의 선언적 스타일과 Haskell 같은 함수형 언어의 강력함을 결합했습니다.

**LINQ 이전의 세상:**

```csharp
// C# 2.0에서 컬렉션 필터링
List<Product> products = GetProducts();
List<Product> expensiveProducts = new List<Product>();

foreach (Product p in products)
{
    if (p.Price > 1000)
    {
        expensiveProducts.Add(p);
    }
}

// 정렬
expensiveProducts.Sort(delegate(Product a, Product b)
{
    return a.Name.CompareTo(b.Name);
});

// 이름만 추출
List<string> names = new List<string>();
foreach (Product p in expensiveProducts)
{
    names.Add(p.Name);
}

// 문제점:
// 1. 보일러플레이트 코드가 너무 많음
// 2. 의도가 명확하지 않음 (구현 세부사항에 묻힘)
// 3. 데이터베이스, XML 등 다른 데이터 소스는 완전히 다른 API 사용
// 4. 조합하기 어려움
```

**LINQ의 혁명:**

```csharp
// C# 3.0 LINQ - 같은 작업을 한 줄로
var expensiveProductNames = products
    .Where(p => p.Price > 1000)
    .OrderBy(p => p.Name)
    .Select(p => p.Name);

// 또는 쿼리 구문으로
var expensiveProductNames = from p in products
                           where p.Price > 1000
                           orderby p.Name
                           select p.Name;

// 장점:
// 1. 간결함 - 의도가 명확하게 드러남
// 2. 선언적 - "무엇을" 할지만 명시
// 3. 조합 가능 - 쿼리를 변수에 저장하고 연결 가능
// 4. 지연 실행 - 필요할 때만 실행
// 5. 타입 안전 - 컴파일 타임 검증
```

**람다 식(Lambda Expressions) - 함수형 프로그래밍의 핵심:**

람다 식은 1930년대 수학자 Alonzo Church의 람다 계산법(λ-calculus)에서 이름을 따왔으며, 익명 함수를 간결하게 표현합니다.

```csharp
// C# 2.0 익명 메서드
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
List<int> evenNumbers = numbers.FindAll(delegate(int n)
{
    return n % 2 == 0;
});

// C# 3.0 람다 식 - 훨씬 간결
List<int> evenNumbers = numbers.FindAll(n => n % 2 == 0);

// 다양한 람다 식 형태
Func<int, int> square = x => x * x;  // 단일 매개변수
Func<int, int, int> add = (x, y) => x + y;  // 여러 매개변수
Action<string> print = s => Console.WriteLine(s);  // 반환값 없음
Func<int, bool> isEven = n =>  // 블록 본문
{
    if (n % 2 == 0)
        return true;
    return false;
};

// 클로저(Closure) - 외부 변수 캡처
int threshold = 100;
var expensive = products.Where(p => p.Price > threshold);
// 람다가 threshold 변수를 "캡처"함
```

**확장 메서드(Extension Methods) - LINQ의 기반:**

확장 메서드는 기존 타입을 수정하지 않고 새 메서드를 추가할 수 있게 합니다. LINQ의 모든 연산자(Where, Select 등)는 확장 메서드로 구현되어 있습니다.

```csharp
// 확장 메서드 정의
public static class StringExtensions
{
    public static bool IsValidEmail(this string email)
    {
        // 간단한 이메일 검증 로직
        return email.Contains("@") && email.Contains(".");
    }
    
    public static string Reverse(this string str)
    {
        char[] chars = str.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
}

// 사용 - 마치 string의 메서드처럼
string email = "user@example.com";
if (email.IsValidEmail())
{
    Console.WriteLine("Valid email");
}

string text = "Hello";
string reversed = text.Reverse();  // "olleH"

// LINQ 연산자도 확장 메서드
public static class Enumerable
{
    public static IEnumerable<T> Where<T>(
        this IEnumerable<T> source, 
        Func<T, bool> predicate)
    {
        foreach (T item in source)
        {
            if (predicate(item))
            {
                yield return item;
            }
        }
    }
}
```

**익명 타입(Anonymous Types) - 즉석 데이터 구조:**

```csharp
// 익명 타입 생성
var person = new { Name = "Alice", Age = 30 };
Console.WriteLine(person.Name);  // "Alice"
Console.WriteLine(person.Age);   // 30

// LINQ 쿼리의 프로젝션에서 유용
var productSummaries = products.Select(p => new
{
    p.Name,
    p.Price,
    IsExpensive = p.Price > 1000,
    Tax = p.Price * 0.1m
});

foreach (var summary in productSummaries)
{
    Console.WriteLine($"{summary.Name}: ${summary.Price} (Tax: ${summary.Tax})");
}

// 여러 데이터 소스 조인
var customerOrders = from c in customers
                     join o in orders on c.Id equals o.CustomerId
                     select new
                     {
                         CustomerName = c.Name,
                         OrderDate = o.Date,
                         OrderTotal = o.Total
                     };
```

**자동 구현 속성(Auto-implemented Properties):**

```csharp
// C# 1.0/2.0 방식 - 필드와 속성을 수동으로 작성
public class Person
{
    private string name;
    private int age;
    
    public string Name
    {
        get { return name; }
        set { name = value; }
    }
    
    public int Age
    {
        get { return age; }
        set { age = value; }
    }
}

// C# 3.0 - 자동 구현 속성
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

// 읽기 전용 속성도 가능
public class Product
{
    public string Name { get; private set; }
    public decimal Price { get; set; }
    
    public Product(string name, decimal price)
    {
        Name = name;
        Price = price;
    }
}
```

**객체 및 컬렉션 초기화 구문:**

```csharp
// C# 2.0 방식
Person person = new Person();
person.Name = "Alice";
person.Age = 30;

List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
numbers.Add(3);

// C# 3.0 - 객체 초기화 구문
Person person = new Person
{
    Name = "Alice",
    Age = 30
};

// 컬렉션 초기화 구문
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// 복잡한 객체 그래프도 간결하게
var company = new Company
{
    Name = "Tech Corp",
    Address = new Address
    {
        Street = "123 Main St",
        City = "Seoul"
    },
    Employees = new List<Employee>
    {
        new Employee { Name = "Alice", Department = "Engineering" },
        new Employee { Name = "Bob", Department = "Sales" }
    }
};
```

**var 키워드와 타입 추론:**

```csharp
// 명시적 타입 선언
Dictionary<string, List<Customer>> customersByCity = 
    new Dictionary<string, List<Customer>>();

// var로 간결하게
var customersByCity = new Dictionary<string, List<Customer>>();

// LINQ 쿼리의 복잡한 익명 타입
var query = from c in customers
            join o in orders on c.Id equals o.CustomerId
            group o by c.City into g
            select new
            {
                City = g.Key,
                TotalOrders = g.Count(),
                TotalRevenue = g.Sum(o => o.Total)
            };
// 타입이 너무 복잡해서 명시적으로 쓰기 어려움!
```

**표현식 트리(Expression Trees) - 코드를 데이터로:**

표현식 트리는 LINQ to SQL, Entity Framework 같은 ORM의 핵심 기술입니다.

```csharp
// 일반 람다 - 실행 가능한 코드로 컴파일됨
Func<int, bool> isEven = x => x % 2 == 0;
bool result = isEven(4);  // true

// 표현식 트리 - 코드 구조를 데이터로 표현
Expression<Func<int, bool>> isEvenExpr = x => x % 2 == 0;

// 표현식 트리를 분석
// isEvenExpr.Body: (x % 2) == 0
// isEvenExpr.Parameters[0]: x
// isEvenExpr.Body.NodeType: Equal

// LINQ to SQL이 하는 일:
// Expression 트리를 분석하여 SQL로 변환
var expensiveProducts = dbContext.Products
    .Where(p => p.Price > 1000);  // SQL로 변환: WHERE Price > 1000
// 데이터베이스에서 필터링됨, 메모리로 모든 데이터를 가져오지 않음!
```

**LINQ 쿼리 구문:**

```csharp
// 메서드 구문
var result = products
    .Where(p => p.Category == "Electronics")
    .OrderBy(p => p.Price)
    .Select(p => new { p.Name, p.Price });

// 쿼리 구문 - SQL과 유사
var result = from p in products
             where p.Category == "Electronics"
             orderby p.Price
             select new { p.Name, p.Price };

// 복잡한 쿼리
var customerOrderSummary = 
    from c in customers
    join o in orders on c.Id equals o.CustomerId
    where o.Date.Year == 2007
    group o by c.Name into g
    let totalSpent = g.Sum(o => o.Total)
    where totalSpent > 5000
    orderby totalSpent descending
    select new
    {
        CustomerName = g.Key,
        OrderCount = g.Count(),
        TotalSpent = totalSpent
    };
```

**C# 3.0의 영향:**

C# 3.0은 언어의 패러다임을 근본적으로 변화시켰습니다:

1. **함수형 프로그래밍의 통합**: 람다, 고차 함수, 불변성 개념 도입
2. **선언적 프로그래밍**: "어떻게"가 아닌 "무엇을" 표현
3. **데이터 중심 프로그래밍**: LINQ를 통한 통합된 데이터 접근
4. **생산성 향상**: 보일러플레이트 코드 대폭 감소
5. **타입 안전성 유지**: 강력한 타입 시스템을 유지하면서 유연성 확보

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
