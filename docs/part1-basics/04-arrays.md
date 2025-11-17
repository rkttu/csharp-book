# 4장. 배열

3장에서 제어문을 배워 프로그램의 흐름을 제어하고 반복을 다루는 방법을 익혔습니다. 하지만 지금까지는 개별 변수로 데이터를 하나씩 저장했습니다. 만약 학급 30명의 성적을 관리하려면 어떻게 해야 할까요? `score1`, `score2`, ... `score30`처럼 30개의 변수를 만들 수는 있지만, 이는 비효율적이고 관리하기 어렵습니다.

이럴 때 필요한 것이 **배열(Array)**입니다. 배열은 같은 타입의 여러 데이터를 하나의 이름으로 묶어서 관리할 수 있는 자료구조입니다. 반복문과 함께 사용하면 많은 데이터를 효율적으로 처리할 수 있습니다.

## 이 장에서 배울 내용

- 1차원 배열의 선언, 초기화, 사용
- 2차원 배열과 가변 배열
- 배열 관련 유용한 메서드들 (`Sort`, `Reverse`, `IndexOf` 등)

---

## 4.1 1차원 배열

1차원 배열(One-Dimensional Array)은 가장 기본적이고 직관적인 형태의 배열로, 같은 타입의 요소들이 일렬로 나열된 선형 구조(Linear Structure)를 가집니다. 컴퓨터 메모리 구조의 본질적 특성인 선형성을 직접적으로 반영한 자료구조로, 메모리상에서는 물리적으로 연속된 공간에 데이터가 저장됩니다. 각 요소는 0부터 시작하는 인덱스(Index)를 통해 직접 접근(Direct Access)할 수 있으며, 이는 포인터 산술(Pointer Arithmetic)을 통해 구현되어 매우 빠른 속도를 보장합니다.

**메모리 구조와 성능 특성:**

배열의 가장 큰 장점은 **O(1) 시간 복잡도**로 임의의 요소에 접근할 수 있다는 점입니다. 이는 배열의 시작 주소와 인덱스, 그리고 요소의 크기만 알면 수학적 계산만으로 특정 요소의 메모리 주소를 즉시 구할 수 있기 때문입니다. 예를 들어, `int` 배열에서 5번째 요소의 주소는 `배열시작주소 + (5 × 4바이트)`로 계산됩니다. 이러한 특성은 링크드 리스트(Linked List)처럼 순차적으로 탐색해야 하는 O(n) 자료구조와 대비되는 배열의 핵심 강점입니다.

**배열의 핵심 특징과 설계 철학:**

1. **고정 크기 (Fixed Size)**: 한 번 생성된 배열의 크기는 변경할 수 없습니다. 이는 메모리 효율성과 접근 성능을 위한 의도적인 설계 결정으로, 컴파일러와 런타임이 메모리 레이아웃을 최적화할 수 있게 해줍니다. 동적 크기 조정이 필요한 경우에는 `List<T>`와 같은 컬렉션을 사용해야 합니다.

2. **동일 타입 (Homogeneous)**: 배열의 모든 요소는 같은 데이터 타입이어야 합니다. 이는 C#의 강력한 타입 시스템(Strong Type System)의 일부로, 컴파일 시점에 타입 안정성을 보장하고 각 요소가 동일한 메모리 크기를 가지도록 보장하여 인덱스 기반 주소 계산을 가능하게 합니다.

3. **0-기반 인덱싱 (Zero-based Indexing)**: 첫 번째 요소의 인덱스는 0이며, 마지막 요소의 인덱스는 (배열길이 - 1)입니다. 이는 C 언어 계열의 전통을 따르는 것으로, 포인터 산술에서 자연스럽게 도출되는 규칙입니다. `배열[i]`는 실제로 `*(배열주소 + i × 요소크기)`의 문법적 설탕(Syntactic Sugar)입니다.

4. **참조 타입 (Reference Type)**: 배열은 참조 타입으로, 힙(Heap) 메모리에 저장됩니다. 배열 변수는 실제 데이터가 아닌 데이터의 시작 주소를 가리키는 참조(Reference)를 저장합니다. 이는 큰 배열을 함수에 전달할 때도 복사 비용이 적게 든다는 장점이 있지만, 참조를 공유하므로 한 곳에서 수정하면 모든 참조에서 변경이 보인다는 점에 주의해야 합니다.

### 4.1.1 배열 선언과 초기화

배열을 사용하기 위해서는 먼저 배열을 선언하고 초기화해야 합니다. C#은 배열 생성을 위한 여러 가지 편리한 구문을 제공하며, 각각의 방법은 서로 다른 사용 시나리오에 최적화되어 있습니다. 이러한 다양성은 개발자가 상황에 맞는 가장 명확하고 효율적인 표현 방법을 선택할 수 있도록 하여, 코드의 가독성과 의도 전달을 향상시킵니다.

**C#에서의 배열 타입 시스템:**

C#의 배열은 `System.Array` 클래스로부터 파생된 특별한 참조 타입입니다. `int[]`, `string[]` 같은 배열 타입은 컴파일러가 자동으로 생성하는 타입으로, CLR(Common Language Runtime) 수준에서 특별한 지원을 받습니다. 이는 배열이 단순한 메모리 블록이 아니라, 길이 정보(`Length` 속성), 경계 검사(Bounds Checking), 그리고 타입 안정성을 제공하는 완전한 객체임을 의미합니다.

**기본 선언 구문:**

```csharp
// 방법 1: 크기를 지정하여 배열 생성
int[] numbers = new int[5];  // 5개의 정수를 저장할 수 있는 배열 생성

// 방법 2: 초기값과 함께 배열 생성
int[] scores = new int[] { 85, 92, 78, 95, 88 };

// 방법 3: 타입 추론을 활용한 간결한 초기화
int[] ages = { 25, 30, 22, 28, 35 };

// 방법 4: var 키워드 사용
var temperatures = new double[] { 23.5, 25.0, 22.8, 26.3 };
```

**배열 선언의 세부 사항:**

```csharp
// 배열 타입은 타입명 뒤에 대괄호[]를 붙여 표현
int[] intArray;      // 정수 배열
string[] strArray;   // 문자열 배열
double[] dblArray;   // 실수 배열
bool[] boolArray;    // 불린 배열

// 선언만 하고 초기화하지 않으면 null 상태
int[] uninitArray;
Console.WriteLine(uninitArray == null);  // True

// 배열을 생성하면 기본값으로 초기화됨
int[] defaultInts = new int[3];     // { 0, 0, 0 }
string[] defaultStrs = new string[3]; // { null, null, null }
bool[] defaultBools = new bool[3];  // { false, false, false }
```

**다양한 초기화 방법:**

```csharp
// 명시적 크기 지정
int[] numbers = new int[5];  // 5개 요소를 가진 배열, 모두 0으로 초기화

// 초기값 제공 - 크기는 자동 결정
string[] fruits = new string[] { "사과", "바나나", "오렌지" };  // 크기 3

// 타입 생략 (더 간결한 구문)
string[] colors = { "빨강", "파랑", "초록", "노랑" };  // 권장하는 방식

// 빈 배열 생성
int[] empty = new int[0];  // 또는 Array.Empty<int>()
```

**실전 예제:**

