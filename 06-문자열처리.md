# 6장. 문자열 처리

텍스트 처리는 컴퓨터 과학의 역사에서 가장 오래되고도 보편적인 작업 중 하나입니다. 1960년대 초기 컴퓨터 시스템에서 문자 인코딩 표준이 정립되기 시작한 이래로, 문자열(String) 처리는 모든 프로그래밍 언어의 핵심 기능으로 자리잡았습니다. ASCII 코드 체계에서 시작하여 현대의 유니코드(Unicode) 표준에 이르기까지, 문자열 처리 기술은 인류의 모든 문자 체계를 디지털로 표현하고 조작하는 능력으로 발전해 왔습니다.

현대 소프트웨어 개발에서 문자열은 단순한 텍스트 표현을 넘어서, 사용자 인터페이스, 데이터 직렬화, 네트워크 프로토콜, 로그 메시지, 설정 파일, 데이터베이스 쿼리 등 거의 모든 영역에서 중추적인 역할을 담당합니다. 웹 개발에서의 HTML/XML 파싱, 데이터 과학에서의 텍스트 마이닝, 자연어 처리(NLP) 애플리케이션에 이르기까지, 효율적이고 정확한 문자열 처리 능력은 현대 프로그래머에게 필수불가결한 기술입니다.

C#의 문자열 시스템은 .NET 플랫폼의 강력한 타입 시스템과 메모리 관리 메커니즘을 기반으로 설계되었습니다. `System.String` 클래스는 **불변(Immutable)** 참조 타입으로 구현되어 있는데, 이는 함수형 프로그래밍의 불변성(Immutability) 원칙과 멀티스레드 환경에서의 안전성을 동시에 달성하기 위한 의도적인 설계 결정입니다. 한 번 생성된 문자열 객체의 내용은 절대 변경될 수 없으며, 모든 수정 연산은 새로운 문자열 객체를 생성하여 반환합니다.

이러한 불변성 설계는 여러 중요한 이점을 제공합니다. 첫째, **스레드 안전성(Thread Safety)**이 자동으로 보장됩니다. 여러 스레드가 동시에 같은 문자열을 읽어도 데이터 경쟁(Race Condition)이 발생하지 않으므로, 복잡한 동기화 메커니즘 없이도 안전하게 문자열을 공유할 수 있습니다. 둘째, **문자열 인터닝(String Interning)** 최적화가 가능해집니다. 동일한 내용의 문자열 리터럴은 메모리상의 같은 객체를 참조하도록 하여 메모리 사용량을 최소화할 수 있습니다. 셋째, **캐싱과 해시 테이블** 활용이 안전해집니다. 문자열의 해시 코드를 한 번 계산하여 캐시해 둘 수 있으며, 이는 딕셔너리 키로 사용될 때 성능 향상을 가져옵니다.

하지만 불변성에는 트레이드오프가 존재합니다. 빈번한 문자열 수정이 필요한 경우, 매번 새로운 객체를 생성하고 기존 객체를 가비지 컬렉션(Garbage Collection)하는 오버헤드가 발생합니다. 특히 반복문 내에서 문자열을 연결하는 작업은 O(n²)의 시간 복잡도를 가지게 되어 성능 병목 현상을 일으킬 수 있습니다. 이러한 시나리오를 위해 .NET은 `StringBuilder` 클래스를 제공하는데, 이는 가변(Mutable) 문자 버퍼를 사용하여 효율적인 문자열 조작을 가능하게 합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 C#의 문자열 처리 시스템을 깊이 있게 이해하고, 다양한 실무 상황에서 최적의 문자열 처리 기법을 선택할 수 있는 능력을 갖추게 됩니다:

- **문자열 기본 연산의 이론과 실제**: 문자열 연결의 다양한 방법론(+, 보간, `Concat()`, `Join()`)과 각각의 성능 특성, 문자열 비교의 여러 차원(서수, 문화권, 대소문자), 그리고 유니코드 환경에서의 길이 계산 등 문자열의 기초를 탄탄히 다집니다.

- **문자열 메서드 생태계**: .NET의 `String` 클래스가 제공하는 풍부한 메서드들(`Substring()`, `Replace()`, `Split()`, `Trim()`, `ToUpper()`, `ToLower()` 등)의 내부 동작 원리와 실무 활용 패턴을 학습하며, 각 메서드의 시간 복잡도와 메모리 영향을 이해합니다.

- **StringBuilder의 내부 메커니즘**: 가변 문자 버퍼의 작동 원리, 용량(Capacity) 관리 전략, 그리고 언제 `String` 대신 `StringBuilder`를 사용해야 하는지에 대한 성능 기반 의사결정 방법론을 습득합니다.

---

## 6.1 문자열 기본 연산

문자열 처리의 가장 기본이 되는 연산들은 모든 고수준 텍스트 처리 작업의 토대입니다. 문자열 연결(Concatenation), 비교(Comparison), 그리고 길이 계산(Length Calculation)은 단순해 보이지만, 각각이 내부적으로 복잡한 알고리즘과 메모리 관리 전략을 포함하고 있습니다. 이러한 기본 연산들을 깊이 이해하는 것은 효율적이고 안전한 문자열 처리 코드를 작성하는 첫걸음입니다.

### 6.1.1 문자열 연결

문자열 연결은 두 개 이상의 문자열 조각을 하나의 연속된 문자열로 결합하는 작업으로, 텍스트 처리에서 가장 빈번하게 수행되는 연산 중 하나입니다. C#의 불변 문자열 특성으로 인해, 각 연결 연산은 새로운 문자열 객체를 힙(Heap) 메모리에 할당하고, 원본 문자열들의 내용을 복사한 후 연결하는 과정을 거칩니다. 따라서 연결 방법의 선택은 단순한 문법적 취향을 넘어서, 프로그램의 성능과 메모리 사용 패턴에 직접적인 영향을 미치는 중요한 의사결정입니다.

C#은 문자열 연결을 위한 다양한 방법론을 제공하며, 각 방법은 특정 사용 사례에 최적화되어 있습니다. 가장 직관적인 `+` 연산자부터 현대적인 문자열 보간(String Interpolation), 그리고 다중 문자열을 효율적으로 처리하는 `String.Concat()`과 `String.Join()`에 이르기까지, 상황에 맞는 적절한 도구를 선택하는 것이 중요합니다.

**+ 연산자 사용:**

가장 직관적인 방법은 `+` 연산자를 사용하는 것입니다. C#은 연산자 오버로딩(Operator Overloading)을 통해 `+` 연산자가 문자열에 대해 연결 연산을 수행하도록 정의하고 있습니다. 이는 수학적 덧셈 연산과의 자연스러운 유추를 제공하여 직관성을 높입니다.

그러나 `+` 연산자의 내부 동작을 이해하는 것이 중요합니다. 두 문자열을 연결할 때, .NET 런타임은 다음과 같은 과정을 거칩니다:
1. 두 문자열의 길이를 합산하여 새로운 문자열에 필요한 메모리 크기를 계산합니다.
2. 힙 메모리에 계산된 크기만큼 새로운 메모리 블록을 할당합니다.
3. 첫 번째 문자열의 내용을 새 메모리 블록에 복사합니다.
4. 두 번째 문자열의 내용을 첫 번째 문자열 뒤에 이어서 복사합니다.
5. 새로 생성된 문자열 객체의 참조를 반환합니다.

이 과정은 두 문자열의 연결에서는 효율적이지만, 연속적인 연결(`a + b + c + d`)의 경우 중간에 임시 문자열 객체들이 생성되어 메모리 압력을 증가시킬 수 있습니다. 다행히 C# 컴파일러는 리터럴 문자열의 연속 연결을 컴파일 시점에 최적화하여 단일 문자열로 병합합니다.

```csharp
string firstName = "김";
string lastName = "철수";
string fullName = firstName + lastName;

Console.WriteLine(fullName);  // 출력: 김철수

// 여러 문자열 연결
string greeting = "안녕하세요, " + fullName + "님!";
Console.WriteLine(greeting);  // 출력: 안녕하세요, 김철수님!
```

**문자열 보간 (String Interpolation):**

C# 6.0에서 도입된 문자열 보간(String Interpolation)은 문자열 연결의 패러다임을 근본적으로 변화시킨 혁신적인 기능입니다. `$` 접두사로 시작하는 보간 문자열은 컴파일 시점에 `String.Format()` 호출로 변환되거나, 더 최적화된 경우 직접적인 연결 코드로 변환됩니다.

