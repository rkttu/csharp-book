# 14장. LINQ 기초

프로그래밍 언어의 역사에서 데이터 쿼리(Data Query)의 통합은 오랫동안 해결되지 않은 근본적인 문제였습니다. 1970년대 Edgar F. Codd의 관계형 모델(Relational Model) 제안과 이를 구현한 SQL(Structured Query Language)의 등장 이후, 데이터베이스 쿼리는 선언적(Declarative) 스타일의 강력함을 보여주었습니다. 그러나 일반 프로그래밍 언어에서 컬렉션을 다루는 방식은 여전히 명령형(Imperative) 패러다임에 머물러 있었습니다. 개발자들은 데이터베이스를 다룰 때는 SQL을, 메모리 상의 컬렉션을 다룰 때는 반복문을, XML을 다룰 때는 또 다른 API를 사용해야 했으며, 이는 코드의 일관성을 저해하고 학습 곡선을 가파르게 만들었습니다.

**LINQ(Language Integrated Query)**는 이러한 문제를 해결하기 위해 .NET 3.5와 C# 3.0에서 2007년에 도입된 혁명적인 기능입니다. Microsoft Research의 수석 연구원이자 함수형 프로그래밍의 대가인 Erik Meijer가 주도한 이 프로젝트는, SQL의 선언적 쿼리 스타일과 Haskell, ML 같은 함수형 언어의 강력함을 C#에 통합하여, 다양한 데이터 소스를 일관된 방식으로 다룰 수 있게 했습니다. LINQ는 단순히 편리한 문법적 설탕(Syntactic Sugar)을 넘어서, 프로그래밍 패러다임의 근본적인 변화를 가져왔습니다.

**LINQ의 역사적 맥락과 진화:**

LINQ의 개념은 하루아침에 탄생한 것이 아니라, 컴퓨터 과학의 여러 분야에서 축적된 지식의 결정체입니다:

1. **1970년대 - 관계형 모델의 등장**: Edgar F. Codd의 관계형 모델은 데이터를 수학적 집합(Set)으로 다루는 선언적 접근을 제시했습니다. SQL은 이를 실용화하여, "무엇을(What)" 가져올지만 명시하면 "어떻게(How)" 가져올지는 데이터베이스 엔진이 최적화하는 혁신을 가져왔습니다.

2. **1980-90년대 - 함수형 프로그래밍의 성숙**: Lisp, Scheme, ML, Haskell 등의 함수형 언어는 고차 함수(Higher-order Functions), 람다 계산법(Lambda Calculus), 타입 시스템의 이론적 기반을 확립했습니다. 특히 map, filter, reduce 같은 리스트 처리 함수들은 LINQ 연산자의 직접적인 영감이 되었습니다.

3. **1990년대 후반 - ORM의 등장**: Hibernate(Java), ActiveRecord(Ruby) 같은 객체-관계 매핑(Object-Relational Mapping) 도구들은 데이터베이스 레코드를 객체로 다루려는 시도를 보여주었지만, 타입 안정성과 컴파일 타임 검증의 부족이라는 한계가 있었습니다.

4. **2000년대 초반 - 모나드와 표현식 트리**: Haskell의 모나드(Monad) 개념과 Lisp의 매크로 시스템이 보여준 "코드를 데이터로 다루기(Code as Data)"는 LINQ의 표현식 트리(Expression Trees) 설계에 큰 영향을 미쳤습니다.

5. **2007년 - LINQ의 탄생**: 이러한 모든 개념들이 융합되어 LINQ가 탄생했습니다. Erik Meijer는 범주론(Category Theory)의 개념을 활용하여, 다양한 데이터 소스를 추상화하는 통합된 쿼리 모델을 설계했습니다.

**LINQ 이전 시대의 문제점:**

LINQ가 해결하려 했던 근본적인 문제들을 구체적으로 살펴보면:

```csharp
// LINQ 이전: 데이터베이스 쿼리 (문자열 기반, 타입 불안정)
string sql = "SELECT * FROM Customers WHERE City = 'Seoul'";
// 문제점:
// 1. 컴파일 타임에 오류 검출 불가 (오타, 컬럼명 변경 등)
// 2. IntelliSense 지원 없음
// 3. SQL Injection 공격에 취약
// 4. 결과를 수동으로 객체에 매핑해야 함

// 메모리 컬렉션 처리 (명령형, 반복적)
List<Customer> seoulCustomers = new List<Customer>();
foreach (Customer c in allCustomers)
{
    if (c.City == "Seoul")
    {
        seoulCustomers.Add(c);
    }
}
// 문제점:
// 1. 보일러플레이트 코드 과다
// 2. 의도가 명확하지 않음 (구현 세부사항에 집중)
// 3. 실수하기 쉬움 (인덱스 오류, 누락 등)
// 4. 병렬 처리가 어려움

// XML 처리 (복잡한 API)
XmlDocument doc = new XmlDocument();
doc.Load("data.xml");
XmlNodeList nodes = doc.SelectNodes("//Customer[@City='Seoul']");
// 문제점:
// 1. DOM API의 복잡성
// 2. XPath 문자열의 타입 불안정성
// 3. 다른 데이터 소스와 일관성 없음
```

**LINQ의 철학적 기반:**

LINQ는 다음과 같은 핵심 설계 원칙을 기반으로 합니다:

1. **통합성(Integration)**: 쿼리가 언어의 일급 구성 요소(First-class Construct)가 되어야 합니다. 이는 문자열로 쿼리를 작성하는 것이 아니라, 컴파일러가 이해하고 검증할 수 있는 코드로 작성함을 의미합니다. 이를 통해 리팩토링 도구, IntelliSense, 컴파일 타임 오류 검출 등 언어의 모든 인프라를 활용할 수 있습니다.

2. **타입 안정성(Type Safety)**: 쿼리의 모든 부분이 강타입(Strongly-typed)이어야 합니다. 이는 런타임 오류를 컴파일 타임으로 이동시켜, 버그를 조기에 발견하고 수정 비용을 획기적으로 줄입니다. 이는 C#의 정적 타입 시스템(Static Type System)의 강점을 극대화합니다.

3. **선언적 프로그래밍(Declarative Programming)**: "무엇을(What)" 할지만 명시하고, "어떻게(How)"는 라이브러리나 런타임에 위임합니다. 이는 관계형 데이터베이스의 쿼리 최적화기(Query Optimizer)처럼, LINQ 프로바이더가 최적의 실행 계획을 선택할 수 있게 합니다. 예를 들어, LINQ to SQL은 C# 쿼리를 최적화된 SQL로 변환하고, PLINQ는 자동으로 병렬화합니다.

4. **지연 실행(Deferred Execution)**: 쿼리 정의와 실행을 분리합니다. 이는 Haskell의 게으른 평가(Lazy Evaluation)에서 영감을 받은 개념으로, 불필요한 계산을 피하고 쿼리 조합(Query Composition)을 가능하게 합니다. 쿼리를 변수에 저장하고, 다른 쿼리와 결합하고, 필요한 시점에 실행할 수 있습니다.

5. **합성 가능성(Composability)**: 작은 쿼리들을 조합하여 복잡한 쿼리를 만들 수 있어야 합니다. 이는 함수형 프로그래밍의 함수 합성(Function Composition) 개념을 쿼리에 적용한 것입니다. LINQ 연산자는 모두 `IEnumerable<T>`를 입력으로 받고 출력으로 반환하여, 무한히 체이닝할 수 있습니다.

**LINQ의 기술적 기반 - C# 3.0의 혁신:**

LINQ는 C# 3.0에서 도입된 여러 혁신적인 언어 기능들의 시너지로 작동합니다. 이러한 기능들은 각각 독립적으로도 유용하지만, LINQ라는 하나의 목표를 위해 조화롭게 설계되었습니다:

1. **람다 식(Lambda Expressions)**: Alonzo Church의 람다 계산법을 C#에 도입하여, 익명 함수를 간결하게 표현합니다. `x => x * 2`와 같은 표현은 수학의 λx.x×2와 동일한 의미를 가지며, 코드를 데이터처럼 다룰 수 있게 합니다.