```csharp
// 학생 5명의 점수 저장
int[] scores = { 85, 92, 78, 95, 88 };

Console.WriteLine($"배열의 크기: {scores.Length}");  // 5
Console.WriteLine($"첫 번째 점수: {scores[0]}");     // 85
Console.WriteLine($"마지막 점수: {scores[4]}");       // 88

// 일주일 동안의 최고 온도 기록
double[] weekTemperatures = new double[7];
weekTemperatures[0] = 23.5;  // 월요일
weekTemperatures[1] = 25.0;  // 화요일
weekTemperatures[2] = 22.8;  // 수요일
// ... 나머지 요일들

// 출력:
// 배열의 크기: 5
// 첫 번째 점수: 85
// 마지막 점수: 88
```

**배열 크기와 Length 속성:**

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

// Length 속성: 배열의 전체 요소 개수
Console.WriteLine($"배열 크기: {numbers.Length}");  // 5

// 유효한 인덱스 범위: 0 ~ (Length - 1)
int firstIndex = 0;
int lastIndex = numbers.Length - 1;  // 4

Console.WriteLine($"첫 인덱스: {firstIndex}, 마지막 인덱스: {lastIndex}");
// 출력: 첫 인덱스: 0, 마지막 인덱스: 4
```

### 4.1.2 배열 요소 접근

배열의 각 요소는 **인덱스(Index)** 또는 **첨자(Subscript)**를 사용하여 접근할 수 있습니다. 인덱스는 대괄호 `[]` 안에 넣어 표기하며, 0부터 시작하여 (배열크기 - 1)까지의 정수 값을 가집니다. 이러한 0-기반 인덱싱(Zero-based Indexing)은 C 언어 계열의 전통을 따르는 것으로, 메모리 주소 계산의 효율성과 직접적으로 관련이 있습니다.

**0-기반 인덱싱의 수학적 근거:**

컴퓨터 메모리에서 배열의 i번째 요소 주소는 `기본주소 + (i × 요소크기)`로 계산됩니다. 0-기반 인덱싱을 사용하면 첫 번째 요소(i=0)의 주소가 정확히 기본주소가 되어, 별도의 오프셋 계산이 필요 없습니다. 만약 1-기반 인덱싱을 사용한다면 `기본주소 + ((i-1) × 요소크기)`처럼 매번 감산 연산이 추가되어 성능에 영향을 미칩니다. 이는 수십억 번 반복되는 연산에서는 무시할 수 없는 차이를 만듭니다.

**배열 경계 검사(Bounds Checking):**

C#은 배열 접근 시 자동으로 경계 검사를 수행하여 안전성을 보장합니다. 이는 C/C++에서 발생하는 버퍼 오버플로우(Buffer Overflow) 같은 심각한 보안 취약점을 원천적으로 차단합니다. 유효 범위를 벗어난 인덱스로 접근하려고 하면 `IndexOutOfRangeException` 런타임 예외가 발생하여 프로그램이 즉시 중단됩니다. 이는 약간의 성능 오버헤드를 수반하지만, 메모리 안전성이라는 훨씬 중요한 가치를 제공합니다.

**기본 요소 접근:**

```csharp
string[] fruits = { "사과", "바나나", "오렌지", "포도", "딸기" };

// 읽기(Read)
Console.WriteLine(fruits[0]);  // 사과 (첫 번째 요소)
Console.WriteLine(fruits[2]);  // 오렌지 (세 번째 요소)
Console.WriteLine(fruits[4]);  // 딸기 (마지막 요소)

// 쓰기(Write) - 요소값 변경
fruits[1] = "망고";
Console.WriteLine(fruits[1]);  // 망고 (변경됨)

// 출력:
// 사과
// 오렌지
// 딸기
// 망고
```

**인덱스 범위와 오류:**

```csharp
int[] numbers = { 10, 20, 30 };

// 유효한 인덱스: 0, 1, 2
Console.WriteLine(numbers[0]);  // ✅ 10
Console.WriteLine(numbers[1]);  // ✅ 20
Console.WriteLine(numbers[2]);  // ✅ 30

// 잘못된 인덱스 - 런타임 오류 발생!
// Console.WriteLine(numbers[3]);   // ❌ IndexOutOfRangeException
// Console.WriteLine(numbers[-1]);  // ❌ IndexOutOfRangeException
```

**인덱스 오류(IndexOutOfRangeException)와 방어적 프로그래밍:**

배열의 유효 범위를 벗어난 인덱스로 접근하려고 하면 `IndexOutOfRangeException` 런타임 오류가 발생합니다. 이는 C#에서 매우 흔한 오류 중 하나이므로, 항상 인덱스가 0 이상이고 배열 길이보다 작은지 확인해야 합니다. 방어적 프로그래밍(Defensive Programming)의 원칙에 따라, 외부 입력이나 계산된 인덱스를 사용할 때는 반드시 유효성 검증을 수행해야 합니다.

이러한 경계 검사는 C#의 안전성 철학을 잘 보여줍니다. C/C++에서는 경계를 벗어난 접근이 정의되지 않은 동작(Undefined Behavior)을 초래하여, 메모리 손상, 보안 취약점, 또는 예측 불가능한 버그를 일으킬 수 있습니다. 반면 C#은 명확한 예외를 발생시켜 문제를 즉시 드러내고, 디버깅을 용이하게 만듭니다.

```csharp
int[] data = { 1, 2, 3, 4, 5 };

// 안전한 접근 방법
int index = 10;
if (index >= 0 && index < data.Length)
{
    Console.WriteLine(data[index]);
}
else
{
    Console.WriteLine($"인덱스 {index}는 유효하지 않습니다.");
}

// 출력: 인덱스 10는 유효하지 않습니다.
```

**동적 인덱스 사용:**

```csharp
int[] scores = { 85, 92, 78, 95, 88 };

// 변수를 인덱스로 사용
int position = 2;
Console.WriteLine($"인덱스 {position}의 값: {scores[position]}");  // 78

// 수식을 인덱스로 사용
int lastIndex = scores.Length - 1;
Console.WriteLine($"마지막 요소: {scores[lastIndex]}");  // 88

// 반복문 안에서 인덱스 사용
for (int i = 0; i < scores.Length; i++)
{
    Console.WriteLine($"scores[{i}] = {scores[i]}");
}

// 출력:
// 인덱스 2의 값: 78
// 마지막 요소: 88
// scores[0] = 85
// scores[1] = 92
// scores[2] = 78
// scores[3] = 95
// scores[4] = 88
```

**C# 8.0 인덱스와 범위 연산자 - 현대적 배열 접근:**

C# 8.0부터는 더욱 편리하고 직관적인 인덱스 접근 방법이 도입되었습니다. 이는 Python 같은 언어의 슬라이싱(Slicing) 기능에서 영감을 받은 것으로, 배열 조작의 표현력을 크게 향상시켰습니다. 특히 **인덱스 구조체(Index struct)**와 **범위 구조체(Range struct)**는 컴파일러의 특별한 지원을 받아 효율적으로 변환됩니다.

**인덱스 연산자 (`^`):**
- `^N` 표기법은 "뒤에서 N번째"를 의미합니다
- `^1`은 마지막 요소, `^2`는 뒤에서 두 번째 요소
- 내부적으로는 `length - N`으로 변환되어 추가 오버헤드가 거의 없습니다
- 음수 인덱스를 직접 지원하지 않던 C#의 제약을 우아하게 해결

**범위 연산자 (`..`):**
- 부분 배열(Slice)을 생성하는 간결한 문법 제공
- `start..end` 형태로, start는 포함(inclusive), end는 제외(exclusive)
- 내부적으로 새로운 배열을 할당하고 복사하므로, 대용량 데이터에서는 성능 고려 필요
- `..`(처음부터 끝까지), `..n`(처음부터 n-1까지), `n..`(n부터 끝까지) 같은 축약 표현 지원

이러한 기능들은 함수형 프로그래밍의 영향을 받은 것으로, 코드를 더 선언적(Declarative)이고 읽기 쉽게 만들어줍니다.

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

// ^N: 뒤에서 N번째 요소 (^1은 마지막, ^2는 뒤에서 두 번째)
Console.WriteLine(numbers[^1]);   // 50 (마지막 요소)
Console.WriteLine(numbers[^2]);   // 40 (뒤에서 두 번째)
Console.WriteLine(numbers[^5]);   // 10 (뒤에서 다섯 번째 = 첫 번째)

// 범위 연산자 ..: 부분 배열 (슬라이싱)
int[] slice1 = numbers[1..4];     // 인덱스 1, 2, 3 (4는 포함 안 됨)
// slice1 = { 20, 30, 40 }

int[] slice2 = numbers[..3];      // 처음부터 인덱스 2까지
// slice2 = { 10, 20, 30 }

int[] slice3 = numbers[2..];      // 인덱스 2부터 끝까지
// slice3 = { 30, 40, 50 }

int[] all = numbers[..];          // 전체 복사
// all = { 10, 20, 30, 40, 50 }

// 출력:
// 50
// 40
// 10
```