문자열 보간의 핵심 이점은 가독성과 유지보수성입니다. 전통적인 `String.Format("{0}님의 나이는 {1}세입니다", name, age)` 방식은 플레이스홀더 인덱스와 실제 변수의 대응을 머릿속으로 추적해야 하는 인지적 부담을 줍니다. 반면 보간 문자열 `$"{name}님의 나이는 {age}세입니다"`는 변수가 사용되는 위치에 직접 표현되어 코드의 의도가 즉각적으로 드러납니다.

컴파일러 최적화 관점에서 보면, C# 10부터 도입된 개선된 문자열 보간은 `DefaultInterpolatedStringHandler`를 사용하여 메모리 할당을 최소화합니다. 이는 스택 할당(Stack Allocation)과 스팬(Span) 기반 처리를 활용하여, 특히 구조체 타입의 값을 문자열로 변환할 때 박싱(Boxing) 오버헤드를 회피합니다.

```csharp
string firstName = "이";
string lastName = "영희";
int age = 25;

string message = $"{firstName}{lastName}님의 나이는 {age}세입니다.";
Console.WriteLine(message);
// 출력: 이영희님의 나이는 25세입니다.

// 표현식 사용 가능
string result = $"내년 나이: {age + 1}세";
Console.WriteLine(result);  // 출력: 내년 나이: 26세
```

**String.Concat() 메서드:**

`String.Concat()` 메서드는 문자열 연결을 위한 저수준 API로, `+` 연산자의 내부 구현체이기도 합니다. 이 메서드는 여러 오버로드(Overload)를 제공하여 다양한 입력 형식을 효율적으로 처리합니다.

`String.Concat()`의 중요한 특징은 가변 인자(params) 오버로드를 제공한다는 점입니다. 이는 임의의 개수의 문자열을 단일 호출로 연결할 수 있게 하며, 내부적으로는 모든 입력 문자열의 총 길이를 먼저 계산한 후 한 번의 메모리 할당으로 최종 결과를 생성합니다. 이는 연속적인 `+` 연산보다 훨씬 효율적인 접근 방식입니다.

또한 `String.Concat()`은 `IEnumerable<string>` 오버로드를 제공하여, LINQ 쿼리 결과나 컬렉션을 직접 연결할 수 있습니다. 이는 함수형 프로그래밍 패턴과의 통합을 용이하게 합니다.

```csharp
string str1 = "Hello";
string str2 = " ";
string str3 = "World";
string result = String.Concat(str1, str2, str3);

Console.WriteLine(result);  // 출력: Hello World

// 배열의 모든 문자열 연결
string[] words = { "C#", "은", "강력한", "언어입니다" };
string sentence = String.Concat(words);
Console.WriteLine(sentence);  // 출력: C#은강력한언어입니다
```

**String.Join() 메서드:**

`String.Join()` 메서드는 구분자(Separator) 기반 문자열 연결의 표준 구현체로, CSV 파일 생성, 경로 조합, 쿼리 문자열 구성 등 실무에서 매우 빈번하게 사용됩니다. 이 메서드의 핵심 가치는 구분자 삽입 로직을 자동화하여 오프바이원(Off-by-One) 오류를 방지한다는 점입니다.

내부 구현 관점에서 `String.Join()`은 매우 효율적으로 설계되어 있습니다. 먼저 모든 요소의 문자열 표현을 평가하고 총 길이를 계산한 후, 정확한 크기의 메모리를 한 번에 할당합니다. 그 다음 각 요소와 구분자를 순차적으로 복사하여 최종 결과를 생성합니다. 이는 O(n) 시간 복잡도와 O(1) 추가 메모리 할당(최종 결과 제외)을 달성하는 최적의 알고리즘입니다.

`String.Join()`은 제네릭 오버로드를 제공하여 `IEnumerable<T>` 형식의 컬렉션을 처리할 수 있으며, 각 요소에 대해 `ToString()` 메서드를 자동으로 호출합니다. 이는 정수 배열이나 사용자 정의 객체의 컬렉션도 직접 문자열로 변환할 수 있음을 의미합니다.

```csharp
string[] fruits = { "사과", "바나나", "오렌지" };

string result1 = String.Join(", ", fruits);
Console.WriteLine(result1);  // 출력: 사과, 바나나, 오렌지

string result2 = String.Join(" / ", fruits);
Console.WriteLine(result2);  // 출력: 사과 / 바나나 / 오렌지

// 숫자 배열도 가능
int[] numbers = { 1, 2, 3, 4, 5 };
string numberStr = String.Join("-", numbers);
Console.WriteLine(numberStr);  // 출력: 1-2-3-4-5
```

### 6.1.2 문자열 비교

문자열 비교는 단순히 두 문자열이 "같은지"를 판단하는 것을 넘어서, 언어학적, 문화적, 그리고 컴퓨터 과학적 복잡성을 모두 포괄하는 다층적인 작업입니다. 유니코드 표준의 등장과 전 세계 언어의 디지털화로 인해, 문자열 비교는 알파벳의 이진 코드 비교라는 단순한 개념에서 벗어나 복잡한 문화권별 규칙, 정규화(Normalization), 그리고 대소문자 매핑(Case Mapping)을 고려해야 하는 정교한 프로세스로 진화했습니다.

C#은 문자열 비교를 위한 포괄적인 API를 제공하며, 각 방법은 특정 사용 사례와 성능 특성에 최적화되어 있습니다. **서수 비교(Ordinal Comparison)**는 문자의 유니코드 코드 포인트를 직접 비교하는 가장 빠른 방법으로, 파일 경로나 프로토콜 헤더 같은 기술적 문자열에 적합합니다. 반면 **문화권 인식 비교(Culture-Aware Comparison)**는 해당 언어의 정렬 규칙을 적용하여 사용자 친화적인 결과를 제공하지만, 상대적으로 높은 계산 비용을 수반합니다.

문자열 비교의 성능은 애플리케이션의 전반적인 성능에 중대한 영향을 미칠 수 있습니다. 특히 정렬, 검색, 필터링 같은 작업에서 비교 연산은 알고리즘의 시간 복잡도를 결정하는 핵심 요소입니다. 예를 들어, 10,000개의 문자열을 정렬하는 경우, 평균적으로 약 133,000번의 비교 연산이 발생합니다(O(n log n) 알고리즘 가정). 따라서 각 비교 연산의 효율성은 전체 작업 시간에 직접적으로 곱해집니다.

**== 연산자:**

가장 간단하고 직관적인 비교 방법으로, 대소문자를 구분하는 서수 비교를 수행합니다. C#에서 `==` 연산자는 참조 타입에 대해 기본적으로 참조 동등성(Reference Equality)을 검사하지만, `String` 클래스는 이 연산자를 오버로드하여 값 동등성(Value Equality)을 검사하도록 재정의했습니다.

이는 중요한 설계 결정입니다. 만약 `==` 연산자가 참조를 비교했다면, 동일한 내용의 두 문자열이 서로 다른 메모리 위치에 있을 때 `false`를 반환하게 되어 프로그래머에게 혼란을 줄 수 있습니다. 문자열 인터닝(String Interning)이 항상 보장되지 않기 때문에, 값 비교는 예측 가능하고 일관된 동작을 제공하는 올바른 선택입니다.

`==` 연산자는 컴파일러에 의해 `String.Equals(string, string)` 정적 메서드 호출로 최적화될 수 있으며, 이 메서드는 다음과 같은 최적화를 수행합니다:
1. **참조 동등성 먼저 검사**: 두 문자열이 같은 객체를 참조하면 즉시 `true` 반환
2. **null 검사**: 한쪽이 null이면 다른 쪽도 null인지 확인
3. **길이 검사**: 길이가 다르면 즉시 `false` 반환
4. **내용 비교**: 바이트 단위 또는 워드 단위로 효율적인 메모리 비교 수행

```csharp
string str1 = "Hello";
string str2 = "Hello";
string str3 = "hello";

Console.WriteLine(str1 == str2);  // 출력: True
Console.WriteLine(str1 == str3);  // 출력: False (대소문자 다름)

// null 체크
string str4 = null;
Console.WriteLine(str4 == null);  // 출력: True
```

**Equals() 메서드:**