2. **확장 메서드(Extension Methods)**: 기존 타입(`IEnumerable<T>`)을 수정하지 않고도 새 메서드를 추가할 수 있게 합니다. 이는 SOLID 원칙의 개방-폐쇄 원칙(Open-Closed Principle)을 실현하며, LINQ의 모든 연산자(`Where`, `Select` 등)는 확장 메서드로 구현되어 있습니다.

3. **익명 타입(Anonymous Types)**: 일회성 데이터 구조를 즉석에서 정의할 수 있게 합니다. `new { Name = "John", Age = 30 }`처럼 쿼리 결과를 원하는 형태로 투영(Projection)할 때 매우 유용하며, 보일러플레이트 클래스 정의를 제거합니다.

4. **표현식 트리(Expression Trees)**: 코드를 컴파일 가능한 데이터 구조로 표현합니다. `Expression<Func<int, bool>>`는 람다 식을 AST(Abstract Syntax Tree)로 만들어, 런타임에 분석하고 다른 언어(예: SQL)로 변환할 수 있게 합니다. 이는 Lisp의 호모아이코닉(Homoiconic) 특성을 C#에 부분적으로 도입한 것입니다.

5. **타입 추론(Type Inference)**: `var` 키워드로 복잡한 타입 선언을 생략할 수 있게 합니다. LINQ 쿼리는 종종 복잡한 제네릭 타입을 반환하므로(`IEnumerable<IGrouping<string, Customer>>` 등), 타입 추론은 코드 가독성을 크게 향상시킵니다.

6. **쿼리 구문(Query Syntax)**: SQL과 유사한 선언적 문법을 C#에 직접 통합합니다. 이는 단순한 문법적 설탕이 아니라, 컴파일러가 메서드 호출로 변환하는 **쿼리 컴프리헨션(Query Comprehension)**입니다. Haskell의 do-notation이나 Scala의 for-comprehension과 유사한 개념입니다.

**LINQ의 아키텍처와 공급자 모델:**

LINQ는 3계층 아키텍처로 설계되어 있습니다:

```
[쿼리 표현 계층]
  ↓ 컴파일러 변환
[표준 쿼리 연산자 계층] (IEnumerable<T> / IQueryable<T>)
  ↓ 공급자 구현
[실행 계층] (메모리, SQL, XML, 외부 서비스 등)
```

이러한 계층화된 설계는 **공급자 모델(Provider Model)**을 가능하게 합니다. 각 LINQ 공급자(Provider)는 표준 쿼리 연산자를 특정 데이터 소스에 맞게 구현합니다:

- **LINQ to Objects**: `IEnumerable<T>` 기반, 메모리 내 컬렉션을 직접 순회하며 처리
- **LINQ to SQL / Entity Framework**: `IQueryable<T>` 기반, 표현식 트리를 SQL로 변환하여 데이터베이스에 전송
- **LINQ to XML**: `XDocument`, `XElement` 등의 특화된 타입으로 XML 트리 탐색
- **PLINQ**: 자동으로 쿼리를 병렬화하여 멀티코어 CPU 활용

이러한 공급자들은 모두 동일한 LINQ 구문을 사용하지만, 내부적으로는 각 데이터 소스에 최적화된 방식으로 실행됩니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 LINQ(Language Integrated Query)의 핵심 개념과 실전 활용법을 체계적으로 학습하게 됩니다. LINQ는 단순한 편의 기능이 아니라, 데이터 처리 패러다임의 근본적인 변화를 가져온 혁신적인 기술입니다. 이 장의 내용을 완전히 이해하면, 더 간결하고 표현력 있으며 유지보수하기 쉬운 코드를 작성할 수 있게 됩니다.

- **LINQ의 개념과 원리의 심층 이해**: LINQ가 무엇이며 왜 프로그래밍 역사에서 중요한 이정표인지, 어떤 실무 문제를 해결하는지 이해합니다. 명령형 프로그래밍에서 선언적 프로그래밍으로의 패러다임 전환, 타입 안정성의 중요성, 그리고 지연 실행(Deferred Execution)의 의미와 장점을 배웁니다.