### 4.1.3 배열 순회

배열 순회(Array Traversal 또는 Array Iteration)는 배열의 모든 요소를 하나씩 방문하여 처리하는 작업으로, 배열을 다루는 프로그래밍에서 가장 빈번하게 수행되는 연산 중 하나입니다. C#은 배열을 순회하는 여러 가지 방법을 제공하며, 각 방법은 서로 다른 사용 시나리오와 성능 특성을 가지고 있습니다. 적절한 순회 방법을 선택하는 것은 코드의 명확성과 실행 효율성 모두에 영향을 미칩니다.

**반복자 패턴(Iterator Pattern)의 구현:**

배열 순회는 디자인 패턴 중 하나인 반복자 패턴의 구체적 실현입니다. 이 패턴은 컬렉션의 내부 구조를 노출하지 않으면서도 요소들을 순차적으로 접근할 수 있게 해주는 방법을 제공합니다. C#의 `IEnumerable<T>` 인터페이스와 `foreach` 문은 이 패턴의 언어 차원 지원입니다.

**for 문을 사용한 순회 - 인덱스 기반 접근의 유연성:**

`for` 문은 인덱스가 필요하거나 배열을 역순으로 순회해야 할 때, 또는 순회 중 배열 요소를 수정해야 할 때 가장 적합한 방법입니다. 인덱스에 대한 완전한 제어권을 제공하므로, 건너뛰기(skip), 역순 순회, 부분 범위 순회 등 다양한 패턴을 구현할 수 있습니다.

**for 문의 성능 특성:**
- 컴파일러 최적화가 매우 잘 적용되는 구조
- JIT(Just-In-Time) 컴파일러가 배열 경계 검사를 최적화할 수 있음
- 인덱스 변수가 명시적이므로 복잡한 인덱싱 로직 구현 가능
- 벡터화(Vectorization) 같은 SIMD 최적화의 대상이 될 수 있음

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

// 전통적인 for 문 순회
Console.WriteLine("정방향 순회:");
for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine($"numbers[{i}] = {numbers[i]}");
}

// 역방향 순회
Console.WriteLine("\n역방향 순회:");
for (int i = numbers.Length - 1; i >= 0; i--)
{
    Console.WriteLine($"numbers[{i}] = {numbers[i]}");
}

// 출력:
// 정방향 순회:
// numbers[0] = 10
// numbers[1] = 20
// numbers[2] = 30
// numbers[3] = 40
// numbers[4] = 50
//
// 역방향 순회:
// numbers[4] = 50
// numbers[3] = 40
// numbers[2] = 30
// numbers[1] = 20
// numbers[0] = 10
```

**foreach 문을 사용한 순회 - 안전성과 가독성의 균형:**

`foreach` 문은 배열의 모든 요소를 순차적으로 읽기만 할 때 가장 간결하고 안전한 방법입니다. 이는 C#의 반복자 패턴(Iterator Pattern) 구현의 핵심이며, `IEnumerable<T>` 인터페이스를 통해 작동합니다.

**foreach의 내부 동작 메커니즘:**

컴파일러는 `foreach` 문을 `IEnumerator<T>`를 사용하는 `while` 루프로 변환합니다. 배열의 경우, 이 과정은 고도로 최적화되어 있어 `for` 문과 거의 동일한 성능을 보입니다. 다음 코드는 개념적으로 동일합니다:

```csharp
// foreach 문
foreach (int item in array) { /* ... */ }

// 컴파일러가 생성하는 코드 (개념적)
var enumerator = array.GetEnumerator();
try {
    while (enumerator.MoveNext()) {
        int item = (int)enumerator.Current;
        /* ... */
    }
} finally {
    enumerator.Dispose();  // 리소스 정리
}
```

**foreach의 장점과 설계 의도:**
- **안전성**: 인덱스 범위 오류가 원천적으로 불가능
- **가독성**: 코드의 의도("모든 요소를 순회한다")가 명확히 드러남
- **추상화**: 내부 구조와 무관하게 동작 (배열, 리스트, 트리 모두 동일한 방식)
- **컴파일러 지원**: 타입 추론, 분해(Deconstruction), 비동기 스트림 등과 통합

**foreach의 제약 사항과 그 철학적 근거:**

`foreach`의 반복 변수는 읽기 전용(read-only)입니다. 이는 반복 중 컬렉션의 무결성을 보장하기 위한 의도적인 설계입니다. 컬렉션을 순회하면서 동시에 수정하는 것은 많은 버그의 원인이 되며, "순회 중 수정 금지(No modification during iteration)" 원칙은 많은 자료구조에서 중요한 불변조건(Invariant)입니다.

```csharp
string[] fruits = { "사과", "바나나", "오렌지", "포도" };

Console.WriteLine("과일 목록:");
foreach (string fruit in fruits)
{
    Console.WriteLine($"- {fruit}");
}

// 출력:
// 과일 목록:
// - 사과
// - 바나나
// - 오렌지
// - 포도
```

**foreach의 장점과 제약:**

```csharp
int[] scores = { 85, 92, 78, 95, 88 };

// 장점: 간결하고 안전함 (인덱스 오류 없음)
int sum = 0;
foreach (int score in scores)
{
    sum += score;
}
Console.WriteLine($"총점: {sum}");  // 438

// 제약: 요소를 수정할 수 없음 (읽기 전용)
// foreach (int score in scores)
// {
//     score = score + 10;  // ❌ 컴파일 오류: 반복 변수를 수정할 수 없음
// }

// 요소를 수정하려면 for 문 사용
for (int i = 0; i < scores.Length; i++)
{
    scores[i] = scores[i] + 10;  // ✅ 가능
}
```

**실전 예제: 배열의 최대값 찾기:**

```csharp
int[] numbers = { 45, 23, 89, 12, 67, 34, 91, 56 };

// for 문 사용
int max1 = numbers[0];
for (int i = 1; i < numbers.Length; i++)
{
    if (numbers[i] > max1)
    {
        max1 = numbers[i];
    }
}
Console.WriteLine($"최대값 (for): {max1}");  // 91

// foreach 문 사용
int max2 = numbers[0];
foreach (int num in numbers)
{
    if (num > max2)
    {
        max2 = num;
    }
}
Console.WriteLine($"최대값 (foreach): {max2}");  // 91