`Equals()` 메서드는 `==` 연산자보다 더 명시적이고 유연한 비교 인터페이스를 제공합니다. 특히 `StringComparison` 열거형을 매개변수로 받는 오버로드는 비교 의미론(Comparison Semantics)을 세밀하게 제어할 수 있게 해줍니다.

`StringComparison` 열거형의 값들은 서로 다른 비교 전략을 나타냅니다:
- **Ordinal**: 유니코드 코드 포인트의 바이너리 비교. 가장 빠르지만 언어학적 의미를 무시합니다.
- **OrdinalIgnoreCase**: 대소문자를 무시하는 서수 비교. 영어 ASCII 범위에서는 매우 효율적이지만, 비ASCII 문자의 경우 제한적입니다.
- **CurrentCulture**: 현재 스레드의 문화권 설정을 사용한 비교. 사용자 인터페이스에 표시될 문자열에 적합합니다.
- **CurrentCultureIgnoreCase**: 현재 문화권에서 대소문자를 무시하는 비교.
- **InvariantCulture**: 문화권 독립적(Culture-Invariant) 비교. 데이터 저장이나 내부 처리에 적합합니다.
- **InvariantCultureIgnoreCase**: 문화권 독립적이면서 대소문자를 무시하는 비교.

보안과 정확성이 중요한 경우, Microsoft는 `StringComparison.Ordinal` 또는 `StringComparison.OrdinalIgnoreCase` 사용을 권장합니다. 문화권 인식 비교는 예측하기 어려운 결과를 초래할 수 있으며, 특히 터키어의 'i' 문자처럼 특수한 대소문자 규칙을 가진 언어에서 보안 취약점이 발생할 수 있습니다.

```csharp
string str1 = "Apple";
string str2 = "apple";

// 기본 비교 (대소문자 구분)
Console.WriteLine(str1.Equals(str2));  // 출력: False

// 대소문자 무시 비교
bool result = str1.Equals(str2, StringComparison.OrdinalIgnoreCase);
Console.WriteLine(result);  // 출력: True
```

**CompareTo() 메서드:**

`CompareTo()` 메서드는 `IComparable<string>` 인터페이스의 구현체로, 두 문자열의 상대적 순서를 결정합니다. 이 메서드는 정렬 알고리즘의 핵심 구성 요소로, `Array.Sort()`, `List<T>.Sort()`, 그리고 `SortedDictionary<TKey, TValue>`와 같은 정렬된 컬렉션에서 내부적으로 사용됩니다.

반환값의 의미는 다음과 같습니다:
- **음수**: 현재 문자열이 비교 대상보다 사전적으로 앞에 옴
- **0**: 두 문자열이 동등함
- **양수**: 현재 문자열이 비교 대상보다 사전적으로 뒤에 옴

`CompareTo()`의 내부 동작은 문자별 비교를 수행하며, 첫 번째로 다른 문자를 만나면 해당 문자의 유니코드 값 차이를 반환합니다. 모든 문자가 같다면 길이를 비교합니다. 이 알고리즘의 시간 복잡도는 O(min(n, m))으로, 여기서 n과 m은 각 문자열의 길이입니다.

문화권 인식 비교가 필요한 경우, `String.Compare(string, string, StringComparison)` 정적 메서드를 사용해야 합니다. 이는 특히 다국어 애플리케이션에서 사용자에게 자연스러운 정렬 순서를 제공하는 데 중요합니다. 예를 들어, 독일어의 'ß'는 'ss'로 취급되어야 하며, 스페인어의 'ch'는 단일 문자로 정렬되어야 합니다.

```csharp
string str1 = "Apple";
string str2 = "Banana";
string str3 = "Apple";

int result1 = str1.CompareTo(str2);
Console.WriteLine(result1);  // 출력: -1 (str1이 str2보다 앞)

int result2 = str1.CompareTo(str3);
Console.WriteLine(result2);  // 출력: 0 (같음)

int result3 = str2.CompareTo(str1);
Console.WriteLine(result3);  // 출력: 1 (str2가 str1보다 뒤)

// 정렬에 활용
string[] names = { "Charlie", "Alice", "Bob" };
Array.Sort(names);
foreach (string name in names)
{
    Console.WriteLine(name);
}
// 출력:
// Alice
// Bob
// Charlie
```

**Contains() 메서드:**

`Contains()` 메서드는 부분 문자열 검색의 핵심 도구로, 필터링, 검증, 조건부 로직 등에서 널리 사용됩니다. 이 메서드는 내부적으로 효율적인 문자열 검색 알고리즘을 사용하며, .NET의 각 버전에서 지속적으로 최적화되어 왔습니다.

**검색 알고리즘의 진화:**

초기 .NET Framework에서는 단순한 brute-force 검색(모든 위치에서 패턴 비교)을 사용했으나, 현대 .NET에서는 상황에 따라 다른 알고리즘을 선택합니다:
- 짧은 패턴: SIMD(Single Instruction Multiple Data) 명령어를 활용한 벡터화 검색
- 긴 패턴: Boyer-Moore 또는 변형 알고리즘을 사용한 효율적 검색
- 문화권 인식 검색: 복잡한 collation 규칙을 적용한 검색

C# 5.0부터 `StringComparison` 매개변수를 받는 오버로드가 추가되어, 대소문자 무시 검색이나 문화권 인식 검색을 명시적으로 지정할 수 있게 되었습니다. 이는 특히 사용자 입력 검증이나 다국어 애플리케이션에서 중요합니다.

```csharp
string text = "C# 프로그래밍은 재미있습니다.";

bool hasCS = text.Contains("C#");
Console.WriteLine(hasCS);  // 출력: True

bool hasPython = text.Contains("Python");
Console.WriteLine(hasPython);  // 출력: False

// 대소문자 무시 검색 (C# 5.0 이상)
bool result = text.Contains("c#", StringComparison.OrdinalIgnoreCase);
Console.WriteLine(result);  // 출력: True
```

**StartsWith() 와 EndsWith():**

`StartsWith()`와 `EndsWith()` 메서드는 문자열의 시작과 끝을 효율적으로 검사하는 특화된 연산입니다. 이들은 파일 확장자 검증, URL 프로토콜 확인, 프리픽스 기반 라우팅 등에서 핵심적인 역할을 수행합니다.

**최적화된 구현:**

이 메서드들은 전체 문자열을 검색하는 `Contains()`와 달리, 검사할 위치가 명확하므로 매우 효율적으로 구현됩니다. 길이 검사를 먼저 수행하여 명백히 일치하지 않는 경우를 즉시 거부하고, 일치 가능성이 있는 경우에만 실제 문자 비교를 수행합니다. 시간 복잡도는 O(m)으로, 여기서 m은 검색 패턴의 길이입니다.

`StringComparison` 매개변수를 통해 다양한 비교 의미론을 지원하며, 특히 파일 시스템 작업에서는 `StringComparison.OrdinalIgnoreCase`를 사용하는 것이 일반적입니다. Windows 파일 시스템은 대소문자를 구분하지 않기 때문입니다.

```csharp
string fileName = "document.txt";

bool isTxt = fileName.EndsWith(".txt");
Console.WriteLine(isTxt);  // 출력: True

bool isPdf = fileName.EndsWith(".pdf");
Console.WriteLine(isPdf);  // 출력: False

string url = "https://www.example.com";
bool isHttps = url.StartsWith("https");
Console.WriteLine(isHttps);  // 출력: True
```

### 6.1.3 문자열 길이

문자열의 길이는 단순해 보이는 개념이지만, 유니코드의 복잡성으로 인해 실제로는 매우 미묘한 주제입니다. C#의 `Length` 속성은 문자열을 구성하는 **UTF-16 코드 유닛(Code Unit)**의 개수를 반환하며, 이는 항상 우리가 직관적으로 생각하는 "문자 개수"와 일치하지는 않습니다.

**유니코드와 문자 표현의 복잡성:**

유니코드는 전 세계의 모든 문자를 표현하기 위해 설계된 문자 인코딩 표준으로, 현재 약 149,000개 이상의 문자를 정의하고 있습니다. 이러한 방대한 문자 집합을 표현하기 위해 유니코드는 여러 인코딩 방식을 정의하는데, .NET의 `String` 클래스는 UTF-16 인코딩을 사용합니다.