- **쿼리 구문과 메서드 구문의 완벽한 이해**: LINQ를 표현하는 두 가지 방식인 쿼리 구문(Query Syntax)과 메서드 구문(Method Syntax)의 차이점과 각각의 장단점을 배웁니다. 컴파일러가 쿼리 구문을 메서드 호출로 변환하는 과정을 이해하고, 상황에 따라 적절한 구문을 선택하는 능력을 기릅니다.

- **기본 LINQ 연산자의 숙달**: 데이터 처리의 핵심인 필터링(Where), 투영(Select), 정렬(OrderBy/OrderByDescending) 연산자들을 깊이 있게 학습합니다. 각 연산자의 내부 동작 원리, 시간 복잡도, 성능 특성을 이해하고, 실무에서 자주 마주치는 다양한 시나리오에 적용하는 방법을 익힙니다.

- **집계 연산자를 통한 데이터 요약**: Count, Sum, Average, Min, Max, Aggregate 등 데이터를 단일 값으로 축약하는 연산자들을 학습합니다. 특히 범용 집계 연산자인 Aggregate의 활용법과, 함수형 프로그래밍의 reduce/fold 개념과의 연결을 이해합니다.

**학습 목표:**
- 선언적 프로그래밍 스타일로 데이터 처리 로직을 표현하는 능력 배양
- 지연 실행과 즉시 실행의 차이를 이해하고 적절히 활용
- 복잡한 데이터 변환 파이프라인을 LINQ 메서드 체이닝으로 구축
- 코드의 가독성과 유지보수성을 향상시키는 LINQ 활용 패턴 습득
- 15장의 고급 LINQ 기능을 학습하기 위한 탄탄한 기초 확립

---

## 14.1 LINQ란?

LINQ(Language Integrated Query, 링크)는 C#과 .NET에 내장된 강력한 쿼리 기능으로, 다양한 데이터 소스에 대해 일관된 방식으로 쿼리를 작성할 수 있게 해줍니다. "Language Integrated"라는 이름이 시사하듯, LINQ는 단순히 라이브러리가 아니라 언어의 핵심 기능으로 통합되어 있으며, 컴파일러, 타입 시스템, IDE가 모두 LINQ를 완전히 지원합니다. 이는 SQL 스타일의 선언적 구문으로 컬렉션, 데이터베이스, XML 등을 쉽게 조회하고 변환할 수 있게 하며, 데이터 처리 코드의 패러다임을 근본적으로 변화시킵니다.

**LINQ의 본질적 가치:**

LINQ의 진정한 혁신은 단순히 편리한 문법을 제공하는 것을 넘어서, 다음과 같은 근본적인 가치를 실현한 데 있습니다:

1. **타입 안정성(Type Safety)과 컴파일 타임 검증**: SQL 문자열이나 XPath 표현식은 런타임에만 오류를 발견할 수 있지만, LINQ 쿼리는 컴파일 시점에 모든 타입 오류, 멤버 접근 오류, 메서드 시그니처 불일치 등을 검출합니다. 이는 "Fail Fast" 원칙을 실현하여, 버그 수정 비용을 극적으로 줄입니다. Martin Fowler가 제시한 "리팩터링(Refactoring)"도 타입 안정성이 있어야 안전하게 수행할 수 있습니다.

2. **IntelliSense와 도구 지원**: Visual Studio와 VS Code는 LINQ 쿼리 작성 시 실시간으로 사용 가능한 연산자, 속성, 메서드를 제안합니다. 이는 API 문서를 찾아볼 필요를 줄이고, 개발 속도를 크게 향상시킵니다. 특히 복잡한 도메인 객체를 다룰 때, IntelliSense는 객체의 구조를 탐색하는 강력한 도구가 됩니다.