// 출력:
// 최대값 (for): 91
// 최대값 (foreach): 91
```

**실전 예제: 배열의 평균 계산:**

```csharp
double[] temperatures = { 23.5, 25.0, 22.8, 26.3, 24.7, 23.9, 25.5 };

double sum = 0;
foreach (double temp in temperatures)
{
    sum += temp;
}

double average = sum / temperatures.Length;
Console.WriteLine($"일주일 평균 온도: {average:F1}도");

// 출력: 일주일 평균 온도: 24.5도
```

**while 문을 사용한 순회:**

```csharp
string[] cities = { "서울", "부산", "대구", "인천", "광주" };

int index = 0;
while (index < cities.Length)
{
    Console.WriteLine($"{index + 1}. {cities[index]}");
    index++;
}

// 출력:
// 1. 서울
// 2. 부산
// 3. 대구
// 4. 인천
// 5. 광주
```

---

## 4.2 다차원 배열

지금까지 살펴본 1차원 배열은 데이터를 일렬로 나열하는 선형 구조였습니다. 하지만 실제 프로그래밍에서는 표(Table), 행렬(Matrix), 게임 보드처럼 2차원 이상의 구조로 데이터를 표현해야 하는 경우가 매우 많습니다. 엑셀 스프레드시트의 셀, 이미지의 픽셀, 체스판의 칸, 3D 공간의 좌표 등은 모두 다차원 데이터의 예입니다.

C#은 이러한 필요를 충족시키기 위해 두 가지 방식의 다차원 배열을 제공합니다: **다차원 배열(Multidimensional Array)** 또는 **직사각형 배열(Rectangular Array)**이라 불리는 `[,]` 형태와, **가변 배열(Jagged Array)** 또는 **톱니 배열**이라 불리는 `[][]` 형태입니다. 이 두 방식은 메모리 레이아웃, 성능 특성, 사용 시나리오에서 중요한 차이를 가집니다.

**다차원 데이터 구조의 컴퓨터 과학적 의미:**

다차원 배열은 수학의 벡터와 행렬 개념을 프로그래밍 언어로 구현한 것입니다. 선형대수학에서 행렬은 2차원 숫자 배열로, 데이터 분석, 그래픽스, 물리 시뮬레이션, 기계 학습 등 많은 분야에서 핵심적입니다. C#의 다차원 배열은 이러한 수학적 추상화를 효율적으로 지원합니다.

### 4.2.1 2차원 배열

2차원 배열(Two-Dimensional Array)은 행(Row)과 열(Column)로 구성된 표나 행렬 구조를 표현하는 데 사용됩니다. 수학의 행렬, 엑셀의 스프레드시트, 체스판이나 오목판 같은 게임 보드, 이미지의 픽셀 그리드 등을 코드로 표현할 때 매우 유용합니다.

**메모리 레이아웃과 Row-Major 순서:**

C#의 2차원 배열은 메모리상에서 **행 우선 순서(Row-Major Order)**로 저장됩니다. 이는 `[0,0], [0,1], [0,2], ..., [1,0], [1,1], ...` 순서로 메모리에 연속적으로 배치된다는 의미입니다. 이러한 레이아웃은 다음과 같은 특성을 가집니다:

- **캐시 지역성(Cache Locality)**: 행 단위로 접근할 때 CPU 캐시 효율이 좋습니다
- **메모리 연속성**: 모든 요소가 하나의 연속된 메모리 블록에 저장됩니다
- **성능 예측 가능성**: 인덱스 계산이 수학적으로 명확합니다 (`기본주소 + (행*열수 + 열) × 요소크기`)

이는 Fortran의 열 우선 순서(Column-Major Order)와 대비되는 C 계열 언어의 전통입니다. 행렬 곱셈 같은 연산에서 메모리 접근 패턴을 고려하면 성능 차이가 발생할 수 있습니다.

**직사각형 배열의 특징과 제약:**

2차원 배열은 "직사각형(Rectangular)" 구조를 강제합니다. 모든 행은 반드시 동일한 개수의 열을 가져야 하며, 이는 컴파일 시점에 결정됩니다. 이러한 제약은 메모리 레이아웃의 예측 가능성과 성능 최적화를 위한 것으로, 런타임 오버헤드를 최소화합니다.

**2차원 배열의 선언과 초기화:**

```csharp
// 방법 1: 크기를 지정하여 생성
int[,] matrix = new int[3, 4];  // 3행 4열의 2차원 배열

// 방법 2: 초기값과 함께 생성
int[,] scores = new int[,]
{
    { 85, 92, 78 },  // 1행
    { 95, 88, 91 },  // 2행
    { 82, 89, 93 }   // 3행
};

// 방법 3: 간결한 초기화
int[,] grid = {
    { 1, 2, 3, 4 },
    { 5, 6, 7, 8 },
    { 9, 10, 11, 12 }
};
```

**2차원 배열의 요소 접근:**

```csharp
int[,] table = {
    { 10, 20, 30 },
    { 40, 50, 60 },
    { 70, 80, 90 }
};

// 요소 읽기: [행, 열]
Console.WriteLine(table[0, 0]);  // 10 (1행 1열)
Console.WriteLine(table[1, 2]);  // 60 (2행 3열)
Console.WriteLine(table[2, 1]);  // 80 (3행 2열)

// 요소 쓰기
table[1, 1] = 55;
Console.WriteLine(table[1, 1]);  // 55 (변경됨)

// 출력:
// 10
// 60
// 80
// 55
```

**2차원 배열의 크기 정보:**

```csharp
int[,] matrix = {
    { 1, 2, 3, 4 },
    { 5, 6, 7, 8 },
    { 9, 10, 11, 12 }
};

// GetLength(차원): 특정 차원의 길이
int rows = matrix.GetLength(0);     // 3 (행의 개수)
int cols = matrix.GetLength(1);     // 4 (열의 개수)

// Length: 전체 요소의 개수
int totalElements = matrix.Length;  // 12 (3 × 4)

Console.WriteLine($"행: {rows}, 열: {cols}, 전체: {totalElements}");

// 출력: 행: 3, 열: 4, 전체: 12
```

**2차원 배열의 순회:**

```csharp
int[,] grid = {
    { 1, 2, 3 },
    { 4, 5, 6 },
    { 7, 8, 9 }
};

// 중첩 for 문을 사용한 순회
Console.WriteLine("2차원 배열 순회:");
for (int i = 0; i < grid.GetLength(0); i++)  // 행 반복
{
    for (int j = 0; j < grid.GetLength(1); j++)  // 열 반복
    {
        Console.Write($"{grid[i, j],3}");
    }
    Console.WriteLine();
}

// 출력:
// 2차원 배열 순회:
//   1  2  3
//   4  5  6
//   7  8  9
```

**foreach를 사용한 2차원 배열 순회:**

```csharp
int[,] numbers = {
    { 10, 20 },
    { 30, 40 },
    { 50, 60 }
};

// foreach는 모든 요소를 1차원처럼 순회
int sum = 0;
foreach (int num in numbers)
{
    sum += num;
    Console.Write($"{num} ");
}
Console.WriteLine($"\n합계: {sum}");

// 출력:
// 10 20 30 40 50 60
// 합계: 210
```

**실전 예제: 학생 성적 관리:**

```csharp
// 3명의 학생, 4개의 과목 점수
int[,] studentScores = {
    { 85, 92, 78, 88 },  // 학생 1
    { 90, 85, 95, 92 },  // 학생 2
    { 78, 88, 82, 85 }   // 학생 3
};

string[] subjects = { "국어", "영어", "수학", "과학" };