UTF-16에서 대부분의 일반적인 문자(BMP, Basic Multilingual Plane)는 단일 16비트 코드 유닛으로 표현됩니다. 그러나 이모지, 고대 문자, 수학 기호 등 BMP 외부의 문자(Supplementary Characters)는 두 개의 16비트 코드 유닛(서로게이트 쌍, Surrogate Pair)으로 표현됩니다. 예를 들어, 😀 이모지는 `Length`가 2로 계산됩니다.

더 복잡한 경우는 결합 문자(Combining Characters)입니다. 예를 들어, "é"는 기본 문자 'e'와 결합 악센트 '´'의 조합으로 표현될 수 있으며, 이 경우 `Length`는 2가 됩니다. 하지만 동일한 문자는 사전 결합 형태(Precomposed Form) 'é'로도 표현될 수 있으며, 이 경우 `Length`는 1입니다.

**성능 특성:**

`Length` 속성은 O(1) 시간 복잡도를 가집니다. 문자열 객체가 생성될 때 길이 정보가 계산되어 내부 필드에 저장되므로, 속성 접근은 단순한 필드 읽기로 구현됩니다. 이는 문자열 전체를 순회하여 길이를 계산하는 C의 `strlen()` 함수와 대조됩니다.

**Length 속성:**

```csharp
string text = "Hello, World!";
int length = text.Length;

Console.WriteLine($"문자열 길이: {length}");  // 출력: 문자열 길이: 13

// 빈 문자열
string empty = "";
Console.WriteLine(empty.Length);  // 출력: 0

// 한글도 한 글자씩 계산
string korean = "안녕하세요";
Console.WriteLine(korean.Length);  // 출력: 5
```

**실용 예제: 비밀번호 길이 검증**

```csharp
Console.Write("비밀번호를 입력하세요 (8자 이상): ");
string password = Console.ReadLine();

if (password.Length < 8)
{
    Console.WriteLine("비밀번호는 최소 8자 이상이어야 합니다.");
}
else
{
    Console.WriteLine("유효한 비밀번호입니다.");
}
```

**문자열이 비어있는지 확인:**

```csharp
string str1 = "";
string str2 = "   ";
string str3 = null;

// 빈 문자열 확인
Console.WriteLine(str1.Length == 0);  // 출력: True
Console.WriteLine(String.IsNullOrEmpty(str1));  // 출력: True

// 공백만 있는 문자열
Console.WriteLine(str2.Length == 0);  // 출력: False
Console.WriteLine(String.IsNullOrWhiteSpace(str2));  // 출력: True

// null 문자열 (Length 호출 시 오류 발생)
// Console.WriteLine(str3.Length);  // ❌ NullReferenceException
Console.WriteLine(String.IsNullOrEmpty(str3));  // 출력: True
```

**문자열 자르기와 길이:**

```csharp
string longText = "이것은 매우 긴 텍스트입니다.";

// 특정 길이까지만 표시
int maxLength = 10;
if (longText.Length > maxLength)
{
    string shortened = longText.Substring(0, maxLength) + "...";
    Console.WriteLine(shortened);  // 출력: 이것은 매우 긴 텍...
}
```

---

## 6.2 문자열 메서드

C#의 `String` 클래스는 문자열 처리를 위한 풍부한 메서드 라이브러리를 제공합니다. 이들 메서드는 수십 년간의 문자열 처리 경험과 모범 사례를 집약한 것으로, 직접 구현하기 복잡하거나 오류가 발생하기 쉬운 작업들을 안전하고 효율적으로 수행할 수 있게 해줍니다.

문자열 메서드의 설계 철학은 **불변성(Immutability)** 원칙을 철저히 따릅니다. 모든 메서드는 원본 문자열을 변경하지 않고 새로운 문자열을 반환하며, 이는 함수형 프로그래밍의 순수 함수(Pure Function) 개념과 일치합니다. 이러한 설계는 예측 가능한 동작을 보장하고, 멀티스레드 환경에서의 안전성을 자동으로 제공합니다.

성능 관점에서 보면, 대부분의 문자열 메서드는 최적화된 네이티브 코드로 구현되어 있으며, SIMD(Single Instruction Multiple Data) 명령어를 활용하여 벡터화된 연산을 수행합니다. 예를 들어, 긴 문자열에서 특정 문자를 검색하는 `IndexOf()` 메서드는 한 번에 여러 바이트를 비교하여 선형 검색의 상수 계수를 크게 줄입니다.

### 6.2.1 Substring()

`Substring()` 메서드는 문자열의 일부분을 추출하는 핵심 연산으로, 파싱(Parsing), 토큰화(Tokenization), 그리고 데이터 추출 작업의 기반이 됩니다. 이 메서드는 두 가지 오버로드를 제공합니다: 시작 인덱스만 지정하는 버전과 시작 인덱스와 길이를 모두 지정하는 버전입니다.

**내부 구현과 메모리 할당:**

`Substring()`은 불변성 원칙에 따라 새로운 문자열 객체를 생성합니다. 과거 .NET Framework 버전에서는 원본 문자열의 내부 배열을 공유하고 오프셋만 저장하는 최적화가 있었으나, 이는 원본 문자열이 가비지 컬렉션되지 못하는 메모리 누수 문제를 야기했습니다. 현대 .NET에서는 항상 새로운 메모리를 할당하고 필요한 문자들을 복사하는 방식을 사용합니다.

이 접근 방식은 메모리 사용 패턴을 예측 가능하게 만들지만, 큰 문자열에서 작은 부분을 반복적으로 추출하는 경우 성능 오버헤드가 발생할 수 있습니다. 이러한 시나리오에서는 `ReadOnlySpan<char>`를 사용하는 것이 더 효율적일 수 있습니다. Span은 메모리 할당 없이 원본 문자열의 슬라이스를 표현할 수 있는 스택 기반 타입입니다.

**기본 사용법:**

```csharp
string text = "Hello, World!";

// 인덱스 7부터 끝까지
string sub1 = text.Substring(7);
Console.WriteLine(sub1);  // 출력: World!

// 인덱스 0부터 5글자
string sub2 = text.Substring(0, 5);
Console.WriteLine(sub2);  // 출력: Hello

// 인덱스 7부터 5글자
string sub3 = text.Substring(7, 5);
Console.WriteLine(sub3);  // 출력: World
```

**실용 예제: 날짜 문자열 파싱**

```csharp
string dateStr = "2024-11-14";

string year = dateStr.Substring(0, 4);
string month = dateStr.Substring(5, 2);
string day = dateStr.Substring(8, 2);

Console.WriteLine($"연도: {year}");  // 출력: 연도: 2024
Console.WriteLine($"월: {month}");    // 출력: 월: 11
Console.WriteLine($"일: {day}");      // 출력: 일: 14
```

**실용 예제: 파일 확장자 추출**

```csharp
string fileName = "document.pdf";
int dotIndex = fileName.LastIndexOf('.');

if (dotIndex >= 0)
{
    string extension = fileName.Substring(dotIndex);
    Console.WriteLine($"확장자: {extension}");  // 출력: 확장자: .pdf
}
```

**주의사항:**

```csharp
string text = "Hello";

// 범위를 벗어나면 예외 발생
try
{
    string invalid = text.Substring(10);  // ❌ ArgumentOutOfRangeException
}
catch (ArgumentOutOfRangeException)
{
    Console.WriteLine("인덱스가 범위를 벗어났습니다.");
}
```

### 6.2.2 Replace()

`Replace()` 메서드는 문자열 내의 모든 특정 문자 또는 부분 문자열을 다른 것으로 치환하는 강력한 도구입니다. 이 메서드는 텍스트 정제(Text Sanitization), 템플릿 처리, 데이터 변환 등 다양한 시나리오에서 핵심적인 역할을 수행합니다.

**알고리즘 및 성능 특성:**

`Replace()`의 내부 구현은 효율성을 위해 최적화되어 있습니다. 먼저 원본 문자열을 순회하면서 모든 일치 항목의 위치를 찾고, 필요한 최종 문자열의 크기를 계산합니다. 그 다음 정확한 크기의 새 문자열을 할당하고, 치환되지 않는 부분과 치환된 부분을 순차적으로 복사합니다. 이는 단일 메모리 할당과 최소한의 복사 연산으로 최적의 성능을 달성합니다.

시간 복잡도는 O(n * m)입니다. 여기서 n은 원본 문자열의 길이이고, m은 검색 패턴의 길이입니다. 일치 항목의 개수를 k라고 하면, 공간 복잡도는 O(n + k * (replacement.Length - oldValue.Length))입니다.