3. **코드 간결성과 표현력**: LINQ는 복잡한 데이터 처리 로직을 매우 간결하게 표현할 수 있게 합니다. 20-30줄의 명령형 코드가 3-4줄의 LINQ 쿼리로 축약되는 경우가 흔하며, 더 중요한 것은 코드의 **의도(Intent)**가 명확해진다는 점입니다. "어떻게(How)"가 아닌 "무엇을(What)"에 집중하게 되어, 코드 리뷰와 유지보수가 훨씬 수월해집니다.

4. **쿼리의 재사용성과 조합**: LINQ 쿼리는 일급 객체(First-class Object)입니다. 변수에 저장하고, 매개변수로 전달하고, 다른 쿼리와 조합할 수 있습니다. 이는 **쿼리 조합(Query Composition)**을 가능하게 하여, 작은 재사용 가능한 쿼리들로 복잡한 로직을 구축할 수 있게 합니다. 이는 함수형 프로그래밍의 핵심 원칙인 합성 가능성(Composability)을 데이터 쿼리에 적용한 것입니다.

5. **지연 실행을 통한 성능 최적화**: 대부분의 LINQ 연산자는 지연 실행(Deferred Execution)됩니다. 쿼리를 정의하는 것만으로는 아무 일도 일어나지 않으며, 실제로 결과를 열거(Enumerate)할 때 실행됩니다. 이는 불필요한 계산을 피하고, 쿼리를 조합할 때 중간 결과를 만들지 않아 메모리를 절약합니다. 또한 데이터베이스 공급자의 경우, 여러 LINQ 연산을 하나의 최적화된 SQL 쿼리로 변환할 수 있습니다.

**LINQ의 생태계 - 다양한 공급자:**

LINQ의 공급자 모델(Provider Model)은 동일한 쿼리 구문으로 다양한 데이터 소스를 다룰 수 있게 합니다. 각 공급자는 표준 쿼리 연산자를 자신의 데이터 소스에 최적화된 방식으로 구현합니다:

- **LINQ to Objects**: 메모리 상의 컬렉션(List, Array, IEnumerable 등)을 쿼리합니다. 이는 가장 기본적이고 자주 사용되는 LINQ 형태로, `IEnumerable<T>` 인터페이스를 구현한 모든 컬렉션에서 동작합니다. 이 장에서 주로 다루는 내용이며, 모든 LINQ의 기초가 됩니다.

- **LINQ to SQL / Entity Framework**: 관계형 데이터베이스를 쿼리합니다. 놀랍게도, LINQ 쿼리가 SQL로 변환되어 데이터베이스에 전송됩니다. 이는 표현식 트리(Expression Trees)를 분석하여 SQL 문을 생성하는 방식으로 작동하며, ORM(Object-Relational Mapping)의 강력한 기능을 제공합니다. 예를 들어, `customers.Where(c => c.City == "Seoul")`은 `SELECT * FROM Customers WHERE City = 'Seoul'`로 변환됩니다.

- **LINQ to XML**: XML 문서를 쿼리합니다. 전통적인 DOM API나 XPath에 비해 훨씬 직관적이고 타입 안전한 방식으로 XML을 탐색하고 조작할 수 있습니다. `XDocument`, `XElement` 등의 타입과 함께 사용하여, 계층적 XML 구조를 쉽게 다룹니다.

- **Parallel LINQ (PLINQ)**: 표준 LINQ 쿼리를 자동으로 병렬화하여 멀티코어 CPU를 활용합니다. 단순히 `.AsParallel()`을 추가하는 것만으로 데이터 처리를 여러 스레드에 분산시킬 수 있으며, Task Parallel Library(TPL)를 기반으로 작동합니다. 대용량 데이터 처리 시 성능을 크게 향상시킬 수 있습니다.

- **LINQ to Entities**: Entity Framework Core에서 사용하는 공급자로, LINQ to SQL보다 더 발전된 형태입니다. 더 복잡한 쿼리 최적화, 변경 추적(Change Tracking), 지연 로딩(Lazy Loading) 등의 고급 기능을 제공합니다.