// 각 학생의 평균 계산
for (int i = 0; i < studentScores.GetLength(0); i++)
{
    int sum = 0;
    for (int j = 0; j < studentScores.GetLength(1); j++)
    {
        sum += studentScores[i, j];
    }
    double average = (double)sum / studentScores.GetLength(1);
    Console.WriteLine($"학생 {i + 1}의 평균: {average:F1}점");
}

// 출력:
// 학생 1의 평균: 85.8점
// 학생 2의 평균: 90.5점
// 학생 3의 평균: 83.2점
```

### 4.2.2 가변 배열 (Jagged Array)

가변 배열(Jagged Array), 또는 톱니 배열은 **배열의 배열(Array of Arrays)**입니다. 2차원 배열과 달리, 각 행이 독립적인 배열로 존재하며 서로 다른 길이를 가질 수 있어 메모리를 효율적으로 사용할 수 있습니다. "Jagged"라는 이름은 각 행의 길이가 달라 배열의 모양이 톱니처럼 들쭉날쭉하다는 의미에서 유래했습니다.

**가변 배열의 메모리 구조와 성능 특성:**

가변 배열은 2차원 배열과 근본적으로 다른 메모리 레이아웃을 가집니다:

```
2차원 배열 [,]:
[메모리 블록: 모든 요소가 연속적으로 배치]
[0,0][0,1][0,2][1,0][1,1][1,2][2,0][2,1][2,2]

가변 배열 [][]:
[배열 참조들] → [첫 번째 배열: 요소들]
              → [두 번째 배열: 요소들]
              → [세 번째 배열: 요소들]
```

**가변 배열의 특성:**

1. **간접 참조(Indirection)**: 요소 접근 시 두 번의 포인터 추적이 필요합니다 (배열의 배열 → 내부 배열 → 요소). 이는 추가적인 메모리 접근을 의미하지만, 현대 CPU의 캐시와 분기 예측으로 인해 실제 성능 차이는 미미할 수 있습니다.

2. **메모리 단편화 가능성**: 각 행이 힙의 서로 다른 위치에 할당될 수 있어, 2차원 배열의 연속성에 비해 캐시 효율이 떨어질 수 있습니다.

3. **유연한 구조**: 각 행의 길이가 다를 수 있어, 삼각 행렬, 파스칼의 삼각형, 가변 길이 데이터 등을 표현할 때 메모리를 절약할 수 있습니다.

4. **동적 할당**: 각 행을 서로 다른 시점에 할당할 수 있어, 데이터의 크기를 동적으로 결정해야 하는 경우에 유리합니다.

**2차원 배열 vs 가변 배열 - 언제 무엇을 선택할 것인가:**

| 측면 | 2차원 배열 `[,]` | 가변 배열 `[][]` |
|------|-----------------|-----------------|
| 메모리 레이아웃 | 연속적, 단일 블록 | 분산적, 여러 블록 |
| 접근 시간 | O(1), 단일 계산 | O(1), 이중 간접 참조 |
| 캐시 효율 | 우수 (행 단위 접근 시) | 보통 (행은 비연속적) |
| 메모리 효율 | 고정 크기만 가능 | 가변 크기 지원 |
| 배열 교체 | 불가능 | 가능 (각 행 독립적) |
| 사용 사례 | 행렬, 게임 보드, 이미지 | 그래프, 가변 길이 데이터 |
| 문법 복잡도 | 단순 `[i, j]` | 중첩 `[i][j]` |

**실무에서의 선택 가이드:**

- **균일한 크기의 데이터**: 2차원 배열 사용 (더 간단하고 빠름)
- **행마다 크기가 다른 데이터**: 가변 배열 사용 (메모리 절약)
- **행 전체를 자주 교체**: 가변 배열 사용 (각 행이 독립적)
- **성능이 극도로 중요**: 벤치마크 후 결정 (상황에 따라 다름)

가변 배열은 C 언어의 포인터 배열과 유사한 개념으로, 더 복잡하지만 더 유연합니다. C#에서는 타입 안정성을 유지하면서도 이러한 유연성을 제공합니다.

**가변 배열의 선언과 초기화:**

```csharp
// 방법 1: 단계적 생성
int[][] triangle = new int[3][];
triangle[0] = new int[] { 1 };
triangle[1] = new int[] { 2, 3 };
triangle[2] = new int[] { 4, 5, 6 };

// 방법 2: 초기값과 함께 생성
int[][] numbers = new int[][]
{
    new int[] { 1, 2, 3 },
    new int[] { 4, 5 },
    new int[] { 6, 7, 8, 9 }
};

// 방법 3: 간결한 초기화
int[][] data = {
    new[] { 10, 20 },
    new[] { 30, 40, 50 },
    new[] { 60 }
};
```

**가변 배열의 요소 접근:**

```csharp
int[][] jagged = {
    new[] { 1, 2, 3 },
    new[] { 4, 5 },
    new[] { 6, 7, 8, 9 }
};

// 요소 접근: [행][열]
Console.WriteLine(jagged[0][0]);  // 1
Console.WriteLine(jagged[1][1]);  // 5
Console.WriteLine(jagged[2][3]);  // 9

// 각 행의 길이가 다름
Console.WriteLine($"1행 길이: {jagged[0].Length}");  // 3
Console.WriteLine($"2행 길이: {jagged[1].Length}");  // 2
Console.WriteLine($"3행 길이: {jagged[2].Length}");  // 4

// 출력:
// 1
// 5
// 9
// 1행 길이: 3
// 2행 길이: 2
// 3행 길이: 4
```

**가변 배열의 순회:**

```csharp
int[][] pyramid = {
    new[] { 1 },
    new[] { 2, 3 },
    new[] { 4, 5, 6 },
    new[] { 7, 8, 9, 10 }
};

Console.WriteLine("피라미드 구조:");
for (int i = 0; i < pyramid.Length; i++)
{
    for (int j = 0; j < pyramid[i].Length; j++)
    {
        Console.Write($"{pyramid[i][j]} ");
    }
    Console.WriteLine();
}

// 출력:
// 피라미드 구조:
// 1
// 2 3
// 4 5 6
// 7 8 9 10
```

**foreach를 사용한 가변 배열 순회:**

```csharp
string[][] teams = {
    new[] { "김철수", "이영희" },
    new[] { "박민수", "최지혜", "정다은" },
    new[] { "홍길동" }
};

int teamNumber = 1;
foreach (string[] team in teams)
{
    Console.WriteLine($"팀 {teamNumber}:");
    foreach (string member in team)
    {
        Console.WriteLine($"  - {member}");
    }
    teamNumber++;
}

// 출력:
// 팀 1:
//   - 김철수
//   - 이영희
// 팀 2:
//   - 박민수
//   - 최지혜
//   - 정다은
// 팀 3:
//   - 홍길동
```

**가변 배열 vs 2차원 배열 비교:**

```csharp
// 2차원 배열의 특징:
// - 직사각형 구조 (모든 행이 같은 길이)
// - 메모리 상에 연속적으로 배치
// - 문법: [,]
int[,] rect = new int[3, 4];

// 가변 배열의 특징:
// - 각 행이 독립적인 배열 (길이가 다를 수 있음)
// - 메모리 효율적 (필요한 만큼만 할당)
// - 문법: [][]
int[][] jagg = new int[3][];
```

**실전 예제: 월별 일수 표현:**

```csharp
// 각 월의 일수 (2월은 28일로 가정)
int[][] monthDays = {
    new int[31],  // 1월: 31일
    new int[28],  // 2월: 28일
    new int[31],  // 3월: 31일
    new int[30],  // 4월: 30일
    new int[31],  // 5월: 31일
    new int[30],  // 6월: 30일
    new int[31],  // 7월: 31일
    new int[31],  // 8월: 31일
    new int[30],  // 9월: 30일
    new int[31],  // 10월: 31일
    new int[30],  // 11월: 30일
    new int[31]   // 12월: 31일
};