**단일 패스 vs 다중 패스:**

여러 개의 치환을 수행해야 하는 경우, 연속적인 `Replace()` 호출은 비효율적일 수 있습니다. 각 호출마다 새로운 문자열이 생성되고 전체 문자열이 순회되기 때문입니다. 이런 경우 정규 표현식(Regular Expression)의 `Regex.Replace()`나 `StringBuilder`를 활용한 수동 치환이 더 효율적일 수 있습니다.

**기본 사용법:**

```csharp
string text = "Hello, World!";

// 단일 문자 교체
string result1 = text.Replace('o', 'O');
Console.WriteLine(result1);  // 출력: HellO, WOrld!

// 문자열 교체
string result2 = text.Replace("World", "C#");
Console.WriteLine(result2);  // 출력: Hello, C#!

// 여러 번 치환
string text2 = "apple, apple, apple";
string result3 = text2.Replace("apple", "orange");
Console.WriteLine(result3);  // 출력: orange, orange, orange
```

**실용 예제: 공백 제거**

```csharp
string text = "Hello World Programming";

// 모든 공백 제거
string noSpaces = text.Replace(" ", "");
Console.WriteLine(noSpaces);  // 출력: HelloWorldProgramming

// 공백을 밑줄로 변경
string withUnderscore = text.Replace(" ", "_");
Console.WriteLine(withUnderscore);  // 출력: Hello_World_Programming
```

**실용 예제: 민감 정보 마스킹**

```csharp
string creditCard = "1234-5678-9012-3456";

// 중간 숫자를 별표로 치환
string masked = creditCard.Substring(0, 5) + "****-****" + creditCard.Substring(14);
Console.WriteLine(masked);  // 출력: 1234-****-****-3456
```

**연쇄 치환:**

```csharp
string text = "Hello, World!";

// 여러 치환을 연속으로 수행
string result = text.Replace("Hello", "Hi")
                   .Replace("World", "C#")
                   .Replace("!", ".");

Console.WriteLine(result);  // 출력: Hi, C#.
```

### 6.2.3 Split()

`Split()` 메서드는 문자열을 구분자(Delimiter)를 기준으로 분할하여 문자열 배열로 변환하는 핵심적인 파싱 도구입니다. CSV 파일 처리, 로그 파싱, 프로토콜 메시지 디코딩, 명령줄 인자 처리 등 거의 모든 데이터 처리 작업에서 필수적으로 사용됩니다.

**파싱 알고리즘과 내부 동작:**

`Split()`의 내부 구현은 두 단계로 구성됩니다. 첫 번째 단계에서는 원본 문자열을 순회하면서 모든 구분자의 위치를 찾아 인덱스 리스트를 구축합니다. 두 번째 단계에서는 이 인덱스들을 사용하여 각 부분 문자열을 추출하고 배열에 저장합니다. 이 접근 방식은 배열의 정확한 크기를 미리 알 수 있어 메모리 재할당을 피할 수 있다는 장점이 있습니다.

현대 .NET에서는 `Split()`이 `string[]` 대신 `ReadOnlySpan<string>`을 반환하는 오버로드도 제공하여, 임시 배열 할당을 줄이고 성능을 향상시킬 수 있습니다. 또한 `StringSplitOptions` 열거형을 통해 빈 항목 제거, 공백 trim 등의 추가 처리를 지정할 수 있습니다.

**성능 고려사항:**

대용량 텍스트를 처리할 때 `Split()`의 성능은 중요한 고려사항입니다. 예를 들어, 10MB 크기의 CSV 파일을 처리하는 경우, `Split()`은 수백만 개의 문자열 객체를 생성할 수 있으며, 이는 상당한 메모리 압력과 가비지 컬렉션 오버헤드를 초래합니다. 이러한 시나리오에서는 스트리밍 방식의 파서(`StreamReader`와 `ReadLine()` 조합)나 `Span<T>` 기반의 분할 방법이 더 적합할 수 있습니다.

**기본 사용법:**

```csharp
string text = "apple,banana,orange";

string[] fruits = text.Split(',');

foreach (string fruit in fruits)
{
    Console.WriteLine(fruit);
}
// 출력:
// apple
// banana
// orange

Console.WriteLine($"과일 개수: {fruits.Length}");  // 출력: 과일 개수: 3
```

**여러 구분자 사용:**

```csharp
string text = "apple,banana;orange|grape";

// 여러 구분자 지정
char[] separators = { ',', ';', '|' };
string[] fruits = text.Split(separators);

foreach (string fruit in fruits)
{
    Console.WriteLine(fruit);
}
// 출력:
// apple
// banana
// orange
// grape
```

**공백으로 분리:**

```csharp
string sentence = "C# is a powerful language";

string[] words = sentence.Split(' ');

foreach (string word in words)
{
    Console.WriteLine(word);
}
// 출력:
// C#
// is
// a
// powerful
// language
```

**빈 항목 제거:**

```csharp
string text = "apple,,banana,,,orange";

// 기본 Split - 빈 항목 포함
string[] fruits1 = text.Split(',');
Console.WriteLine($"항목 개수: {fruits1.Length}");  // 출력: 항목 개수: 6

// 빈 항목 제거
string[] fruits2 = text.Split(',', StringSplitOptions.RemoveEmptyEntries);
Console.WriteLine($"항목 개수: {fruits2.Length}");  // 출력: 항목 개수: 3

foreach (string fruit in fruits2)
{
    Console.WriteLine(fruit);
}
// 출력:
// apple
// banana
// orange
```

**실용 예제: CSV 데이터 파싱**

```csharp
string csvLine = "홍길동,30,서울";

string[] data = csvLine.Split(',');

string name = data[0];
int age = int.Parse(data[1]);
string city = data[2];

Console.WriteLine($"이름: {name}");  // 출력: 이름: 홍길동
Console.WriteLine($"나이: {age}");   // 출력: 나이: 30
Console.WriteLine($"도시: {city}");  // 출력: 도시: 서울
```

**실용 예제: 경로 분리**

```csharp
string path = "C:/Users/Username/Documents/file.txt";

string[] parts = path.Split('/');

Console.WriteLine("경로 구성 요소:");
foreach (string part in parts)
{
    Console.WriteLine($"  - {part}");
}
// 출력:
// 경로 구성 요소:
//   - C:
//   - Users
//   - Username
//   - Documents
//   - file.txt
```

### 6.2.4 Trim(), ToUpper(), ToLower()

이들 메서드는 문자열 정규화(Normalization)와 정제(Sanitization)의 기본 도구입니다. 사용자 입력 처리, 데이터 비교, 텍스트 포맷팅 등에서 필수적으로 사용되며, 각각은 유니코드의 복잡성을 고려한 정교한 구현을 포함합니다.

**Trim() 계열 - 공백 처리의 미묘함:**

`Trim()`, `TrimStart()`, `TrimEnd()` 메서드는 문자열의 앞뒤에서 특정 문자(기본적으로 공백 문자)를 제거합니다. "공백"의 정의는 단순한 스페이스(U+0020)를 넘어서, 유니코드가 정의하는 25개 이상의 공백 문자를 포함합니다: 탭, 줄바꿈, 캐리지 리턴, non-breaking space, zero-width space 등이 모두 공백으로 간주됩니다.

이 메서드들은 양방향 검색을 수행하여 제거할 문자의 범위를 결정한 후, 해당 범위를 제외한 새 문자열을 생성합니다. 최악의 경우(모든 문자가 공백) O(n) 시간 복잡도를 가지지만, 일반적인 경우에는 문자열의 시작과 끝 부분만 검사하므로 매우 빠릅니다.

특정 문자 집합을 제거하는 오버로드(`Trim(params char[] trimChars)`)도 제공되며, 이는 사용자 정의 구분자나 특수 문자를 제거할 때 유용합니다.

**ToUpper() / ToLower() - 대소문자 변환의 복잡성:**

대소문자 변환은 겉보기에 단순해 보이지만, 실제로는 매우 복잡한 언어학적 규칙을 포함합니다. 유니코드는 각 언어의 대소문자 매핑 규칙을 정의하는데, 이는 1:1 매핑이 아닐 수 있습니다. 예를 들어:
- 독일어 소문자 'ß'는 대문자로 'SS'가 됩니다 (1→2)
- 그리스어 소문자 'ς'(단어 끝의 sigma)와 'σ'(중간의 sigma)는 모두 대문자 'Σ'로 변환됩니다 (2→1)
- 터키어와 아제르바이잔어에서 'i'의 대문자는 'İ'(dotted I)이고, 'I'의 소문자는 'ı'(dotless i)입니다