- **커스텀 LINQ 공급자**: `IQueryable<T>` 인터페이스를 구현하여 자신만의 LINQ 공급자를 만들 수 있습니다. 실제로 MongoDB, ElasticSearch, Azure Cosmos DB 등 많은 NoSQL 데이터베이스들이 LINQ 공급자를 제공하여, 동일한 C# 구문으로 다양한 데이터 소스를 쿼리할 수 있게 합니다.

**전통적인 방식 vs LINQ - 심층 비교:**

LINQ 이전과 이후의 코드를 비교하면, 패러다임의 차이가 명확히 드러납니다:

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// ============================================
// 전통적인 명령형 방식 (Imperative Style)
// ============================================
// "어떻게(How)" 하는지에 집중
// 단계별로 명시적으로 제어 흐름을 기술

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

// 문제점 분석:
// 1. 보일러플레이트 코드: 빈 리스트 생성, foreach, if, Add 등 반복적 패턴
// 2. 가변 상태(Mutable State): evenNumbers 리스트가 점진적으로 변경됨
// 3. 구현 세부사항 노출: 반복문, 조건문 등 "어떻게" 구현되는지가 드러남
// 4. 오류 가능성: 잘못된 리스트에 추가, 조건 오류 등 실수 가능
// 5. 병렬화 어려움: 코드를 완전히 재작성해야 함

// ============================================
// LINQ 선언적 방식 (Declarative Style)
// ============================================
// "무엇을(What)" 원하는지에 집중
// 원하는 결과의 특성만 명시

var evenNumbersLinq = numbers.Where(n => n % 2 == 0);
Console.WriteLine("짝수: " + string.Join(", ", evenNumbersLinq));
// 출력: 짝수: 2, 4, 6, 8, 10

// 장점 분석:
// 1. 코드 간결성: 한 줄로 의도를 명확히 표현
// 2. 불변성(Immutability): 원본 컬렉션은 변경되지 않음
// 3. 선언적 표현: "짝수를 선택한다"는 의도가 코드에 직접 드러남
// 4. 타입 안정성: 컴파일러가 n의 타입과 % 연산의 유효성을 검증
// 5. 쉬운 병렬화: .AsParallel()만 추가하면 자동으로 병렬 처리
```

**더 복잡한 예제로 차이 확인:**

```csharp
class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }
}

List<Product> products = new List<Product>
{
    new Product { Name = "노트북", Price = 1500000, Category = "전자제품" },
    new Product { Name = "마우스", Price = 35000, Category = "전자제품" },
    new Product { Name = "의자", Price = 250000, Category = "가구" },
    new Product { Name = "책상", Price = 450000, Category = "가구" },
    new Product { Name = "키보드", Price = 120000, Category = "전자제품" }
};

// ============================================
// 전통적인 방식: 50만원 미만 전자제품, 가격순 정렬
// ============================================
List<Product> affordableElectronics = new List<Product>();
foreach (Product p in products)
{
    if (p.Category == "전자제품" && p.Price < 500000)
    {
        affordableElectronics.Add(p);
    }
}

// 정렬을 위한 별도 단계
affordableElectronics.Sort((p1, p2) => p1.Price.CompareTo(p2.Price));

foreach (Product p in affordableElectronics)
{
    Console.WriteLine($"{p.Name}: {p.Price:C0}");
}
// 출력:
// 마우스: ₩35,000
// 키보드: ₩120,000

// ============================================
// LINQ 방식: 체이닝을 통한 우아한 표현
// ============================================
var affordableElectronicsLinq = products
    .Where(p => p.Category == "전자제품")
    .Where(p => p.Price < 500000)
    .OrderBy(p => p.Price);

foreach (Product p in affordableElectronicsLinq)
{
    Console.WriteLine($"{p.Name}: {p.Price:C0}");
}
// 출력:
// 마우스: ₩35,000
// 키보드: ₩120,000

// 또는 더 간결하게:
var result = products
    .Where(p => p.Category == "전자제품" && p.Price < 500000)
    .OrderBy(p => p.Price)
    .Select(p => $"{p.Name}: {p.Price:C0}");

Console.WriteLine(string.Join("\n", result));
```

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