// 각 월의 일수 출력
for (int i = 0; i < monthDays.Length; i++)
{
    Console.WriteLine($"{i + 1}월: {monthDays[i].Length}일");
}

// 출력:
// 1월: 31일
// 2월: 28일
// 3월: 31일
// ...
```

---

## 4.3 배열 관련 메서드

C#의 `Array` 클래스는 배열을 효과적으로 다루기 위한 다양한 정적 메서드(Static Methods)를 제공합니다. 이러한 메서드들을 활용하면 정렬, 검색, 복사, 역순 배치 등의 일반적인 배열 조작을 간단하고 효율적으로 수행할 수 있습니다. 이는 표준 라이브러리(Standard Library)의 철학을 잘 보여주는 예로, "바퀴를 다시 발명하지 말라(Don't Reinvent the Wheel)"는 원칙을 구현합니다.

**Array 클래스의 설계 철학:**

`System.Array`는 모든 배열 타입의 추상 기본 클래스로, .NET의 타입 시스템에서 특별한 위치를 차지합니다. CLR(Common Language Runtime) 수준에서 특별히 지원되며, 다음과 같은 특징을 가집니다:

1. **제네릭 이전 설계**: .NET 1.0부터 존재하여 `object` 기반으로 동작하지만, 내부적으로는 타입 안정성 유지
2. **알고리즘 추상화**: 구체적인 배열 타입과 무관하게 동작하는 범용 알고리즘 제공
3. **성능 최적화**: 네이티브 코드로 구현된 경우가 많아 직접 구현보다 빠름
4. **BCL 통합**: LINQ, 컬렉션 등 .NET 생태계와 긴밀히 통합

이러한 메서드들은 검증된 알고리즘을 사용하므로, 직접 구현하는 것보다 정확하고 안정적입니다.

### 4.3.1 Array.Sort()

`Array.Sort()` 메서드는 배열의 요소를 오름차순으로 정렬합니다. 이 메서드는 원본 배열을 직접 수정(In-place Modification)하며, 대부분의 기본 타입과 `IComparable` 인터페이스를 구현한 모든 타입에 대해 동작합니다.

**정렬 알고리즘의 이론과 구현:**

`Array.Sort()`는 내부적으로 **인트로소트(Introsort)** 알고리즘을 사용합니다. 이는 Microsoft의 .NET 팀이 선택한 하이브리드 정렬 알고리즘으로, 다음 세 가지 알고리즘을 상황에 따라 조합합니다:

1. **퀵소트(Quicksort)**: 일반적인 경우에 O(n log n)의 우수한 평균 성능
2. **힙소트(Heapsort)**: 재귀 깊이가 과도할 때 최악의 경우를 방지 (O(n log n) 보장)
3. **삽입 정렬(Insertion Sort)**: 작은 배열(16개 이하)에서의 오버헤드 최소화

이러한 하이브리드 접근은 1997년 David Musser가 제안한 것으로, 실전에서 안정적이고 예측 가능한 성능을 제공합니다. 평균 O(n log n), 최악 O(n log n), 추가 공간 O(log n)의 특성을 가집니다.

**비교 방식과 IComparable:**

정렬을 위해서는 요소 간의 대소 관계를 판단해야 합니다. C#은 이를 위해 `IComparable<T>` 또는 `IComparable` 인터페이스를 사용합니다. 기본 타입(`int`, `string`, `double` 등)은 모두 이 인터페이스를 구현하고 있어 추가 코드 없이 정렬할 수 있습니다.

```csharp
public interface IComparable<T>
{
    // this가 other보다 작으면 음수
    // this가 other와 같으면 0
    // this가 other보다 크면 양수를 반환
    int CompareTo(T other);
}
```

**정렬의 안정성(Stability):**

`Array.Sort()`는 **불안정 정렬(Unstable Sort)**입니다. 즉, 동일한 값을 가진 요소들의 상대적 순서가 보존되지 않을 수 있습니다. 안정 정렬이 필요한 경우 `Enumerable.OrderBy()` (LINQ)를 사용해야 합니다. 이는 성능과 안정성 간의 트레이드오프를 보여줍니다.

**기본 사용법:**

```csharp
int[] numbers = { 45, 23, 89, 12, 67, 34 };

Console.WriteLine("정렬 전:");
foreach (int num in numbers)
{
    Console.Write($"{num} ");
}

Array.Sort(numbers);

Console.WriteLine("\n정렬 후:");
foreach (int num in numbers)
{
    Console.Write($"{num} ");
}

// 출력:
// 정렬 전:
// 45 23 89 12 67 34
// 정렬 후:
// 12 23 34 45 67 89
```

**문자열 배열 정렬:**

```csharp
string[] names = { "최지혜", "김철수", "박민수", "이영희" };

Array.Sort(names);

Console.WriteLine("이름 정렬 (가나다순):");
foreach (string name in names)
{
    Console.WriteLine(name);
}

// 출력:
// 이름 정렬 (가나다순):
// 김철수
// 박민수
// 이영희
// 최지혜
```

**부분 정렬:**

```csharp
int[] data = { 50, 30, 70, 10, 90, 20 };

// 인덱스 1부터 3개 요소만 정렬
Array.Sort(data, 1, 3);

Console.WriteLine("부분 정렬 결과:");
foreach (int num in data)
{
    Console.Write($"{num} ");
}

// 출력:
// 부분 정렬 결과:
// 50 10 30 70 90 20
```

**내림차순 정렬:**

```csharp
int[] scores = { 85, 92, 78, 95, 88 };

// 오름차순 정렬 후 역순 배치
Array.Sort(scores);
Array.Reverse(scores);

Console.WriteLine("내림차순 정렬:");
foreach (int score in scores)
{
    Console.Write($"{score} ");
}

// 출력:
// 내림차순 정렬:
// 95 92 88 85 78
```

**실전 예제: 성적 순위 매기기:**

```csharp
string[] students = { "김철수", "이영희", "박민수", "최지혜" };
int[] scores = { 85, 95, 78, 92 };

// 점수와 이름을 함께 정렬하기 위해 Array.Sort의 두 배열 버전 사용
Array.Sort(scores, students);

Console.WriteLine("성적 순위 (낮은 순):");
for (int i = 0; i < students.Length; i++)
{
    Console.WriteLine($"{i + 1}등: {students[i]} ({scores[i]}점)");
}

// 내림차순으로 보려면 역순 배치
Array.Reverse(scores);
Array.Reverse(students);

Console.WriteLine("\n성적 순위 (높은 순):");
for (int i = 0; i < students.Length; i++)
{
    Console.WriteLine($"{i + 1}등: {students[i]} ({scores[i]}점)");
}