이러한 복잡성 때문에 `ToUpper()`와 `ToLower()`는 문화권(CultureInfo) 매개변수를 받는 오버로드를 제공합니다. 기본 매개변수 없는 버전은 현재 스레드의 문화권을 사용하며, 이는 예측하기 어려운 동작을 초래할 수 있습니다.

**보안 및 모범 사례:**

보안이 중요한 컨텍스트(파일명, URL, 프로토콜 헤더 등)에서는 `ToUpperInvariant()`나 `ToLowerInvariant()`를 사용하는 것이 권장됩니다. 이들은 문화권 독립적인 변환을 수행하여 일관된 동작을 보장합니다. 터키어 'i' 문제는 실제로 보안 취약점으로 이어진 사례가 있습니다: "file"을 대문자로 변환한 후 비교하는 코드가 터키어 로케일에서 "FİLE"를 생성하여 검증을 우회하는 공격이 가능했습니다.

**Trim() - 앞뒤 공백 제거:**

```csharp
string text = "   Hello, World!   ";

string trimmed = text.Trim();
Console.WriteLine($"원본: '{text}'");      // 출력: 원본: '   Hello, World!   '
Console.WriteLine($"Trim: '{trimmed}'");  // 출력: Trim: 'Hello, World!'

// 앞쪽 공백만 제거
string leftTrimmed = text.TrimStart();
Console.WriteLine($"TrimStart: '{leftTrimmed}'");  // 출력: TrimStart: 'Hello, World!   '

// 뒤쪽 공백만 제거
string rightTrimmed = text.TrimEnd();
Console.WriteLine($"TrimEnd: '{rightTrimmed}'");   // 출력: TrimEnd: '   Hello, World!'
```

**특정 문자 제거:**

```csharp
string text = "***Hello***";

// 특정 문자 제거
string result = text.Trim('*');
Console.WriteLine(result);  // 출력: Hello

// 여러 문자 제거
string text2 = "##Hello##World##";
char[] charsToTrim = { '#' };
string result2 = text2.Trim(charsToTrim);
Console.WriteLine(result2);  // 출력: Hello##World
```

**ToUpper() - 대문자 변환:**

```csharp
string text = "Hello, World!";

string upper = text.ToUpper();
Console.WriteLine(upper);  // 출력: HELLO, WORLD!

// 실용 예제: 대소문자 무시 비교
string input = "yes";
if (input.ToUpper() == "YES")
{
    Console.WriteLine("긍정 응답입니다.");
}
```

**ToLower() - 소문자 변환:**

```csharp
string text = "Hello, World!";

string lower = text.ToLower();
Console.WriteLine(lower);  // 출력: hello, world!

// 실용 예제: 이메일 정규화
string email = "User@Example.COM";
string normalizedEmail = email.ToLower();
Console.WriteLine(normalizedEmail);  // 출력: user@example.com
```

**실용 예제: 사용자 입력 처리**

```csharp
Console.Write("계속하시겠습니까? (yes/no): ");
string input = Console.ReadLine();

// 공백 제거 및 소문자 변환
string normalized = input.Trim().ToLower();

if (normalized == "yes" || normalized == "y")
{
    Console.WriteLine("계속 진행합니다.");
}
else if (normalized == "no" || normalized == "n")
{
    Console.WriteLine("종료합니다.");
}
else
{
    Console.WriteLine("잘못된 입력입니다.");
}
```

**문화권 고려 변환:**

```csharp
using System.Globalization;

string text = "türkiye";  // 터키어

// 현재 문화권 기준
string upper1 = text.ToUpper();
Console.WriteLine(upper1);

// 특정 문화권 기준
string upper2 = text.ToUpper(new CultureInfo("tr-TR"));
Console.WriteLine(upper2);

// 불변 문화권 기준 (권장)
string upper3 = text.ToUpperInvariant();
Console.WriteLine(upper3);
```

---

## 6.3 StringBuilder

`StringBuilder`는 .NET 프레임워크가 제공하는 가변(Mutable) 문자열 버퍼로, 빈번한 문자열 수정 작업의 성능 문제를 근본적으로 해결하기 위해 설계되었습니다. 이 클래스는 `System.Text` 네임스페이스에 정의되어 있으며, 문자열의 불변성으로 인한 메모리 오버헤드와 가비지 컬렉션 압력을 극적으로 줄일 수 있는 핵심 도구입니다.

### 6.3.1 StringBuilder의 필요성

**문자열 불변성의 성능 함의:**

C#의 문자열이 불변이라는 사실은 안전성과 예측 가능성이라는 중요한 이점을 제공하지만, 빈번한 수정이 필요한 경우 심각한 성능 문제를 야기합니다. 이를 이해하기 위해 반복문에서 문자열을 연결하는 간단한 예제를 분석해 보겠습니다.

```csharp
string result = "";
for (int i = 0; i < 1000; i++)
{
    result += i.ToString();
}
```

이 코드의 문제점은 각 반복마다 다음과 같은 과정이 일어난다는 것입니다:
1. 새로운 문자열 객체를 위한 메모리 할당 (점점 커지는 크기)
2. 기존 문자열의 모든 내용을 새 메모리로 복사
3. 새로운 부분을 추가로 복사
4. 기존 문자열 객체는 가비지가 됨

이는 **이차 시간 복잡도(O(n²))**를 가지는 알고리즘입니다. n번 반복할 때, 총 복사되는 문자 수는 대략 n(n+1)/2가 됩니다. 1000번 반복하면 약 500,000번의 문자 복사가 발생하며, 1000개의 임시 문자열 객체가 생성되어 가비지 컬렉션의 부담이 됩니다.

**StringBuilder의 해결 방안:**

`StringBuilder`는 내부적으로 가변 문자 배열을 유지하며, 용량(Capacity)이 부족하면 더 큰 배열을 할당하고 내용을 복사하는 전략을 사용합니다. 기본적으로 용량이 부족하면 현재 용량의 2배로 확장하는데, 이는 분할 상환 분석(Amortized Analysis)에서 O(1)의 삽입 비용을 보장하는 동적 배열의 표준 기법입니다.

결과적으로 `StringBuilder`를 사용한 문자열 구축은 **선형 시간 복잡도(O(n))**를 가지며, 메모리 할당 횟수도 O(log n)으로 크게 줄어듭니다. 이는 대규모 문자열 조작에서 수백 배에서 수천 배의 성능 향상을 의미할 수 있습니다.

**메모리 사용 패턴:**

`StringBuilder`는 힙 메모리에 단일 연속 문자 배열을 유지합니다. 초기 용량을 적절히 설정하면 재할당을 최소화할 수 있습니다. 예를 들어, 최종 결과가 대략 10,000 문자가 될 것을 알고 있다면, `new StringBuilder(10000)`으로 초기화하여 재할당을 완전히 피할 수 있습니다.

```csharp
// 비효율적인 방식 - 반복적인 문자열 연결
string result = "";
for (int i = 1; i <= 5; i++)
{
    result += i + " ";  // 매번 새로운 문자열 객체 생성
}
Console.WriteLine(result);  // 출력: 1 2 3 4 5

// 위 코드는 다음과 같이 동작:
// 1회: "" + "1 " → 새 문자열 "1 "
// 2회: "1 " + "2 " → 새 문자열 "1 2 "
// 3회: "1 2 " + "3 " → 새 문자열 "1 2 3 "
// ...
// 총 5개의 임시 문자열 객체 생성
```

**StringBuilder의 장점:**

`StringBuilder`는 내부 버퍼를 사용하여 문자열을 조작하므로, 매번 새로운 객체를 생성하지 않습니다.

```csharp
using System.Text;

// 효율적인 방식 - StringBuilder 사용
StringBuilder sb = new StringBuilder();
for (int i = 1; i <= 5; i++)
{
    sb.Append(i);
    sb.Append(" ");
}
string result = sb.ToString();
Console.WriteLine(result);  // 출력: 1 2 3 4 5
```

**성능 비교 예제:**