// 출력:
// 성적 순위 (낮은 순):
// 1등: 박민수 (78점)
// 2등: 김철수 (85점)
// 3등: 최지혜 (92점)
// 4등: 이영희 (95점)
//
// 성적 순위 (높은 순):
// 1등: 이영희 (95점)
// 2등: 최지혜 (92점)
// 3등: 김철수 (85점)
// 4등: 박민수 (78점)
```

### 4.3.2 Array.Reverse()

`Array.Reverse()` 메서드는 배열의 요소 순서를 역순으로 뒤집습니다. 이 메서드 역시 원본 배열을 직접 수정(In-place Operation)하며, 추가 메모리 할당 없이 O(n/2) 시간에 동작합니다.

**역순 배치 알고리즘의 원리:**

`Array.Reverse()`는 매우 간단하지만 효율적인 알고리즘을 사용합니다. 배열의 양 끝에서 시작하여 중앙을 향해 이동하면서 요소를 교환(Swap)합니다:

```
초기: [1, 2, 3, 4, 5]
1회: [5, 2, 3, 4, 1]  // 1↔5 교환
2회: [5, 4, 3, 2, 1]  // 2↔4 교환
완료: [5, 4, 3, 2, 1]  // 3은 중앙이므로 교환 불필요
```

**시간 복잡도 분석:**
- 정확히 `n/2`번의 교환 수행 (n은 배열 길이)
- 각 교환은 O(1) 상수 시간
- 총 시간 복잡도: O(n)
- 공간 복잡도: O(1) - 제자리(in-place) 알고리즘

이러한 효율성은 역순 배치를 별도의 배열 생성 없이 수행할 수 있게 하여, 메모리 효율적입니다.

**실용적 활용 패턴:**

역순 배치는 다양한 상황에서 유용합니다:
- 스택(Stack) 구조를 배열로 구현할 때
- 카운트다운이나 역순 출력
- 정렬과 조합하여 내림차순 정렬 구현
- 알고리즘에서 양방향 순회가 필요한 경우

특히 `Array.Sort()`와 `Array.Reverse()`를 조합하면 간단하게 내림차순 정렬을 구현할 수 있습니다. 이는 직접 비교 함수를 작성하는 것보다 간결하고 명확합니다.

**기본 사용법:**

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };

Console.WriteLine("원본 배열:");
foreach (int num in numbers)
{
    Console.Write($"{num} ");
}

Array.Reverse(numbers);

Console.WriteLine("\n역순 배열:");
foreach (int num in numbers)
{
    Console.Write($"{num} ");
}

// 출력:
// 원본 배열:
// 1 2 3 4 5
// 역순 배열:
// 5 4 3 2 1
```

**문자열 배열 역순:**

```csharp
string[] days = { "월요일", "화요일", "수요일", "목요일", "금요일" };

Array.Reverse(days);

Console.WriteLine("역순 요일:");
foreach (string day in days)
{
    Console.WriteLine(day);
}

// 출력:
// 역순 요일:
// 금요일
// 목요일
// 수요일
// 화요일
// 월요일
```

**부분 역순:**

```csharp
int[] data = { 10, 20, 30, 40, 50, 60 };

// 인덱스 1부터 3개 요소만 역순
Array.Reverse(data, 1, 3);

Console.WriteLine("부분 역순 결과:");
foreach (int num in data)
{
    Console.Write($"{num} ");
}

// 출력:
// 부분 역순 결과:
// 10 40 30 20 50 60
```

**실전 예제: 카운트다운:**

```csharp
int[] countdown = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

Array.Reverse(countdown);

Console.WriteLine("발사 카운트다운:");
foreach (int num in countdown)
{
    Console.Write($"{num}... ");
}
Console.WriteLine("발사!");

// 출력:
// 발사 카운트다운:
// 10... 9... 8... 7... 6... 5... 4... 3... 2... 1... 발사!
```

### 4.3.3 Array.IndexOf()

`Array.IndexOf()` 메서드는 배열에서 특정 값을 검색하여 그 값이 처음 나타나는 위치의 인덱스를 반환합니다. 값을 찾지 못하면 -1을 반환하는데, 이는 유효하지 않은 인덱스를 나타내는 관례적인 표현입니다.

**선형 검색(Linear Search) 알고리즘:**

`Array.IndexOf()`는 **선형 검색(Linear Search)** 또는 **순차 검색(Sequential Search)** 알고리즘을 사용합니다. 배열의 첫 번째 요소부터 시작하여 순차적으로 각 요소를 목표 값과 비교하며, 일치하는 요소를 발견하면 즉시 그 인덱스를 반환합니다.

**알고리즘 분석:**
- **최선의 경우(Best Case)**: O(1) - 첫 번째 요소가 목표 값
- **평균의 경우(Average Case)**: O(n/2) - 중간쯤에서 발견
- **최악의 경우(Worst Case)**: O(n) - 마지막 요소이거나 존재하지 않음
- **공간 복잡도**: O(1) - 추가 메모리 불필요

**정렬된 배열에서의 검색 최적화:**

만약 배열이 이미 정렬되어 있다면, `Array.BinarySearch()` 메서드를 사용하는 것이 훨씬 효율적입니다. 이진 검색은 O(log n) 시간에 동작하여, 큰 배열에서는 현저한 성능 차이를 보입니다:

```
선형 검색 vs 이진 검색 비교 (n = 1,000,000):
- Array.IndexOf(): 최악 1,000,000번 비교
- Array.BinarySearch(): 최악 20번 비교 (log₂ 1,000,000 ≈ 20)
```

**동등성 비교의 의미론:**

`IndexOf()`는 요소 비교 시 `Equals()` 메서드를 사용합니다. 참조 타입의 경우, 참조가 아닌 값의 동등성을 비교하려면 해당 타입이 `Equals()`를 적절히 오버라이드해야 합니다. 이는 C#의 값 기반 동등성(Value Equality) 개념과 연결됩니다.

**-1 반환 규약의 역사:**

요소를 찾지 못했을 때 -1을 반환하는 것은 C 언어 시절부터 내려온 오랜 관례입니다. 이는 다음과 같은 이유로 선택되었습니다:
- 유효한 인덱스는 항상 0 이상이므로 명확히 구분됨
- null이나 예외보다 간단하고 효율적
- 조건문에서 `if (index >= 0)` 같은 자연스러운 검사 가능

현대적인 대안으로는 `FindIndex()` 메서드나 nullable 인덱스를 사용할 수도 있지만, 후방 호환성과 성능을 위해 -1 규약이 유지됩니다.

**기본 사용법:**

```csharp
int[] numbers = { 10, 20, 30, 40, 50, 30, 60 };

int index1 = Array.IndexOf(numbers, 30);
Console.WriteLine($"30의 인덱스: {index1}");  // 2 (첫 번째 30의 위치)

int index2 = Array.IndexOf(numbers, 100);
Console.WriteLine($"100의 인덱스: {index2}");  // -1 (없음)

// 출력:
// 30의 인덱스: 2
// 100의 인덱스: -1
```

**문자열 검색:**

```csharp
string[] fruits = { "사과", "바나나", "오렌지", "포도", "딸기" };

int index = Array.IndexOf(fruits, "오렌지");

if (index != -1)
{
    Console.WriteLine($"오렌지는 인덱스 {index}에 있습니다.");
}
else
{
    Console.WriteLine("오렌지를 찾을 수 없습니다.");
}

// 출력: 오렌지는 인덱스 2에 있습니다.
```

**특정 위치부터 검색:**

```csharp
int[] data = { 5, 10, 15, 10, 20, 10, 25 };

// 처음부터 검색
int first = Array.IndexOf(data, 10);
Console.WriteLine($"첫 번째 10: 인덱스 {first}");  // 1

// 인덱스 2부터 검색
int second = Array.IndexOf(data, 10, 2);
Console.WriteLine($"두 번째 10: 인덱스 {second}");  // 3

// 인덱스 4부터 검색
int third = Array.IndexOf(data, 10, 4);
Console.WriteLine($"세 번째 10: 인덱스 {third}");  // 5

// 출력:
// 첫 번째 10: 인덱스 1
// 두 번째 10: 인덱스 3
// 세 번째 10: 인덱스 5
```

**Array.LastIndexOf() - 마지막 위치 찾기:**

```csharp
int[] numbers = { 10, 20, 30, 20, 40, 20, 50 };

int firstIndex = Array.IndexOf(numbers, 20);
int lastIndex = Array.LastIndexOf(numbers, 20);

Console.WriteLine($"20의 첫 번째 위치: {firstIndex}");  // 1
Console.WriteLine($"20의 마지막 위치: {lastIndex}");    // 5

// 출력:
// 20의 첫 번째 위치: 1
// 20의 마지막 위치: 5
```

**실전 예제: 학생 검색:**

```csharp
string[] students = { "김철수", "이영희", "박민수", "최지혜", "정다은" };
int[] scores = { 85, 92, 78, 95, 88 };

Console.Write("검색할 학생 이름: ");
string searchName = Console.ReadLine();

int index = Array.IndexOf(students, searchName);

if (index != -1)
{
    Console.WriteLine($"{searchName}님의 점수: {scores[index]}점");
}
else
{
    Console.WriteLine($"{searchName}님을 찾을 수 없습니다.");
}

// 실행 예:
// 검색할 학생 이름: 최지혜
// 최지혜님의 점수: 95점
```

**기타 유용한 Array 메서드:**

```csharp
int[] numbers = { 5, 2, 8, 1, 9, 3 };

// Array.Clear() - 배열의 요소를 기본값으로 초기화
Array.Clear(numbers, 0, 3);  // 처음 3개 요소를 0으로
// numbers = { 0, 0, 0, 1, 9, 3 }

// Array.Copy() - 배열 복사
int[] copy = new int[6];
Array.Copy(numbers, copy, numbers.Length);

// Array.Exists() - 조건을 만족하는 요소가 있는지 확인
bool hasLargeNumber = Array.Exists(numbers, n => n > 5);
Console.WriteLine($"5보다 큰 수 존재: {hasLargeNumber}");  // True

// Array.Find() - 조건을 만족하는 첫 번째 요소 찾기
int firstOdd = Array.Find(numbers, n => n % 2 == 1);
Console.WriteLine($"첫 번째 홀수: {firstOdd}");  // 1

// Array.FindAll() - 조건을 만족하는 모든 요소 찾기
int[] evenNumbers = Array.FindAll(numbers, n => n % 2 == 0);
Console.WriteLine("짝수들:");
foreach (int num in evenNumbers)
{
    Console.Write($"{num} ");
}
```

---

## 마무리

이 장에서는 C# 프로그래밍에서 매우 중요한 자료구조인 배열에 대해 깊이 있게 학습했습니다. 배열은 1950년대 컴퓨터 과학의 여명기부터 존재해온 가장 근본적인 자료구조로, 같은 타입의 여러 데이터를 효율적으로 저장하고 관리할 수 있게 해주는 핵심 도구입니다. 메모리의 연속성, O(1) 접근 시간, 그리고 캐시 친화적인 특성은 배열을 오늘날까지도 가장 중요한 자료구조 중 하나로 만들어줍니다.

**학습 정리 - 배열의 핵심 개념:**

✅ **1차원 배열의 완전한 이해**: 배열의 선언과 다양한 초기화 방법, 0-기반 인덱싱을 통한 요소 접근, `for`와 `foreach`를 활용한 효율적인 순회 방법을 익혔습니다. 특히 배열이 메모리상에 연속적으로 배치되어 빠른 접근을 가능하게 한다는 점과, C#의 경계 검사가 메모리 안전성을 보장한다는 점을 이해했습니다.

✅ **다차원 배열의 구조적 특성**: 2차원 직사각형 배열 `[,]`과 가변 배열 `[][]`의 메모리 레이아웃 차이를 이해하고, 각각의 사용 사례와 성능 특성을 비교했습니다. 행 우선 순서(Row-Major Order)와 간접 참조(Indirection)가 성능에 미치는 영향을 학습했습니다.

✅ **배열 조작 메서드의 활용**: `Array.Sort()`의 인트로소트 알고리즘, `Array.Reverse()`의 제자리 역순 배치, `Array.IndexOf()`의 선형 검색 등 검증된 알고리즘을 활용하는 방법을 배웠습니다. 이러한 메서드들은 직접 구현하는 것보다 안정적이고 효율적입니다.

**핵심 포인트 - 실무에 적용할 원칙들:**

- **고정 크기의 제약**: 배열은 생성 후 크기 변경이 불가능한 자료구조입니다. 이는 메모리 효율성과 접근 속도를 위한 의도적 설계 결정으로, 동적 크기가 필요한 경우 `List<T>`를 사용해야 합니다.

- **0-기반 인덱싱의 보편성**: 첫 번째 요소는 인덱스 0, 마지막 요소는 인덱스 (Length-1)입니다. 유효 범위를 벗어나면 `IndexOutOfRangeException`이 발생하므로, 방어적 프로그래밍이 필요합니다.

- **순회 방법의 선택 전략**: `for` 문은 인덱스가 필요하거나 역순 순회가 필요할 때, `foreach` 문은 모든 요소를 순차적으로 읽기만 할 때 적합합니다. `foreach`는 더 안전하고 가독성이 좋지만, 요소 수정이나 인덱스 접근이 불가능합니다.

- **다차원 배열의 선택 기준**: 2차원 배열 `[,]`은 직사각형 구조에서 메모리 연속성과 캐시 효율이 우수하며, 가변 배열 `[][]`은 각 행의 길이가 다를 수 있어 메모리 효율적이고 유연합니다.

- **표준 라이브러리 활용**: `Array` 클래스의 정적 메서드들은 검증된 알고리즘을 제공하므로, 바퀴를 다시 발명하지 말고 적극 활용해야 합니다.

**컴퓨터 과학적 관점에서의 배열:**

배열은 단순해 보이지만, 컴퓨터 과학의 많은 핵심 개념을 포함하고 있습니다. 메모리 주소 계산, 포인터 산술, 캐시 지역성, 시간 복잡도 분석, 알고리즘 선택 등 배열을 통해 우리는 효율적인 프로그래밍의 기초를 배웁니다. 더 복잡한 자료구조인 트리, 그래프, 해시 테이블도 결국 배열을 기반으로 구현되는 경우가 많습니다.

**다음 단계 - 더 강력한 컬렉션으로:**

5장에서는 배열보다 더욱 유연하고 강력한 **컬렉션(Collection)**에 대해 학습하게 됩니다. `List<T>`는 동적 크기 조정을 지원하는 배열의 확장판이며, `Dictionary<TKey, TValue>`는 O(1) 시간에 키-값 쌍을 검색할 수 있는 해시 테이블 구현입니다. `HashSet<T>`, `Queue<T>`, `Stack<T>` 등 각각의 특성에 최적화된 다양한 컬렉션을 배우게 될 것입니다.

배열은 이러한 고급 컬렉션을 이해하는 데 필수적인 기초입니다. 많은 컬렉션이 내부적으로 배열을 사용하여 구현되므로, 배열의 특성을 정확히 이해하면 컬렉션의 성능 특성도 예측할 수 있습니다. 이번 장에서 배운 내용을 충분히 연습하여 완전히 자신의 것으로 만드세요. 단순히 문법을 암기하는 것이 아니라, 메모리 구조와 성능 특성을 이해하는 것이 진정한 프로그래머로 성장하는 길입니다!