```csharp
using System.Text;
using System.Diagnostics;

int iterations = 10000;

// String 연결 방식
Stopwatch sw1 = Stopwatch.StartNew();
string str = "";
for (int i = 0; i < iterations; i++)
{
    str += "a";
}
sw1.Stop();
Console.WriteLine($"String 연결: {sw1.ElapsedMilliseconds}ms");

// StringBuilder 방식
Stopwatch sw2 = Stopwatch.StartNew();
StringBuilder sb = new StringBuilder();
for (int i = 0; i < iterations; i++)
{
    sb.Append("a");
}
string result = sb.ToString();
sw2.Stop();
Console.WriteLine($"StringBuilder: {sw2.ElapsedMilliseconds}ms");

// StringBuilder가 훨씬 빠름 (특히 iterations가 클수록)
```

**언제 StringBuilder를 사용할까:**

- 반복문 내에서 문자열을 계속 수정하는 경우
- 대량의 문자열 조합이 필요한 경우
- 동적으로 생성되는 긴 텍스트를 만들 때

**언제 일반 문자열을 사용할까:**

- 한두 번의 간단한 연결만 필요한 경우
- 문자열 보간(`$""`)으로 충분한 경우
- 문자열을 읽기만 하는 경우

### 6.3.2 효율적인 문자열 조작

`StringBuilder`는 문자열을 효율적으로 조작할 수 있는 포괄적인 메서드 집합을 제공합니다. 이들 메서드는 모두 객체 자체를 수정하고 자기 자신(`this`)을 반환하는 **유창한 인터페이스(Fluent Interface)** 패턴을 따르므로, 메서드 체이닝(Method Chaining)을 통해 간결하고 가독성 높은 코드를 작성할 수 있습니다.

**핵심 메서드와 내부 메커니즘:**

- **Append()**: 문자열, 숫자, 객체 등을 버퍼 끝에 추가합니다. 분할 상환 O(1) 시간 복잡도를 가지며, 다양한 타입에 대한 오버로드가 존재하여 박싱(Boxing) 없이 효율적인 변환이 가능합니다.

- **AppendLine()**: 문자열을 추가한 후 환경에 맞는 줄바꿈 문자(`Environment.NewLine`)를 자동으로 추가합니다. 플랫폼 독립적인 텍스트 생성에 유용합니다.

- **Insert()**: 특정 위치에 문자열을 삽입합니다. 삽입 위치 이후의 모든 문자를 이동해야 하므로 O(n) 시간 복잡도를 가집니다. 빈번한 삽입이 필요한 경우 성능 병목이 될 수 있습니다.

- **Remove()**: 특정 범위의 문자를 제거합니다. Insert()와 마찬가지로 O(n) 복잡도를 가지며, 제거 위치 이후의 문자들을 앞으로 이동시킵니다.

- **Replace()**: 버퍼 내의 모든 일치 항목을 치환합니다. String.Replace()와 달리 새 객체를 생성하지 않고 제자리에서(in-place) 수정하므로 훨씬 효율적입니다. 치환 결과가 원본보다 길어지는 경우 내부 버퍼가 확장될 수 있습니다.

- **Clear()**: 버퍼의 모든 내용을 제거하지만 할당된 메모리는 유지합니다. 이는 StringBuilder를 재사용할 때 유용하며, 메모리 재할당 없이 초기화할 수 있습니다.

**용량 관리 전략:**

`StringBuilder`의 `Capacity` 속성은 현재 할당된 내부 버퍼의 크기를 나타내며, `Length`는 실제 저장된 문자의 개수를 나타냅니다. 용량은 항상 길이보다 크거나 같으며, 길이가 용량을 초과하면 자동으로 확장됩니다.

최적의 성능을 위해서는 예상되는 최종 크기를 기반으로 초기 용량을 설정하는 것이 좋습니다. 이는 재할당 횟수를 최소화하여 메모리 복사 오버헤드를 줄입니다. 또한 `EnsureCapacity()` 메서드를 사용하여 명시적으로 용량을 확보할 수 있습니다.

**기본 사용법:**

```csharp
using System.Text;

// StringBuilder 생성
StringBuilder sb = new StringBuilder();

// 문자열 추가
sb.Append("Hello");
sb.Append(" ");
sb.Append("World");

// 문자열로 변환
string result = sb.ToString();
Console.WriteLine(result);  // 출력: Hello World
```

**초기 용량 지정:**

예상되는 문자열 길이를 미리 지정하면 성능이 더 향상됩니다.

```csharp
// 초기 용량을 100으로 설정
StringBuilder sb = new StringBuilder(100);

// 초기 문자열과 용량 지정
StringBuilder sb2 = new StringBuilder("Initial text", 200);
```

**주요 메서드:**

```csharp
using System.Text;

StringBuilder sb = new StringBuilder();

// Append: 끝에 추가
sb.Append("Hello");
sb.Append(" ");
sb.Append("World");
Console.WriteLine(sb.ToString());  // 출력: Hello World

// AppendLine: 줄바꿈과 함께 추가
sb.Clear();  // 내용 비우기
sb.AppendLine("첫 번째 줄");
sb.AppendLine("두 번째 줄");
sb.AppendLine("세 번째 줄");
Console.WriteLine(sb.ToString());
// 출력:
// 첫 번째 줄
// 두 번째 줄
// 세 번째 줄

// Insert: 특정 위치에 삽입
sb.Clear();
sb.Append("Hello World");
sb.Insert(5, ",");
Console.WriteLine(sb.ToString());  // 출력: Hello, World

// Remove: 특정 범위 제거
sb.Clear();
sb.Append("Hello World");
sb.Remove(5, 6);  // 인덱스 5부터 6글자 제거
Console.WriteLine(sb.ToString());  // 출력: Hello

// Replace: 문자열 치환
sb.Clear();
sb.Append("Hello World");
sb.Replace("World", "C#");
Console.WriteLine(sb.ToString());  // 출력: Hello C#
```

**메서드 체이닝:**

대부분의 `StringBuilder` 메서드는 자기 자신을 반환하므로, 메서드를 연쇄적으로 호출할 수 있습니다.

```csharp
using System.Text;

string result = new StringBuilder()
    .Append("Hello")
    .Append(" ")
    .Append("World")
    .Append("!")
    .ToString();

Console.WriteLine(result);  // 출력: Hello World!

// 복잡한 예제
StringBuilder sb = new StringBuilder()
    .AppendLine("=== 보고서 ===")
    .AppendLine()
    .Append("제목: ")
    .AppendLine("월간 보고서")
    .Append("날짜: ")
    .AppendLine(DateTime.Now.ToString("yyyy-MM-dd"))
    .AppendLine()
    .AppendLine("내용: ...")
    .ToString();

Console.WriteLine(sb);
```

**실용 예제: HTML 생성**

```csharp
using System.Text;

StringBuilder html = new StringBuilder();

html.AppendLine("<html>");
html.AppendLine("  <head>");
html.AppendLine("    <title>My Page</title>");
html.AppendLine("  </head>");
html.AppendLine("  <body>");
html.AppendLine("    <h1>Welcome</h1>");
html.AppendLine("    <p>This is a paragraph.</p>");
html.AppendLine("  </body>");
html.AppendLine("</html>");

string htmlContent = html.ToString();
Console.WriteLine(htmlContent);
```

**실용 예제: 로그 메시지 생성**

```csharp
using System.Text;

void LogMessage(string level, string message)
{
    StringBuilder log = new StringBuilder();
    
    log.Append("[");
    log.Append(DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss"));
    log.Append("] ");
    log.Append($"[{level}] ");
    log.Append(message);
    
    Console.WriteLine(log.ToString());
}

LogMessage("INFO", "애플리케이션 시작");
LogMessage("WARNING", "메모리 사용량이 높습니다");
LogMessage("ERROR", "데이터베이스 연결 실패");

// 출력:
// [2024-11-14 10:30:45] [INFO] 애플리케이션 시작
// [2024-11-14 10:30:46] [WARNING] 메모리 사용량이 높습니다
// [2024-11-14 10:30:47] [ERROR] 데이터베이스 연결 실패
```

**실용 예제: CSV 생성**

```csharp
using System.Text;

// 학생 데이터를 CSV 형식으로 변환
string[][] students = new string[][]
{
    new string[] { "홍길동", "25", "서울" },
    new string[] { "김철수", "23", "부산" },
    new string[] { "이영희", "27", "대구" }
};

StringBuilder csv = new StringBuilder();

// 헤더
csv.AppendLine("이름,나이,도시");

// 데이터 행
foreach (string[] student in students)
{
    csv.AppendLine(String.Join(",", student));
}

string csvContent = csv.ToString();
Console.WriteLine(csvContent);
// 출력:
// 이름,나이,도시
// 홍길동,25,서울
// 김철수,23,부산
// 이영희,27,대구
```

**용량 관리:**

```csharp
using System.Text;

StringBuilder sb = new StringBuilder();

Console.WriteLine($"초기 용량: {sb.Capacity}");  // 출력: 초기 용량: 16
Console.WriteLine($"현재 길이: {sb.Length}");    // 출력: 현재 길이: 0

sb.Append("Hello World");
Console.WriteLine($"추가 후 용량: {sb.Capacity}");  // 출력: 추가 후 용량: 16
Console.WriteLine($"추가 후 길이: {sb.Length}");    // 출력: 추가 후 길이: 11

// 용량이 부족하면 자동으로 증가
for (int i = 0; i < 100; i++)
{
    sb.Append("x");
}
Console.WriteLine($"최종 용량: {sb.Capacity}");  // 출력: 최종 용량: 128 (자동 증가)
Console.WriteLine($"최종 길이: {sb.Length}");    // 출력: 최종 길이: 111
```

---

## 마무리

이 장에서는 C# 문자열 처리의 이론적 토대부터 실용적 기법까지 포괄적으로 탐구했습니다. 문자열 처리는 단순한 텍스트 조작을 넘어서, 메모리 관리, 알고리즘 효율성, 유니코드 표준, 문화권 인식, 그리고 보안 고려사항을 모두 아우르는 복잡한 영역임을 확인했습니다.

**핵심 정리:**

✅ **문자열의 불변성**: C# 문자열은 불변 참조 타입으로, 스레드 안전성과 예측 가능성을 제공하지만 빈번한 수정 시 성능 오버헤드가 발생합니다. 이는 O(n²) 복잡도로 이어질 수 있으며, `StringBuilder`를 통해 O(n)으로 개선할 수 있습니다.

✅ **문자열 연결의 다양한 방법론**: `+` 연산자의 직관성, 문자열 보간의 가독성, `String.Concat()`의 효율성, `String.Join()`의 구분자 처리 능력을 이해하고, 각 상황에 최적의 도구를 선택할 수 있어야 합니다.

✅ **문자열 비교의 다층성**: 서수 비교(Ordinal)와 문화권 인식 비교(Culture-Aware)의 차이, `StringComparison` 열거형의 활용, 그리고 보안 관점에서의 올바른 비교 방법 선택이 중요합니다.

✅ **유니코드의 복잡성**: UTF-16 인코딩, 서로게이트 쌍, 결합 문자, 정규화 등 현대 문자열 처리가 직면한 기술적 도전을 이해하고, `Length` 속성이 항상 "문자 개수"를 의미하지 않음을 인식해야 합니다.

✅ **문자열 메서드 생태계**: `Substring()`, `Replace()`, `Split()`, `Trim()` 등 핵심 메서드의 시간/공간 복잡도, 내부 알고리즘, 그리고 최적 사용 패턴을 습득했습니다.

✅ **StringBuilder의 설계 원리**: 가변 버퍼, 용량 관리, 분할 상환 분석, 유창한 인터페이스 등 `StringBuilder`의 설계 철학과 이것이 어떻게 빈번한 문자열 수정의 성능 문제를 해결하는지 이해했습니다.

**실전 활용 가이드라인:**

1. **소수의 연결**: 2-3개 문자열의 단순 연결에는 문자열 보간(`$""`)이나 `+` 연산자가 충분히 효율적이며 가독성이 높습니다.

2. **반복문 내 연결**: 반복 횟수가 미리 알려지지 않았거나 많은 경우, `StringBuilder`를 사용하고 가능하면 초기 용량을 설정하십시오.

3. **비교 연산 선택**: 
   - 기술적 문자열(파일명, URL): `StringComparison.Ordinal` 또는 `OrdinalIgnoreCase`
   - 사용자 표시 문자열: `StringComparison.CurrentCulture`
   - 데이터 저장/내부 처리: `StringComparison.InvariantCulture`

4. **입력 검증**: `Trim()`으로 앞뒤 공백 제거, `IsNullOrWhiteSpace()`로 유효성 확인, 그리고 적절한 `StringComparison`으로 비교하십시오.

5. **대용량 텍스트 처리**: `Split()`이 많은 메모리를 사용할 수 있으므로, 대용량 파일은 `StreamReader`와 `ReadLine()`을 사용한 스트리밍 처리를 고려하십시오.

6. **성능 최적화가 필요한 경우**: `ReadOnlySpan<char>`, `Memory<T>`, 그리고 `stackalloc`을 활용한 제로 할당(Zero-Allocation) 기법을 연구하십시오.

**소프트웨어 공학적 고려사항:**

효율적인 문자열 처리는 단순히 올바른 메서드를 호출하는 것을 넘어서, 알고리즘 선택, 메모리 관리, 국제화(i18n), 보안, 그리고 유지보수성을 모두 고려하는 종합적인 의사결정입니다. 성능과 가독성 사이의 균형, 문화권 인식의 필요성과 오버헤드 사이의 트레이드오프, 그리고 불변성의 안전성과 가변성의 효율성 사이의 선택은 모두 컨텍스트에 따라 달라집니다.

특히 보안이 중요한 애플리케이션에서는 문자열 비교의 미묘한 차이가 취약점으로 이어질 수 있음을 항상 유념해야 합니다. 터키어 'i' 문제, SQL 인젝션, XSS 공격 등은 모두 부적절한 문자열 처리에서 비롯될 수 있습니다.

**다음 단계:**

7장에서는 메서드의 기초를 학습하여, 문자열 처리 로직을 재사용 가능한 함수로 캡슐화하는 방법을 배우게 됩니다. 메서드를 통해 복잡한 문자열 처리 작업을 모듈화하면, 코드의 가독성, 테스트 가능성, 그리고 유지보수성이 크게 향상됩니다. 또한 LINQ(14-15장)를 학습하면 함수형 프로그래밍 패러다임을 적용하여 더욱 선언적이고 간결한 문자열 처리 코드를 작성할 수 있게 됩니다.

**연습 문제:**

#### 문제 1: 이메일 유효성 검사

사용자로부터 이메일 주소를 입력받아, `@` 기호가 포함되어 있고 `.`도 포함되어 있는지 확인하는 프로그램을 작성하세요.

```csharp
Console.Write("이메일 주소를 입력하세요: ");
string email = Console.ReadLine().Trim();

bool hasAt = email.Contains("@");
bool hasDot = email.Contains(".");
bool isValid = hasAt && hasDot && email.Length > 0;

if (isValid)
{
    Console.WriteLine("유효한 이메일 주소입니다.");
}
else
{
    Console.WriteLine("잘못된 이메일 주소입니다.");
}
```

#### 문제 2: 단어 개수 세기

문장을 입력받아 공백으로 구분된 단어의 개수를 세는 프로그램을 작성하세요.

```csharp
Console.Write("문장을 입력하세요: ");
string sentence = Console.ReadLine();

string[] words = sentence.Split(' ', StringSplitOptions.RemoveEmptyEntries);
int wordCount = words.Length;

Console.WriteLine($"단어 개수: {wordCount}");
```

#### 문제 3: 텍스트 변환기

사용자로부터 텍스트를 입력받아 다음 변환을 수행하는 프로그램을 작성하세요:
- 모든 공백을 밑줄(`_`)로 변경
- 모든 문자를 대문자로 변환
- 양 끝 공백 제거

```csharp
Console.Write("텍스트를 입력하세요: ");
string text = Console.ReadLine();

string processed = text.Trim()
                       .Replace(" ", "_")
                       .ToUpper();

Console.WriteLine($"변환 결과: {processed}");
```

#### 문제 4: StringBuilder로 구구단 생성

`StringBuilder`를 사용하여 2단부터 9단까지의 구구단을 문자열로 생성하는 프로그램을 작성하세요.

```csharp
using System.Text;

StringBuilder sb = new StringBuilder();

for (int dan = 2; dan <= 9; dan++)
{
    sb.AppendLine($"=== {dan}단 ===");
    for (int num = 1; num <= 9; num++)
    {
        sb.AppendLine($"{dan} × {num} = {dan * num}");
    }
    sb.AppendLine();
}

string result = sb.ToString();
Console.WriteLine(result);
```
