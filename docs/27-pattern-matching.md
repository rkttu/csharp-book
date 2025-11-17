# 27장. 패턴 매칭

패턴 매칭(Pattern Matching)은 C# 7.0부터 단계적으로 도입되어 C# 10과 C# 11에서 대폭 강화된 기능으로, 데이터의 구조와 값을 검사하고 분해하는 강력한 방법을 제공합니다. 이는 함수형 프로그래밍 언어에서 오랫동안 사용되어 온 핵심 기법을 객체지향 언어인 C#에 성공적으로 통합한 혁신적인 기능입니다. Anders Hejlsberg를 비롯한 C# 언어 설계팀은 Scala, F#, Haskell과 같은 함수형 언어의 장점을 C#의 타입 안전성 및 성능과 결합하여, 개발자가 복잡한 조건 로직을 더욱 선언적이고 안전하게 표현할 수 있도록 만들었습니다.

**패턴 매칭의 역사적 배경과 함수형 프로그래밍의 영향:**

패턴 매칭의 개념은 1970년대 Robin Milner가 개발한 ML(Meta Language)에서 시작되었으며, 이후 Haskell, OCaml, F#, Scala, Rust 등 수많은 현대 프로그래밍 언어의 핵심 기능으로 자리 잡았습니다. 전통적인 명령형(imperative) 프로그래밍에서는 `if-else` 연쇄나 `switch` 문을 사용하여 조건을 검사하지만, 패턴 매칭은 데이터의 "형태(shape)"를 선언적으로 기술하고 그에 맞는 처리를 매핑하는 더 고차원적인 추상화를 제공합니다.

C# 언어 설계팀은 .NET 생태계에서 F#의 성공을 목격하면서, 패턴 매칭이 복잡한 비즈니스 로직을 표현하는 데 매우 효과적임을 인식했습니다. 특히 도메인 주도 설계(Domain-Driven Design)와 함수형 프로그래밍의 불변성(immutability) 원칙이 결합될 때, 패턴 매칭은 상태 변환과 데이터 검증을 명확하고 안전하게 표현하는 강력한 도구가 됩니다. Microsoft는 2015년경부터 C#에 패턴 매칭 기능을 단계적으로 도입하기 시작했으며, 매 버전마다 기능을 확장하여 현재는 매우 포괄적인 패턴 매칭 시스템을 갖추게 되었습니다.

**패턴 매칭의 핵심 개념과 철학:**

패턴 매칭은 근본적으로 "값이 특정 패턴(pattern)과 일치(match)하는가?"라는 질문에 답하는 과정입니다. 여기서 "패턴"이란 데이터의 구조, 타입, 값에 대한 선언적 서술(declarative description)을 의미합니다. 전통적인 조건문이 "이 조건이 참인가?"라는 단순한 불린 질문을 하는 것과 달리, 패턴 매칭은 "이 데이터가 이러한 형태를 가지는가? 그렇다면 그 구성 요소는 무엇인가?"라는 더 복잡하고 구조적인 질문을 가능하게 합니다.

이러한 접근법은 다음과 같은 근본적인 장점을 제공합니다:

1. **구조 분해(Destructuring)와 검증의 결합**: 데이터의 형태를 검증하는 동시에 그 구성 요소를 추출할 수 있습니다. 이는 별도의 변수 선언이나 캐스팅 없이 한 번에 수행됩니다.

2. **타입 안전성(Type Safety) 강화**: 컴파일러가 모든 가능한 케이스를 다루는지 검증할 수 있으며, 런타임 오류 대신 컴파일 타임에 문제를 발견할 수 있습니다.

3. **불변성(Immutability) 친화적**: 데이터를 변경하는 대신 패턴에 따라 새로운 값을 생성하는 함수형 스타일을 자연스럽게 표현할 수 있습니다.

4. **표현력(Expressiveness) 향상**: 복잡한 조건 로직을 간결하고 읽기 쉬운 형태로 표현할 수 있어, 코드가 비즈니스 요구사항을 더 직접적으로 반영합니다.

5. **유지보수성(Maintainability) 개선**: 새로운 케이스를 추가하거나 기존 로직을 수정할 때, 관련 코드가 한 곳에 모여 있어 변경이 용이합니다.

**패턴 매칭의 실무 활용 시나리오:**

패턴 매칭은 다음과 같은 실무 상황에서 특히 강력합니다:

- **다형성(Polymorphism) 처리**: 상속 계층구조에서 각 타입별로 다른 동작을 수행할 때, 타입 패턴을 사용하여 명확하고 안전하게 분기할 수 있습니다.

- **상태 머신(State Machine) 구현**: 객체의 상태에 따라 다른 동작을 수행하는 경우, 속성 패턴을 사용하여 상태 전환 로직을 선언적으로 표현할 수 있습니다.

- **데이터 검증(Validation)**: 입력 데이터가 특정 조건을 만족하는지 검사하고, 그에 따라 적절한 처리를 수행할 때 관계 패턴과 논리 패턴이 유용합니다.

- **메시지 처리(Message Handling)**: 이벤트 기반 시스템이나 액터 모델에서 다양한 메시지 타입을 처리할 때, 패턴 매칭을 통해 각 메시지에 대한 핸들러를 명확하게 정의할 수 있습니다.

- **파서(Parser) 및 인터프리터(Interpreter) 구현**: 추상 구문 트리(AST)를 순회하며 각 노드 타입에 따라 다른 처리를 수행하는 컴파일러나 인터프리터 개발에 필수적입니다.

**패턴 매칭이 사용되는 주요 위치:**

C#에서 패턴 매칭은 다음과 같은 언어 구성 요소에서 활용됩니다:

- **`is` 표현식(is expression)**: 값이 특정 패턴과 일치하는지 검사하고, 일치하는 경우 해당 값을 추출합니다. 조건문의 일부로 사용되어 타입 안전한 분기를 가능하게 합니다.

- **`switch` 문(switch statement)**: 전통적인 값 기반 분기에서 벗어나, 패턴 기반 분기를 지원합니다. C# 7.0부터 `case` 절에 패턴을 사용할 수 있게 되었습니다.

- **`switch` 식(switch expression)**: C# 8.0에서 도입된 표현식 형태의 switch로, 더욱 간결하고 함수형 스타일에 가까운 패턴 매칭을 제공합니다. 모든 가능한 경우를 다루어야 하는 완전성(exhaustiveness) 검사를 지원합니다.

- **변수 선언(variable declaration)**: `var` 키워드와 함께 사용하여 복잡한 데이터 구조를 분해(destructure)할 수 있습니다.

**C#의 패턴 매칭 진화 - 단계별 발전사:**

C#의 패턴 매칭은 한 번에 완성된 것이 아니라, 여러 버전에 걸쳐 점진적으로 발전해왔습니다. 각 버전마다 새로운 패턴 형태와 표현력이 추가되었으며, 이는 개발자 커뮤니티의 피드백과 함수형 프로그래밍 언어의 모범 사례를 반영한 결과입니다.

- **C# 7.0 (2017년 3월)**: 패턴 매칭의 시작
  - `is` 표현식에서 타입 패턴 지원: `if (obj is string s)`
  - `switch` 문의 `case` 절에서 패턴 사용 가능
  - `when` 절을 통한 추가 조건(가드) 지원
  - 이 버전은 Java의 `instanceof` 패턴 매칭 제안보다 먼저 출시되어, C#이 패턴 매칭 분야에서 선구적 역할을 했음을 보여줍니다.

- **C# 8.0 (2019년 9월)**: 표현식 중심의 패턴 매칭
  - **Switch 식(Switch Expression)**: 완전히 새로운 문법으로 `=>` 기호를 사용한 간결한 패턴 매칭
  - **속성 패턴(Property Pattern)**: 객체의 속성 값을 기반으로 매칭
  - **튜플 패턴(Tuple Pattern)**: 여러 값을 동시에 매칭
  - **위치 패턴(Positional Pattern)**: `Deconstruct` 메서드를 활용한 구조 분해
  - 이 버전에서 C#의 패턴 매칭은 F#과 같은 성숙한 함수형 언어에 필적하는 수준으로 발전했습니다.

- **C# 9.0 (2020년 11월)**: 논리적 조합과 관계 비교
  - **관계 패턴(Relational Pattern)**: `<`, `<=`, `>`, `>=` 연산자를 패턴에서 직접 사용
  - **논리 패턴(Logical Pattern)**: `and`, `or`, `not` 키워드로 패턴 조합
  - **단순 타입 패턴**: 변수 선언 없이 타입만 체크
  - 이러한 기능은 범위 검사와 복잡한 조건 결합을 매우 직관적으로 표현할 수 있게 했습니다.

- **C# 10 (2021년 11월)**: 속성 패턴의 확장
  - **확장 속성 패턴(Extended Property Pattern)**: 중첩된 속성을 점(.) 표기법으로 간결하게 접근
  - 예: `{ Address.City: "Seoul" }` 대신 중첩된 중괄호 불필요
  - 이는 복잡한 도메인 모델을 다룰 때 코드의 가독성을 크게 향상시켰습니다.

- **C# 11 (2022년 11월)**: 컬렉션 패턴
  - **리스트 패턴(List Pattern)**: 배열이나 리스트의 요소를 패턴으로 매칭
  - **슬라이스 패턴(Slice Pattern)**: `..` 연산자로 가변 길이 시퀀스 매칭
  - 예: `[1, 2, .. var rest]`는 처음 두 요소를 매칭하고 나머지를 `rest`에 할당
  - 이는 재귀 알고리즘이나 시퀀스 처리에서 매우 강력한 도구가 되었습니다.

**패턴 매칭의 성능 특성과 컴파일러 최적화:**

패턴 매칭은 단순한 문법적 편의(syntactic sugar)가 아니라, Roslyn 컴파일러의 정교한 최적화를 통해 효율적인 IL(Intermediate Language) 코드로 변환됩니다. 컴파일러는 다음과 같은 최적화를 수행합니다:

1. **타입 검사 캐싱**: 동일한 객체에 대한 반복적인 타입 검사를 최소화하기 위해, 한 번 수행한 타입 검사 결과를 재사용합니다.

2. **결정 트리 생성**: 여러 패턴을 효율적으로 평가하기 위해 최적화된 결정 트리(decision tree)를 생성합니다. 이는 단순한 선형 검사보다 훨씬 빠릅니다.

3. **점프 테이블 최적화**: 상수 값 기반 패턴의 경우, 전통적인 `switch` 문과 동일한 점프 테이블을 생성하여 O(1) 성능을 보장합니다.

4. **단락 평가(Short-circuit Evaluation)**: 논리 패턴에서 첫 번째 조건만으로 결과를 알 수 있는 경우, 나머지 평가를 건너뜁니다.

5. **인라인 확장**: 간단한 패턴 매칭은 메서드 호출 없이 인라인으로 확장되어 오버헤드가 거의 없습니다.

이러한 최적화 덕분에, 패턴 매칭은 전통적인 `if-else` 체인이나 `switch` 문과 비교하여 성능 저하 없이, 오히려 때로는 더 나은 성능을 보이면서도 훨씬 더 표현력 있는 코드를 작성할 수 있게 합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 C#의 패턴 매칭 기능을 체계적이고 심도 있게 학습하게 됩니다:

- **타입 패턴(Type Pattern)의 마스터**: 객체의 런타임 타입을 검사하고 안전하게 캐스팅하는 방법을 배웁니다. 전통적인 `is` 연산자와 명시적 캐스팅의 한계를 이해하고, 타입 패턴이 어떻게 타입 안전성과 코드 간결성을 동시에 달성하는지 학습합니다. 다형성 처리, `switch` 식에서의 활용, `when` 절을 통한 가드 조건 등 실무에서 즉시 적용할 수 있는 패턴들을 익힙니다.

- **속성 패턴(Property Pattern)의 활용**: 객체의 속성 값을 기반으로 복잡한 조건을 선언적으로 표현하는 방법을 학습합니다. 중첩된 객체 그래프를 탐색하는 기존 방식의 불편함을 해소하고, C# 10의 확장 속성 패턴을 활용하여 깊이 중첩된 속성에 간결하게 접근하는 방법을 배웁니다. 상태 머신 구현, DTO 검증, 도메인 로직 표현 등 실전 시나리오를 통해 속성 패턴의 강력함을 체험합니다.

- **위치 패턴(Positional Pattern)의 이해**: 튜플(Tuple), 레코드(Record), 그리고 `Deconstruct` 메서드를 제공하는 사용자 정의 타입을 위치 기반으로 분해하고 매칭하는 방법을 익힙니다. 함수형 프로그래밍의 구조 분해(destructuring) 개념이 C#에 어떻게 적용되었는지 이해하고, 복잡한 데이터 구조를 다룰 때 코드의 가독성을 극대화하는 기법을 습득합니다.

- **관계 패턴(Relational Pattern)의 적용**: C# 9.0에서 도입된 관계 연산자(`<`, `<=`, `>`, `>=`)를 패턴 내에서 직접 사용하여 숫자 범위, 경계값 검사, 크기 비교를 직관적으로 표현하는 방법을 배웁니다. 전통적인 범위 검사 코드와 비교하여 관계 패턴이 어떻게 의도를 더 명확하게 전달하는지, 그리고 비즈니스 로직을 더 선언적으로 표현하는지 학습합니다.

- **논리 패턴(Logical Pattern)의 조합**: `and`, `or`, `not` 키워드를 사용하여 여러 패턴을 논리적으로 결합하고, 불 대수(Boolean Algebra)의 원리를 패턴 매칭에 적용하는 방법을 학습합니다. 복잡한 비즈니스 규칙을 표현할 때 논리 패턴이 어떻게 코드의 표현력을 극대화하는지, 그리고 드모르간의 법칙과 같은 논리학 원리가 실제 코드에 어떻게 적용되는지 이해합니다.

**학습 목표:**

이 장을 마치면 여러분은 다음을 할 수 있게 됩니다:

1. 복잡한 타입 계층구조를 다룰 때 패턴 매칭을 활용하여 안전하고 유지보수 가능한 코드 작성
2. 상태 기반 로직을 명확하고 간결하게 표현하는 속성 패턴 활용
3. 함수형 프로그래밍 스타일의 구조 분해를 C#에 적용
4. 복잡한 비즈니스 규칙을 선언적이고 읽기 쉬운 패턴으로 표현
5. 전통적인 `if-else` 체인과 `switch` 문을 현대적인 패턴 매칭으로 리팩토링
6. 컴파일러의 타입 안전성 검사를 활용하여 런타임 오류 방지

**사전 지식:**

이 장을 효과적으로 학습하기 위해서는 다음 개념에 대한 이해가 필요합니다:
- C#의 기본 타입 시스템과 상속
- 인터페이스와 다형성
- 제네릭(Generics)의 기초
- 람다 식과 LINQ의 기본 개념
- 레코드 타입과 튜플 (11장 내용)

---

## 27.1 타입 패턴

타입 패턴(Type Pattern)은 C# 7.0에서 처음 도입된 패턴 매칭의 가장 기본적이면서도 강력한 형태입니다. 객체의 런타임 타입을 검사하고, 해당 타입으로 안전하게 캐스팅하며, 동시에 새로운 변수를 선언하는 세 가지 작업을 하나의 간결한 표현식으로 수행할 수 있습니다. 이는 전통적인 타입 검사와 캐스팅 패턴의 근본적인 문제점들을 해결하며, 다형성(Polymorphism)을 다루는 코드의 안전성과 가독성을 크게 향상시킵니다.

**전통적인 타입 검사와 캐스팅의 역사적 문제점:**

객체지향 프로그래밍에서 다형성은 핵심 개념이지만, 이를 실제 코드로 구현할 때는 항상 타입 검사와 캐스팅이라는 번거로운 작업이 수반되었습니다. C# 1.0부터 C# 6.0까지, 개발자들은 다음과 같은 패턴을 반복적으로 작성해야 했습니다:

```csharp
// C# 6 이전: 타입 검사와 캐스팅 분리
object obj = "Hello, World!";

if (obj is string)  // 1단계: 타입 검사
{
    string text = (string)obj;  // 2단계: 명시적 캐스팅
    Console.WriteLine($"문자열 길이: {text.Length}");  // 3단계: 사용
}
```

이 전통적인 방식은 다음과 같은 심각한 문제점을 가지고 있습니다:

1. **코드 중복(Code Duplication)**: `obj`라는 변수와 `string`이라는 타입이 두 번씩 언급됩니다. DRY(Don't Repeat Yourself) 원칙에 위배되며, 변수명이나 타입을 변경할 때 여러 곳을 수정해야 합니다.

2. **타입 안전성 약화**: 타입 검사와 캐스팅 사이에 다른 코드가 삽입되거나, 실수로 다른 타입으로 캐스팅하는 버그가 발생할 수 있습니다. 컴파일러는 이러한 논리적 오류를 감지할 수 없습니다.

3. **가독성 저하**: 실제 비즈니스 로직(문자열 길이 출력)보다 타입 처리 코드가 더 많은 공간을 차지하여, 코드의 의도가 불명확해집니다.

4. **성능 오버헤드**: 타입 검사(`is`)와 캐스팅은 모두 런타임에 타입 메타데이터를 조회하는 비용이 있으며, 이를 두 번 수행하는 것은 비효율적입니다.

5. **null 안전성 부재**: `obj`가 `null`인 경우 `is` 검사는 실패하지만, 명시적 캐스팅 코드에서는 이를 명확히 처리하지 않아 혼란을 야기할 수 있습니다.

**타입 패턴의 혁신적 해결책:**

C# 7.0의 타입 패턴은 위의 모든 문제를 한 번에 해결합니다:

```csharp
// C# 7.0+: 타입 패턴 사용
object obj = "Hello, World!";

if (obj is string text)  // 타입 검사 + 캐스팅 + 변수 선언을 한 번에!
{
    Console.WriteLine($"문자열 길이: {text.Length}");
    // text는 이미 string 타입으로 안전하게 캐스팅됨
}
// text는 이 블록 밖에서는 사용 불가 (스코프 제한)
```

이 간결한 한 줄의 코드는 다음을 모두 수행합니다:

1. `obj`의 런타임 타입이 `string`인지 검사
2. 타입 검사가 성공하면 `obj`를 `string` 타입으로 안전하게 캐스팅
3. 캐스팅된 값을 새로운 변수 `text`에 할당
4. `text`의 스코프를 `if` 블록 내부로 제한하여 의도하지 않은 사용 방지

**타입 패턴의 핵심 특징과 의미론(Semantics):**

타입 패턴은 단순한 문법적 편의를 넘어서, 명확한 의미론적 규칙을 따릅니다:

- **타입 검사의 정확성**: 타입 패턴은 `is` 연산자와 동일한 타입 검사를 수행합니다. 이는 단순한 타입 이름 비교가 아니라, .NET의 타입 시스템을 완전히 존중합니다. 예를 들어, 박싱(boxing)된 값 타입, 인터페이스 구현, 상속 관계 등이 모두 올바르게 처리됩니다.

- **Null 안전성**: `obj`가 `null`인 경우, 타입 패턴은 절대 매칭되지 않습니다. `null`은 어떤 타입으로도 캐스팅될 수 없기 때문입니다. 이는 `NullReferenceException`을 방지하는 안전장치가 됩니다.

- **변수 스코프의 정확한 제어**: 패턴 변수(`text`)는 패턴 매칭이 성공한 스코프 내에서만 확정 할당(definitely assigned)됩니다. 컴파일러는 이를 정적으로 분석하여, 패턴이 실패할 수 있는 경로에서 변수를 사용하려고 하면 컴파일 오류를 발생시킵니다.

```csharp
object obj = GetValue();

if (obj is string text)
{
    Console.WriteLine(text.Length);  // ✅ 안전함 - text는 확정 할당됨
}
else
{
    // Console.WriteLine(text.Length);  // ❌ 컴파일 오류 - text가 이 스코프에 없음
}

// Console.WriteLine(text.Length);  // ❌ 컴파일 오류 - text가 이 스코프에 없음
```

- **타입 추론과의 통합**: 패턴 변수의 타입은 패턴에 명시된 타입으로 자동 추론됩니다. `var`를 사용할 필요가 없으며, 항상 명시적으로 타입이 지정됩니다.

- **성능 최적화**: Roslyn 컴파일러는 타입 패턴을 매우 효율적인 IL 코드로 변환합니다. 단일 타입 검사로 캐스팅까지 완료되며, 전통적인 방식보다 빠르거나 동등한 성능을 보입니다.

**switch 문에서의 타입 패턴 - 다형성의 우아한 처리:**

C# 7.0 이전에는 `switch` 문이 정수, 문자열, 열거형 같은 제한된 타입만 지원했지만, 타입 패턴의 도입으로 모든 참조 타입과 값 타입을 다룰 수 있게 되었습니다. 이는 다형성 처리를 위한 전통적인 가상 메서드(virtual method) 패턴의 강력한 대안을 제공합니다.

```csharp
object data = 42;

switch (data)
{
    case int number:
        Console.WriteLine($"정수: {number}");
        Console.WriteLine($"제곱: {number * number}");
        break;
    
    case string text:
        Console.WriteLine($"문자열: {text}");
        Console.WriteLine($"길이: {text.Length}");
        break;
    
    case bool flag:
        Console.WriteLine($"불린: {flag}");
        Console.WriteLine($"반대값: {!flag}");
        break;
    
    case double d:
        Console.WriteLine($"실수: {d}");
        Console.WriteLine($"정수부: {Math.Floor(d)}");
        break;
    
    case null:
        Console.WriteLine("null 값");
        break;
    
    default:
        Console.WriteLine($"알 수 없는 타입: {data.GetType().Name}");
        break;
}
// data가 42(int)이므로 출력: "정수: 42" 및 "제곱: 1764"
```

이 패턴은 비지터 패턴(Visitor Pattern)이나 전략 패턴(Strategy Pattern)과 같은 복잡한 디자인 패턴을 사용하지 않고도, 타입별로 다른 동작을 명확하게 표현할 수 있게 합니다.

**Switch 식에서의 타입 패턴 (C# 8.0+) - 함수형 스타일의 완성:**

C# 8.0에서 도입된 switch 식(switch expression)은 패턴 매칭을 완전히 새로운 차원으로 끌어올렸습니다. 이는 문(statement)이 아닌 식(expression)이므로, 값을 반환하며 더욱 간결하고 함수형 프로그래밍 스타일에 가깝습니다.

```csharp
object data = "Hello";

string result = data switch
{
    int number => $"정수: {number}",
    string text => $"문자열: {text} (길이: {text.Length})",
    bool flag => $"불린: {flag}",
    double d => $"실수: {d:F2}",
    null => "null 값",
    _ => $"알 수 없는 타입: {data.GetType().Name}"  // 와일드카드 패턴
};

Console.WriteLine(result);  // 출력: "문자열: Hello (길이: 5)"
```

Switch 식의 주요 특징:

1. **표현식(Expression) 기반**: 값을 직접 반환하므로 변수 할당이나 메서드 인자로 사용 가능
2. **간결한 문법**: `case`, `break` 키워드 없이 `=>` 기호로 간결하게 표현
3. **완전성 검사(Exhaustiveness Checking)**: 컴파일러가 모든 가능한 케이스를 다루는지 검증
4. **불변성 친화적**: 값을 변경하지 않고 새로운 값을 생성하는 함수형 스타일 촉진

**when 절을 사용한 가드(Guard) 조건 - 조건의 세밀한 제어:**

타입 패턴만으로는 부족한 경우, `when` 절을 추가하여 더 세밀한 조건을 지정할 수 있습니다. 이는 Haskell의 가드(guards) 개념에서 영감을 받은 기능으로, 패턴 매칭의 표현력을 크게 향상시킵니다.

```csharp
object obj = 42;

string result = obj switch
{
    int n when n > 0 => "양수",
    int n when n < 0 => "음수",
    int => "0",  // when 절 없는 패턴은 항상 매칭
    string s when s.Length > 0 => "비어있지 않은 문자열",
    string => "빈 문자열",
    double d when d > 100 => "큰 실수",
    double d when d > 0 => "작은 실수",
    _ => "기타"
};

Console.WriteLine(result);  // 출력: 양수
```

**`when` 절의 평가 순서와 의미론:**

1. 먼저 타입 패턴이 매칭되는지 확인
2. 타입이 일치하면 `when` 절의 조건을 평가
3. 타입과 조건이 모두 만족하면 해당 분기 실행
4. 조건이 맞지 않으면 다음 패턴으로 이동

이는 단락 평가(short-circuit evaluation)를 보장하여 불필요한 계산을 방지합니다.

**복잡한 가드 조건의 실전 예제:**

```csharp
public record Transaction(decimal Amount, DateTime Date, string Category);

string ClassifyTransaction(Transaction transaction) => transaction switch
{
    { Amount: > 1000000 } when transaction.Category == "Investment" 
        => "대형 투자",
    
    { Amount: > 1000000 } when transaction.Category == "Purchase" 
        => "고액 구매 - 승인 필요",
    
    { Amount: < 0 } 
        => "환불 또는 취소",
    
    { Category: "Food" } when transaction.Amount < 10000 && transaction.Date.DayOfWeek != DayOfWeek.Sunday 
        => "평일 소액 식비",
    
    { Category: "Transport" } when transaction.Date.Hour >= 7 && transaction.Date.Hour <= 9 
        => "출근 시간 교통비",
    
    _ => "일반 거래"
};

var tx1 = new Transaction(1500000, DateTime.Now, "Investment");
var tx2 = new Transaction(5000, new DateTime(2024, 1, 15, 8, 30, 0), "Transport");

Console.WriteLine(ClassifyTransaction(tx1));  // "대형 투자"
Console.WriteLine(ClassifyTransaction(tx2));  // "출근 시간 교통비"
```

**var 패턴 - 모든 값을 캡처하는 범용 패턴:**

`var` 패턴은 타입에 관계없이 모든 값을 캡처합니다. 주로 타입 정보를 유지하면서 값을 변수에 할당할 때 사용됩니다.

```csharp
object obj = "anything";

if (obj is var value)  // 항상 true, 타입 관계없이 value에 할당
{
    Console.WriteLine($"값: {value}, 타입: {value.GetType()}");
}
// 출력: 값: anything, 타입: System.String

// 실용적인 사용 예: 중간 계산 결과 저장
int CalculateComplex(int x) => x switch
{
    var n when n < 0 => -1,
    var n when n == 0 => 0,
    var n => n * n + 2 * n + 1  // (n+1)²
};
```

**`var` 패턴 vs `_` (discard) 패턴:**

- **`var` 패턴**: 값을 변수에 할당하여 사용 가능
- **`_` 패턴**: 값을 무시하고 타입 확인만 수행

```csharp
object obj = 123;

// var 패턴 - 값을 사용
if (obj is var value)
{
    Console.WriteLine($"값 사용 가능: {value}");
}

// discard 패턴 - 타입만 확인
if (obj is int _)  // int 타입인지만 확인, 값은 사용 안 함
{
    Console.WriteLine("정수입니다.");  // 값에 접근하지 않음
}
```

**const 패턴 - 특정 값과의 정확한 매칭:**

타입 패턴의 특수한 경우로, 특정 상수 값과 정확히 일치하는지 검사합니다.

```csharp
object value = 42;

bool isAnswer = value is 42;  // true
bool isNull = value is null;  // false

// switch 식에서 활용
string Classify(object obj) => obj switch
{
    null => "null 값",
    0 => "zero",
    1 => "one",
    "" => "빈 문자열",
    "hello" => "인사",
    true => "참",
    false => "거짓",
    _ => "기타"
};
```

**부정(Negation) 패턴 (C# 9.0+) - not 패턴:**

C# 9.0부터는 `not` 키워드를 사용하여 패턴을 부정할 수 있습니다.

```csharp
object obj = "Hello";

if (obj is not null)  // null이 아닌 경우
{
    Console.WriteLine($"값이 있습니다: {obj}");
}

// switch 식에서 활용
string Describe(object value) => value switch
{
    not null => $"값이 있음: {value}",
    null => "값이 없음"
};

// 타입 부정
if (obj is not int)  // int가 아닌 경우
{
    Console.WriteLine("정수가 아닙니다.");
}
```

**실전 예제: 다형성 처리 - 도형 넓이 계산의 진화:**

타입 패턴의 가장 강력한 활용 사례 중 하나는 상속 계층구조에서의 다형성 처리입니다. 전통적인 가상 메서드 패턴과 비교하여 타입 패턴이 어떻게 더 유연하고 안전한 대안을 제공하는지 살펴보겠습니다.

```csharp
// 도형 클래스 계층
public abstract class Shape 
{ 
    public string Color { get; set; }
}

public class Circle : Shape 
{ 
    public double Radius { get; set; }
    
    public Circle(double radius, string color = "Black")
    {
        Radius = radius;
        Color = color;
    }
}

public class Rectangle : Shape 
{ 
    public double Width { get; set; }
    public double Height { get; set; }
    
    public Rectangle(double width, double height, string color = "Black")
    {
        Width = width;
        Height = height;
        Color = color;
    }
}

public class Triangle : Shape 
{ 
    public double Base { get; set; }
    public double Height { get; set; }
    
    public Triangle(double @base, double height, string color = "Black")
    {
        Base = @base;
        Height = height;
        Color = color;
    }
}

public class Polygon : Shape
{
    public int Sides { get; set; }
    public double SideLength { get; set; }
}

// ❌ 전통적인 방식 - 문제점이 많은 코드
double GetAreaOld(Shape shape)
{
    // 타입별로 반복적인 검사와 캐스팅
    if (shape is Circle)
    {
        Circle circle = (Circle)shape;  // 불필요한 캐스팅
        return Math.PI * circle.Radius * circle.Radius;
    }
    else if (shape is Rectangle)
    {
        Rectangle rect = (Rectangle)shape;
        return rect.Width * rect.Height;
    }
    else if (shape is Triangle)
    {
        Triangle tri = (Triangle)shape;
        return tri.Base * tri.Height / 2;
    }
    
    // 새로운 타입 추가 시 여기를 수정해야 함
    // Polygon을 빠뜨리기 쉬움 - 컴파일러가 경고하지 않음
    return 0;
}

// ✅ 타입 패턴 사용 - 간결하고 안전한 코드
double GetArea(Shape shape) => shape switch
{
    Circle c => Math.PI * c.Radius * c.Radius,
    Rectangle r => r.Width * r.Height,
    Triangle t => t.Base * t.Height / 2,
    Polygon p when p.Sides == 6 => 2.598 * p.SideLength * p.SideLength,  // 정육각형
    Polygon => throw new NotImplementedException("일반 다각형은 지원하지 않음"),
    null => throw new ArgumentNullException(nameof(shape)),
    _ => throw new ArgumentException($"알 수 없는 도형: {shape.GetType().Name}")
    // ⚠️ 컴파일러가 모든 케이스를 다루도록 강제하지는 않지만,
    // _ 패턴으로 예외를 던지면 런타임에 누락된 케이스를 발견할 수 있음
};

// 📊 더 복잡한 예: 도형 둘레 계산
double GetPerimeter(Shape shape) => shape switch
{
    Circle { Radius: var r } => 2 * Math.PI * r,
    Rectangle { Width: var w, Height: var h } => 2 * (w + h),
    Triangle t => t.Base + t.Height + Math.Sqrt(t.Base * t.Base + t.Height * t.Height),
    Polygon { Sides: var n, SideLength: var s } => n * s,
    _ => 0
};

// 🎨 타입과 속성을 함께 활용하는 예
string GetShapeDescription(Shape shape) => shape switch
{
    Circle { Radius: > 10, Color: "Red" } c 
        => $"큰 빨간 원 (반지름: {c.Radius})",
    
    Circle { Radius: <= 10 } c 
        => $"작은 원 (반지름: {c.Radius}, 색상: {c.Color})",
    
    Rectangle { Width: var w, Height: var h } when w == h 
        => $"정사각형 (한 변: {w}, 색상: {shape.Color})",
    
    Rectangle { Width: > 100 or Height: > 100 } 
        => "대형 직사각형",
    
    Triangle { Base: var b, Height: var h } when b == h 
        => "이등변삼각형",
    
    _ => $"{shape.GetType().Name} (색상: {shape.Color})"
};

// 사용 예제
Shape[] shapes = 
{
    new Circle(5, "Blue"),
    new Circle(15, "Red"),
    new Rectangle(10, 10, "Green"),
    new Rectangle(200, 50),
    new Triangle(10, 10, "Yellow")
};

Console.WriteLine("=== 도형 정보 ===");
foreach (var shape in shapes)
{
    Console.WriteLine($"{GetShapeDescription(shape)}");
    Console.WriteLine($"  넓이: {GetArea(shape):F2}");
    Console.WriteLine($"  둘레: {GetPerimeter(shape):F2}");
    Console.WriteLine();
}

// 출력:
// 작은 원 (반지름: 5, 색상: Blue)
//   넓이: 78.54
//   둘레: 31.42
//
// 큰 빨간 원 (반지름: 15)
//   넓이: 706.86
//   둘레: 94.25
//
// 정사각형 (한 변: 10, 색상: Green)
//   넓이: 100.00
//   둘레: 40.00
// ...
```

**전통적 가상 메서드 vs 패턴 매칭 - 장단점 비교:**

| 측면 | 가상 메서드 (Virtual Methods) | 패턴 매칭 (Pattern Matching) |
|------|------------------------------|------------------------------|
| **확장 방향** | 새로운 타입 추가 용이 | 새로운 연산 추가 용이 |
| **코드 위치** | 각 클래스에 분산 | 하나의 함수에 집중 |
| **타입 안전성** | 컴파일 타임 검증 | switch 식에서 부분 검증 가능 |
| **종속성** | 클래스가 연산에 의존 | 연산이 클래스에 의존 |
| **테스트** | 클래스마다 개별 테스트 | 함수 단위 테스트 |
| **유연성** | 런타임 다형성 | 컴파일 타임 분기 |

이는 소프트웨어 공학에서 유명한 **Expression Problem**의 전형적인 예입니다. 패턴 매칭은 연산 추가가 빈번한 경우에 특히 유용합니다.

**비지터 패턴(Visitor Pattern)과의 비교:**

전통적으로 Expression Problem을 해결하기 위해 비지터 패턴이 사용되었지만, 패턴 매칭은 더 간결한 대안을 제공합니다:

```csharp
// ❌ 전통적 비지터 패턴 - 보일러플레이트 코드 많음
interface IShapeVisitor<T>
{
    T Visit(Circle circle);
    T Visit(Rectangle rectangle);
    T Visit(Triangle triangle);
}

// Shape 클래스에 Accept 메서드 추가 필요
abstract class Shape
{
    public abstract T Accept<T>(IShapeVisitor<T> visitor);
}

class AreaCalculator : IShapeVisitor<double>
{
    public double Visit(Circle c) => Math.PI * c.Radius * c.Radius;
    public double Visit(Rectangle r) => r.Width * r.Height;
    public double Visit(Triangle t) => t.Base * t.Height / 2;
}

// ✅ 패턴 매칭 - 훨씬 간결하고 직관적
double GetArea(Shape shape) => shape switch
{
    Circle c => Math.PI * c.Radius * c.Radius,
    Rectangle r => r.Width * r.Height,
    Triangle t => t.Base * t.Height / 2,
    _ => 0
};
```

패턴 매칭은 비지터 패턴의 장점(연산 추가 용이성)을 유지하면서, 불필요한 인터페이스와 Accept 메서드 없이 더 간결하게 구현할 수 있습니다.

---

## 27.2 속성 패턴

속성 패턴(Property Pattern)은 C# 8.0에서 도입된 기능으로, 객체의 속성 값을 기반으로 패턴 매칭을 수행합니다. 이는 객체 지향 프로그래밍에서 매우 일반적인 "객체의 상태에 따른 분기" 패턴을 선언적이고 타입 안전하게 표현할 수 있게 해줍니다. 전통적인 다중 `if` 문이나 속성 접근 체인과 비교하여, 속성 패턴은 코드의 의도를 훨씬 명확하게 드러내며, null 안전성과 가독성을 크게 향상시킵니다.

**속성 패턴의 역사적 배경과 설계 동기:**

객체 지향 프로그래밍에서 객체의 상태는 속성(Properties)을 통해 표현됩니다. 비즈니스 로직은 종종 이러한 속성 값의 조합에 따라 다르게 동작해야 하며, 이는 코드의 상당 부분을 차지합니다. 전통적으로 이러한 조건 검사는 다음과 같이 작성되었습니다:

```csharp
// 전통적인 방식 - 장황하고 오류가 발생하기 쉬움
if (person != null && person.Age >= 18 && person.Age < 65 && person.IsStudent == true)
{
    // 성인 학생 처리
}
else if (person != null && person.Age < 18)
{
    // 미성년자 처리
}
// ... 더 많은 조건들
```

이 방식의 문제점은 명백합니다:
1. `person != null` 검사가 반복됨
2. 조건이 복잡해질수록 가독성 급격히 저하
3. 속성 이름과 값이 떨어져 있어 관계 파악이 어려움
4. 오타나 논리 오류 발생 가능성 높음

C# 언어 설계팀은 이러한 문제를 해결하기 위해 속성 패턴을 도입했습니다. 이는 Scala의 케이스 클래스(case class) 매칭과 F#의 레코드 패턴에서 영감을 받았으며, 객체의 "형태(shape)"를 선언적으로 기술하는 방법을 제공합니다.

**기본 속성 패턴 - 단일 속성 검사:**

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string City { get; set; }
    public bool IsStudent { get; set; }
}

Person person = new Person { Name = "김철수", Age = 30, City = "서울", IsStudent = false };

// 단일 속성 검사
if (person is { Age: 30 })
{
    Console.WriteLine("30세입니다.");
}

// 여러 속성 동시 검사 - AND 조건으로 결합됨
if (person is { Age: 30, City: "서울" })
{
    Console.WriteLine("30세이며 서울에 거주합니다.");
}

// 속성 값을 변수로 캡처
if (person is { Age: var age, Name: var name })
{
    Console.WriteLine($"{name}님은 {age}세입니다.");
}
```

**속성 패턴의 의미론(Semantics):**

속성 패턴은 다음과 같은 명확한 규칙을 따릅니다:

1. **null 안전성**: 객체가 `null`이면 속성 패턴은 절대 매칭되지 않습니다. 명시적인 null 검사가 필요 없습니다.

2. **AND 결합**: 여러 속성 조건은 논리 AND로 결합됩니다. 모든 조건이 만족해야 매칭됩니다.

3. **중첩 가능**: 속성의 값이 객체인 경우, 재귀적으로 속성 패턴을 적용할 수 있습니다.

4. **순서 무관**: 속성 지정 순서는 매칭 결과에 영향을 주지 않습니다.

**Switch 식에서의 속성 패턴 - 상태 기반 로직의 우아한 표현:**

속성 패턴은 switch 식과 결합될 때 그 진가를 발휘합니다. 복잡한 비즈니스 규칙을 선언적이고 읽기 쉬운 형태로 표현할 수 있습니다.

```csharp
string GetDiscount(Person person) => person switch
{
    // 나이와 학생 여부를 함께 고려
    { Age: < 18, IsStudent: true } => "청소년 학생 할인 30%",
    { Age: < 18 } => "청소년 할인 20%",
    { Age: >= 65 } => "경로 할인 30%",
    { IsStudent: true, City: "서울" } => "서울 학생 할인 15%",
    { IsStudent: true } => "학생 할인 10%",
    { City: "서울" } => "서울 시민 할인 10%",
    _ => "일반 가격"
};

// 테스트
var people = new[]
{
    new Person { Name = "학생", Age = 15, IsStudent = true, City = "부산" },
    new Person { Name = "어르신", Age = 70, IsStudent = false, City = "대전" },
    new Person { Name = "시민", Age = 35, IsStudent = false, City = "서울" },
    new Person { Name = "대학생", Age = 22, IsStudent = true, City = "서울" }
};

foreach (var p in people)
{
    Console.WriteLine($"{p.Name}: {GetDiscount(p)}");
}

// 출력:
// 학생: 청소년 학생 할인 30%
// 어르신: 경로 할인 30%
// 시민: 서울 시민 할인 10%
// 대학생: 서울 학생 할인 15%
```

**패턴 순서의 중요성 - First-Match Semantics:**

Switch 식에서 패턴은 위에서 아래로 평가되며, 첫 번째로 매칭되는 패턴이 선택됩니다(First-Match Semantics). 따라서 더 구체적인 패턴을 먼저 배치해야 합니다:

```csharp
// ✅ 올바른 순서 - 구체적인 것부터
string Classify(Person p) => p switch
{
    { Age: < 18, IsStudent: true } => "청소년 학생",  // 가장 구체적
    { Age: < 18 } => "청소년",                        // 덜 구체적
    { IsStudent: true } => "학생",                    // 일반적
    _ => "일반인"
};

// ❌ 잘못된 순서 - 일반적인 패턴이 먼저
string ClassifyWrong(Person p) => p switch
{
    { Age: < 18 } => "청소년",                        // 이것이 먼저 매칭됨
    { Age: < 18, IsStudent: true } => "청소년 학생",  // 절대 도달하지 않음!
    { IsStudent: true } => "학생",
    _ => "일반인"
};

// 테스트
var student = new Person { Age = 15, IsStudent = true };
Console.WriteLine(Classify(student));        // "청소년 학생" ✅
Console.WriteLine(ClassifyWrong(student));   // "청소년" ❌ (잘못된 결과)
```

**중첩 속성 패턴 - 복잡한 객체 그래프 탐색:**

실제 애플리케이션에서는 객체가 깊게 중첩된 경우가 많습니다. 속성 패턴은 이러한 복잡한 구조를 우아하게 처리할 수 있습니다.

```csharp
public class Address
{
    public string City { get; set; }
    public string District { get; set; }
    public string ZipCode { get; set; }
}

public class Customer
{
    public string Name { get; set; }
    public int Age { get; set; }
    public Address Address { get; set; }
    public decimal TotalPurchase { get; set; }
}

// 중첩 속성 패턴 사용
string GetShippingInfo(Customer customer) => customer switch
{
    // null 안전 - Address가 null이면 매칭 실패
    { Address: { City: "서울", District: "강남구" } } => "당일 배송 (강남)",
    { Address: { City: "서울" } } => "당일 배송",
    { Address: { City: "부산" or "인천" } } => "익일 배송",
    { Address: { City: var city } } when city != null => $"{city} - 2-3일 배송",
    { Address: null } => "주소 정보 없음",
    _ => "배송 불가"
};

// 복잡한 비즈니스 로직 표현
string GetCustomerTier(Customer customer) => customer switch
{
    // VIP 조건: 서울 강남, 구매액 1000만원 이상
    { Address: { City: "서울", District: "강남구" }, TotalPurchase: >= 10_000_000 }
        => "VIP GOLD",
    
    // 준VIP: 서울 거주, 구매액 500만원 이상
    { Address: { City: "서울" }, TotalPurchase: >= 5_000_000 }
        => "VIP SILVER",
    
    // 우수 고객: 구매액 100만원 이상
    { TotalPurchase: >= 1_000_000 }
        => "우수 고객",
    
    // 일반 고객
    { TotalPurchase: > 0 }
        => "일반 고객",
    
    _ => "신규 고객"
};

// 사용 예
var vipCustomer = new Customer
{
    Name = "김부자",
    Age = 45,
    Address = new Address { City = "서울", District = "강남구", ZipCode = "06000" },
    TotalPurchase = 15_000_000
};

Console.WriteLine($"{vipCustomer.Name}: {GetCustomerTier(vipCustomer)}");
// 출력: 김부자: VIP GOLD
Console.WriteLine($"배송 정보: {GetShippingInfo(vipCustomer)}");
// 출력: 배송 정보: 당일 배송 (강남)
```

**확장 속성 패턴 (C# 10+) - 점 표기법의 간결함:**

C# 10에서는 중첩된 속성 접근을 점(`.`) 표기법으로 더욱 간결하게 표현할 수 있게 되었습니다.

```csharp
// C# 8-9 방식 - 중첩된 중괄호
if (customer is { Address: { City: "서울" } }) { }

// C# 10+ 방식 - 점 표기법 (더 간결)
if (customer is { Address.City: "서울" }) { }

// Switch 식에서도 동일
string GetDeliverySpeed(Customer c) => c switch
{
    // 확장 속성 패턴으로 더 읽기 쉬움
    { Address.City: "서울", Address.District: "강남구" } => "2시간 배송",
    { Address.City: "서울" } => "당일 배송",
    { Address.City: not null } => "일반 배송",
    _ => "배송 불가"
};

// 더 깊은 중첩도 가능
public class Company
{
    public Customer PrimaryContact { get; set; }
}

string GetCompanyLocation(Company company) => company switch
{
    { PrimaryContact.Address.City: "서울" } => "서울 소재",
    { PrimaryContact.Address.City: var city } when city != null => $"{city} 소재",
    _ => "위치 정보 없음"
};
```

```csharp
// C# 9 방식 (중첩된 중괄호)
if (customer is { Address: { City: "서울" } }) { }

// C# 10+ 방식 (점 표기법)
if (customer is { Address.City: "서울" }) { }

// switch 식에서도 동일
string GetInfo(Customer c) => c switch
{
    { Address.City: "서울", Address.ZipCode: "12345" } => "강남구",
    { Address.City: "부산" } => "부산 지역",
    _ => "기타"
};
```

**실전 예제: 주문 상태 처리:**

```csharp
public enum OrderStatus { Pending, Processing, Shipped, Delivered, Cancelled }

public class Order
{
    public int Id { get; set; }
    public OrderStatus Status { get; set; }
    public decimal Amount { get; set; }
    public DateTime OrderDate { get; set; }
}

string GetOrderMessage(Order order) => order switch
{
    { Status: OrderStatus.Pending, Amount: > 100000 } 
        => "고액 주문 대기 중 - 관리자 승인 필요",
    
    { Status: OrderStatus.Pending } 
        => "주문 대기 중",
    
    { Status: OrderStatus.Processing, Amount: > 50000 } 
        => "고액 주문 처리 중",
    
    { Status: OrderStatus.Shipped } 
        => "배송 중",
    
    { Status: OrderStatus.Delivered } 
        => "배송 완료",
    
    { Status: OrderStatus.Cancelled } 
        => "주문 취소됨",
    
    _ => "알 수 없는 상태"
};

var order1 = new Order { Id = 1, Status = OrderStatus.Pending, Amount = 150000 };
var order2 = new Order { Id = 2, Status = OrderStatus.Shipped, Amount = 30000 };

Console.WriteLine(GetOrderMessage(order1));  // 고액 주문 대기 중 - 관리자 승인 필요
Console.WriteLine(GetOrderMessage(order2));  // 배송 중
```

**속성 패턴과 타입 패턴 결합:**

```csharp
object data = new Person { Name = "이영희", Age = 25, City = "대전" };

string result = data switch
{
    Person { Age: < 20, City: "서울" } p => $"{p.Name} - 서울의 청소년",
    Person { Age: >= 60 } p => $"{p.Name} - 경로 우대",
    Person { City: "서울" } p => $"{p.Name} - 서울 시민",
    Person p => $"{p.Name} - 일반",
    _ => "사람이 아님"
};

Console.WriteLine(result);  // 이영희 - 일반
```

---

## 27.3 위치 패턴

위치 패턴(Positional Pattern)은 객체를 위치 기반으로 분해(deconstruct)하여 패턴 매칭을 수행합니다. 주로 튜플(Tuple), 레코드(Record), 그리고 `Deconstruct` 메서드를 제공하는 타입에서 사용됩니다.

**튜플 패턴:**

```csharp
// 튜플 생성
var point = (X: 10, Y: 20);

// 위치 패턴으로 분해
if (point is (10, 20))
{
    Console.WriteLine("점이 (10, 20)입니다.");
}

// 변수로 캡처
if (point is (var x, var y))
{
    Console.WriteLine($"X: {x}, Y: {y}");
}

// 특정 위치만 검사
if (point is (10, _))  // Y 값은 무시
{
    Console.WriteLine("X가 10입니다.");
}
```

**switch 식에서의 튜플 패턴:**

```csharp
var coordinates = (X: 5, Y: 0);

string GetQuadrant((int X, int Y) point) => point switch
{
    (0, 0) => "원점",
    (> 0, > 0) => "1사분면",
    (< 0, > 0) => "2사분면",
    (< 0, < 0) => "3사분면",
    (> 0, < 0) => "4사분면",
    (0, _) => "X축",
    (_, 0) => "Y축"
};

Console.WriteLine(GetQuadrant(coordinates));  // Y축
Console.WriteLine(GetQuadrant((3, 4)));       // 1사분면
Console.WriteLine(GetQuadrant((-2, 5)));      // 2사분면
```

**레코드와 위치 패턴:**

레코드 타입은 위치 패턴을 자연스럽게 지원합니다.

```csharp
// 레코드 정의
public record Point(int X, int Y);
public record Person(string Name, int Age, string City);

// 위치 패턴 사용
Point point = new Point(10, 20);

if (point is (10, 20))
{
    Console.WriteLine("점이 (10, 20)입니다.");
}

// switch 식에서 사용
string Describe(Point p) => p switch
{
    (0, 0) => "원점",
    (var x, 0) => $"X축 위의 점 (X={x})",
    (0, var y) => $"Y축 위의 점 (Y={y})",
    (var x, var y) when x == y => $"y=x 직선 위의 점 ({x}, {y})",
    (var x, var y) => $"일반 점 ({x}, {y})"
};

Console.WriteLine(Describe(new Point(0, 0)));   // 원점
Console.WriteLine(Describe(new Point(5, 0)));   // X축 위의 점 (X=5)
Console.WriteLine(Describe(new Point(0, 10)));  // Y축 위의 점 (Y=10)
Console.WriteLine(Describe(new Point(7, 7)));   // y=x 직선 위의 점 (7, 7)
```

**클래스에 Deconstruct 메서드 구현:**

사용자 정의 클래스에 `Deconstruct` 메서드를 추가하면 위치 패턴을 사용할 수 있습니다.

```csharp
public class Rectangle
{
    public int Width { get; set; }
    public int Height { get; set; }

    // Deconstruct 메서드 정의
    public void Deconstruct(out int width, out int height)
    {
        width = Width;
        height = Height;
    }
}

Rectangle rect = new Rectangle { Width = 10, Height = 20 };

// 위치 패턴 사용 가능
if (rect is (10, 20))
{
    Console.WriteLine("10x20 직사각형입니다.");
}

// 변수로 캡처
if (rect is (var w, var h))
{
    Console.WriteLine($"넓이: {w * h}");
}

// switch 식
string ClassifyRectangle(Rectangle r) => r switch
{
    (var w, var h) when w == h => "정사각형",
    (var w, var h) when w > h => "가로로 긴 직사각형",
    (var w, var h) when w < h => "세로로 긴 직사각형",
    _ => "알 수 없음"
};

Console.WriteLine(ClassifyRectangle(rect));  // 세로로 긴 직사각형
```

**복잡한 중첩 구조 분해:**

```csharp
public record Address(string City, string ZipCode);
public record Person(string Name, int Age, Address Address);

Person person = new Person("홍길동", 30, new Address("서울", "12345"));

// 중첩된 위치 패턴
string GetInfo(Person p) => p switch
{
    ("홍길동", _, ("서울", _)) => "홍길동님(서울)",
    (var name, < 20, _) => $"{name}님(청소년)",
    (var name, >= 60, _) => $"{name}님(경로)",
    (var name, _, (var city, _)) => $"{name}님({city})",
    _ => "알 수 없음"
};

Console.WriteLine(GetInfo(person));  // 홍길동님(서울)
```

**실전 예제: 가위바위보 게임:**

```csharp
enum Hand { Rock, Paper, Scissors }

string DetermineWinner((Hand player1, Hand player2) game) => game switch
{
    (Hand.Rock, Hand.Scissors) => "플레이어 1 승리",
    (Hand.Paper, Hand.Rock) => "플레이어 1 승리",
    (Hand.Scissors, Hand.Paper) => "플레이어 1 승리",
    (Hand.Scissors, Hand.Rock) => "플레이어 2 승리",
    (Hand.Rock, Hand.Paper) => "플레이어 2 승리",
    (Hand.Paper, Hand.Scissors) => "플레이어 2 승리",
    (var h1, var h2) when h1 == h2 => "무승부",
    _ => "오류"
};

Console.WriteLine(DetermineWinner((Hand.Rock, Hand.Scissors)));  // 플레이어 1 승리
Console.WriteLine(DetermineWinner((Hand.Paper, Hand.Paper)));    // 무승부
```

---

## 27.4 관계 패턴

관계 패턴(Relational Pattern)은 C# 9.0에서 도입된 기능으로, 값의 크기를 비교하는 패턴입니다. `<`, `<=`, `>`, `>=` 연산자를 사용하여 숫자 범위와 경계 조건을 직관적으로 표현할 수 있습니다.

**기본 관계 패턴:**

```csharp
int score = 85;

// 전통적인 방식
if (score >= 90)
{
    Console.WriteLine("A학점");
}

// 관계 패턴 사용
string grade = score switch
{
    >= 90 => "A학점",
    >= 80 => "B학점",
    >= 70 => "C학점",
    >= 60 => "D학점",
    _ => "F학점"
};

Console.WriteLine(grade);  // B학점
```

**관계 패턴의 장점:**
- 코드가 더 선언적이고 읽기 쉬움
- 범위 검사를 명확하게 표현
- switch 식에서 간결한 다중 조건 분기

**숫자 범위 분류:**

```csharp
int GetAgeGroup(int age) => age switch
{
    < 0 => throw new ArgumentException("나이는 음수일 수 없습니다."),
    < 13 => 1,  // 어린이
    < 20 => 2,  // 청소년
    < 60 => 3,  // 성인
    _ => 4      // 노년
};

string GetAgeGroupName(int age) => age switch
{
    < 13 => "어린이",
    < 20 => "청소년",
    < 60 => "성인",
    >= 60 => "노년",
    _ => "알 수 없음"
};

Console.WriteLine(GetAgeGroupName(15));  // 청소년
Console.WriteLine(GetAgeGroupName(45));  // 성인
Console.WriteLine(GetAgeGroupName(70));  // 노년
```

**온도 분류 예제:**

```csharp
string ClassifyTemperature(double celsius) => celsius switch
{
    < -273.15 => "불가능한 온도 (절대 영도 이하)",
    < 0 => "영하",
    0 => "어는점",
    > 0 and < 100 => "일반 온도",
    100 => "끓는점",
    > 100 => "끓는점 이상",
    _ => "알 수 없음"
};

Console.WriteLine(ClassifyTemperature(-10));   // 영하
Console.WriteLine(ClassifyTemperature(25));    // 일반 온도
Console.WriteLine(ClassifyTemperature(100));   // 끓는점
```

**속성 패턴과 관계 패턴 결합:**

```csharp
public record Product(string Name, decimal Price, int Stock);

string GetStockStatus(Product product) => product switch
{
    { Stock: 0 } => "품절",
    { Stock: < 10 } => "재고 부족",
    { Stock: < 50 } => "재고 보통",
    { Stock: >= 50 } => "재고 충분",
    _ => "알 수 없음"
};

string GetPriceCategory(Product product) => product switch
{
    { Price: < 10000 } => "저가",
    { Price: >= 10000 and < 50000 } => "중가",
    { Price: >= 50000 } => "고가",
    _ => "알 수 없음"
};

var product1 = new Product("노트북", 1200000, 5);
var product2 = new Product("마우스", 15000, 100);

Console.WriteLine($"{product1.Name}: {GetStockStatus(product1)}, {GetPriceCategory(product1)}");
// 출력: 노트북: 재고 부족, 고가

Console.WriteLine($"{product2.Name}: {GetStockStatus(product2)}, {GetPriceCategory(product2)}");
// 출력: 마우스: 재고 충분, 중가
```

**실전 예제: BMI 계산 및 분류:**

```csharp
public record Person(string Name, double HeightCm, double WeightKg)
{
    public double BMI => WeightKg / Math.Pow(HeightCm / 100, 2);
}

string ClassifyBMI(Person person) => person.BMI switch
{
    < 18.5 => "저체중",
    >= 18.5 and < 23 => "정상",
    >= 23 and < 25 => "과체중",
    >= 25 and < 30 => "비만",
    >= 30 => "고도 비만",
    _ => "알 수 없음"
};

var person = new Person("홍길동", 175, 70);
Console.WriteLine($"BMI: {person.BMI:F1}, 분류: {ClassifyBMI(person)}");
// 출력: BMI: 22.9, 분류: 정상
```

**할인율 계산:**

```csharp
decimal CalculateDiscount(decimal totalAmount) => totalAmount switch
{
    < 10000 => 0,           // 할인 없음
    < 50000 => 0.05m,       // 5% 할인
    < 100000 => 0.10m,      // 10% 할인
    < 500000 => 0.15m,      // 15% 할인
    >= 500000 => 0.20m      // 20% 할인
};

decimal GetFinalPrice(decimal amount)
{
    var discount = CalculateDiscount(amount);
    return amount * (1 - discount);
}

Console.WriteLine($"구매금액: 75,000원, 할인율: {CalculateDiscount(75000) * 100}%, 최종금액: {GetFinalPrice(75000):N0}원");
// 출력: 구매금액: 75,000원, 할인율: 10%, 최종금액: 67,500원
```

---

## 27.5 논리 패턴

논리 패턴(Logical Pattern)은 C# 9.0에서 도입된 기능으로, 여러 패턴을 논리 연산자(`and`, `or`, `not`)로 결합하여 복잡한 조건을 표현합니다. 이는 불 대수(Boolean Algebra)의 개념을 패턴 매칭에 적용한 것으로, 가독성 높은 조건 표현을 가능하게 합니다.

**`and` 패턴 (논리곱):**

두 패턴이 모두 일치할 때만 true를 반환합니다.

```csharp
int number = 42;

// 범위 검사
if (number is >= 10 and <= 100)
{
    Console.WriteLine("10 이상 100 이하입니다.");
}

// switch 식에서 사용
string ClassifyNumber(int n) => n switch
{
    >= 0 and < 10 => "한 자리 양수",
    >= 10 and < 100 => "두 자리 양수",
    >= 100 and < 1000 => "세 자리 양수",
    < 0 and > -10 => "한 자리 음수",
    _ => "기타"
};

Console.WriteLine(ClassifyNumber(42));   // 두 자리 양수
Console.WriteLine(ClassifyNumber(-5));   // 한 자리 음수
```

**`or` 패턴 (논리합):**

패턴 중 하나라도 일치하면 true를 반환합니다.

```csharp
char character = 'A';

// 모음 검사
if (character is 'a' or 'e' or 'i' or 'o' or 'u' or 'A' or 'E' or 'I' or 'O' or 'U')
{
    Console.WriteLine("모음입니다.");
}

// switch 식
string ClassifyChar(char c) => c switch
{
    >= 'a' and <= 'z' => "소문자",
    >= 'A' and <= 'Z' => "대문자",
    >= '0' and <= '9' => "숫자",
    ' ' or '\t' or '\n' => "공백 문자",
    _ => "특수 문자"
};

Console.WriteLine(ClassifyChar('A'));    // 대문자
Console.WriteLine(ClassifyChar('5'));    // 숫자
Console.WriteLine(ClassifyChar(' '));    // 공백 문자
```

**`not` 패턴 (논리 부정):**

패턴이 일치하지 않을 때 true를 반환합니다.

```csharp
object obj = "Hello";

// null이 아닌 경우
if (obj is not null)
{
    Console.WriteLine("null이 아닙니다.");
}

// 특정 타입이 아닌 경우
if (obj is not int)
{
    Console.WriteLine("정수가 아닙니다.");
}

// switch 식
string DescribeObject(object o) => o switch
{
    not null => $"null이 아님: {o}",
    null => "null"
};
```

**복잡한 논리 패턴 조합:**

```csharp
public record Person(string Name, int Age, bool IsStudent);

string GetTicketPrice(Person person) => person switch
{
    // 청소년이거나 학생
    { Age: < 19 } or { IsStudent: true } => "할인 요금",
    
    // 노년층이면서 학생이 아님
    { Age: >= 65 } and { IsStudent: false } => "경로 우대",
    
    // 성인이면서 학생이 아님
    { Age: >= 19 and < 65 } and not { IsStudent: true } => "일반 요금",
    
    _ => "확인 필요"
};

var student = new Person("학생", 25, true);
var senior = new Person("어르신", 70, false);
var adult = new Person("성인", 35, false);

Console.WriteLine($"{student.Name}: {GetTicketPrice(student)}");   // 학생: 할인 요금
Console.WriteLine($"{senior.Name}: {GetTicketPrice(senior)}");     // 어르신: 경로 우대
Console.WriteLine($"{adult.Name}: {GetTicketPrice(adult)}");       // 성인: 일반 요금
```

**실전 예제: 상품 필터링:**

```csharp
public record Product(string Name, decimal Price, string Category, bool InStock);

bool ShouldDisplay(Product product) => product switch
{
    // 재고가 있고 (전자제품이거나 책)이면서 가격이 적절한 경우
    { InStock: true } and ({ Category: "전자제품" } or { Category: "책" }) and { Price: < 100000 }
        => true,
    
    // 재고가 있고 세일 중인 의류
    { InStock: true, Category: "의류", Price: < 50000 }
        => true,
    
    _ => false
};

var products = new[]
{
    new Product("노트북", 1200000, "전자제품", true),
    new Product("마우스", 25000, "전자제품", true),
    new Product("프로그래밍 책", 35000, "책", true),
    new Product("셔츠", 45000, "의류", true),
    new Product("품절 상품", 10000, "기타", false)
};

Console.WriteLine("표시할 상품:");
foreach (var product in products.Where(ShouldDisplay))
{
    Console.WriteLine($"- {product.Name} ({product.Price:N0}원)");
}
// 출력:
// 표시할 상품:
// - 마우스 (25,000원)
// - 프로그래밍 책 (35,000원)
// - 셔츠 (45,000원)
```

**날짜 범위 검사:**

```csharp
bool IsWeekend(DateTime date) => date.DayOfWeek is DayOfWeek.Saturday or DayOfWeek.Sunday;

bool IsBusinessHour(DateTime time) => time.Hour is >= 9 and < 18 
    and time.DayOfWeek is not (DayOfWeek.Saturday or DayOfWeek.Sunday);

string GetTimeStatus(DateTime dt) => dt switch
{
    { DayOfWeek: DayOfWeek.Saturday or DayOfWeek.Sunday } 
        => "주말",
    
    { Hour: >= 9 and < 18 } 
        => "업무 시간",
    
    { Hour: >= 18 and < 22 } 
        => "저녁 시간",
    
    { Hour: >= 22 or < 6 } 
        => "심야 시간",
    
    _ => "기타"
};

var now = DateTime.Now;
Console.WriteLine($"현재: {GetTimeStatus(now)}");
Console.WriteLine($"업무 시간 여부: {IsBusinessHour(now)}");
```

**복합 조건 검증:**

```csharp
public record Order(decimal Amount, string PaymentMethod, bool IsMember);

string ValidateOrder(Order order) => order switch
{
    // 회원이고 (신용카드 또는 계좌이체)이며 금액이 적절
    { IsMember: true } and ({ PaymentMethod: "신용카드" } or { PaymentMethod: "계좌이체" }) 
        and { Amount: > 0 and < 1000000 }
        => "주문 가능",
    
    // 비회원이고 신용카드이며 소액
    { IsMember: false, PaymentMethod: "신용카드", Amount: > 0 and < 100000 }
        => "주문 가능 (비회원)",
    
    { Amount: <= 0 }
        => "잘못된 금액",
    
    { Amount: >= 1000000 }
        => "고액 주문 - 확인 필요",
    
    _ => "주문 불가"
};

var order1 = new Order(50000, "신용카드", true);
var order2 = new Order(150000, "무통장입금", false);
var order3 = new Order(2000000, "신용카드", true);

Console.WriteLine(ValidateOrder(order1));  // 주문 가능
Console.WriteLine(ValidateOrder(order2));  // 주문 불가
Console.WriteLine(ValidateOrder(order3));  // 고액 주문 - 확인 필요
```

---

## 마치며

패턴 매칭은 C#을 단순한 객체지향 언어에서 다중 패러다임(Multi-paradigm) 언어로 진화시킨 핵심 기능 중 하나입니다. 이 장에서 우리는 타입 패턴, 속성 패턴, 위치 패턴, 관계 패턴, 논리 패턴이라는 다섯 가지 주요 패턴 형태를 깊이 있게 탐구했으며, 각각이 어떻게 복잡한 조건 로직을 간결하고 안전하게 표현하는지 배웠습니다.

**패턴 매칭의 철학적 의미:**

패턴 매칭은 단순한 문법적 편의를 넘어서, 프로그래밍에 대한 근본적인 사고방식의 전환을 요구합니다. 전통적인 명령형(imperative) 프로그래밍이 "무엇을 어떻게 할 것인가(How)"에 초점을 맞춘다면, 패턴 매칭은 "데이터가 어떤 형태인가(What)"에 집중합니다. 이러한 선언적(declarative) 접근은 코드의 의도를 더 명확하게 드러내며, 유지보수성과 테스트 용이성을 크게 향상시킵니다.

함수형 프로그래밍의 창시자 중 한 명인 John Backus는 1977년 튜링상 수상 강연에서 "프로그래밍은 대수학이 되어야 한다"고 주장했습니다. 패턴 매칭은 바로 이러한 대수적 사고를 실용적인 프로그래밍 언어에 구현한 것으로, 데이터의 구조와 연산을 명확하게 분리하여 추론하기 쉬운 코드를 작성할 수 있게 합니다.

**실무에서의 패턴 매칭 활용 전략:**

1. **타입 패턴 - 다형성의 새로운 패러다임**
   - 전통적인 가상 메서드(virtual method) 디스패치의 대안으로 활용
   - Expression Problem이 발생하는 도메인(컴파일러, 인터프리터, AST 처리)에서 특히 강력
   - 비지터 패턴보다 간결하면서도 동일한 표현력 제공
   - **활용 시나리오**: API 응답 처리, 이벤트 핸들링, 플러그인 시스템

2. **속성 패턴 - 상태 기반 로직의 선언적 표현**
   - 복잡한 비즈니스 규칙을 읽기 쉬운 형태로 문서화
   - null 안전성을 자동으로 보장하여 방어적 프로그래밍 필요성 감소
   - 도메인 주도 설계(DDD)에서 값 객체(Value Object) 검증에 최적
   - **활용 시나리오**: 주문 검증, 권한 체크, 상태 머신 구현, 할인 로직

3. **위치 패턴 - 구조 분해의 우아함**
   - 튜플과 레코드를 활용한 경량 데이터 전송 객체(DTO) 처리
   - 함수형 프로그래밍 스타일의 데이터 변환 파이프라인 구축
   - 복잡한 중첩 구조를 평탄화(flatten)하여 가독성 향상
   - **활용 시나리오**: 좌표 처리, 파서 구현, 수학 연산, 그래프 알고리즘

4. **관계 패턴 - 범위 검사의 직관적 표현**
   - 숫자 범위 검사를 비즈니스 도메인 언어로 표현
   - 경계값 테스트(boundary testing)를 명확하게 문서화
   - if-else 체인보다 의도가 명확하고 유지보수 용이
   - **활용 시나리오**: 등급 분류, 가격 책정, 성능 모니터링, 임계값 처리

5. **논리 패턴 - 복합 조건의 조합**
   - 불 대수(Boolean Algebra)의 원리를 코드에 직접 적용
   - 복잡한 자격 검증이나 필터링 로직을 선언적으로 표현
   - 드모르간의 법칙 등을 활용한 조건 최적화
   - **활용 시나리오**: 복합 검색 필터, 접근 제어, 규칙 엔진, 데이터 검증

**패턴 매칭의 성능 고려사항:**

패턴 매칭은 추상화의 비용(cost of abstraction)이 매우 낮은 기능입니다. Roslyn 컴파일러는 다음과 같은 최적화를 수행합니다:

- **인라인 확장(Inlining)**: 간단한 패턴은 메서드 호출 없이 직접 인라인됨
- **결정 트리 최적화**: 여러 패턴을 O(log n) 시간에 평가할 수 있는 최적 결정 트리 생성
- **점프 테이블**: 상수 기반 패턴은 O(1) 해시 테이블이나 점프 테이블로 변환
- **타입 검사 캐싱**: 동일 객체에 대한 반복 타입 검사 최소화

따라서 패턴 매칭은 가독성을 희생하지 않으면서도 전통적인 if-else 체인과 동등하거나 더 나은 성능을 제공합니다.

**안티패턴(Anti-patterns)과 주의사항:**

패턴 매칭을 남용하면 오히려 코드를 복잡하게 만들 수 있습니다:

1. **과도한 중첩 패턴**: 3단계 이상 중첩된 속성 패턴은 가독성을 해침
   ```csharp
   // ❌ 너무 복잡함
   if (x is { A: { B: { C: { D: 42 } } } })
   
   // ✅ 중간 변수나 헬퍼 메서드 사용
   bool HasSpecificValue(X x) => x.A?.B?.C?.D == 42;
   ```

2. **패턴 순서 오류**: switch 식에서 일반적인 패턴을 먼저 배치하면 후속 패턴이 절대 실행되지 않음
   - 컴파일러 경고를 주의 깊게 확인
   - 단위 테스트로 모든 분기 검증

3. **타입 안전성 과신**: switch 식이 모든 케이스를 강제하지는 않음
   - 새로운 타입 추가 시 컴파일 오류가 발생하지 않을 수 있음
   - `_` 패턴에서 예외를 던져 누락 감지

4. **가상 메서드와의 혼용**: 같은 계층구조에서 패턴 매칭과 가상 메서드를 혼용하면 혼란
   - 일관된 접근 방식 선택
   - 팀 코딩 규칙으로 명확히 정의

**함수형 프로그래밍으로의 여정:**

패턴 매칭을 마스터하는 것은 함수형 프로그래밍 패러다임을 이해하는 첫 걸음입니다. C#은 LINQ, 람다 식, record 타입, 패턴 매칭 등을 통해 점진적으로 함수형 개념을 도입해왔으며, 이는 다음과 같은 이점을 제공합니다:

- **불변성(Immutability)**: 데이터를 변경하지 않고 새로운 값을 생성
- **참조 투명성(Referential Transparency)**: 같은 입력에 항상 같은 출력
- **합성 가능성(Composability)**: 작은 함수를 조합하여 복잡한 로직 구축
- **테스트 용이성**: 부작용(side effect)이 없어 단위 테스트가 간단

**다음 단계 - 계속된 학습:**

패턴 매칭은 C#의 진화하는 기능입니다. C# 11의 리스트 패턴과 슬라이스 패턴, 그리고 향후 버전에서 추가될 기능들을 지속적으로 학습하세요:

- **C# 11 리스트 패턴**: `[1, 2, .. var rest]` 형태의 시퀀스 매칭
- **C# 12 이후**: 활성 패턴(Active Patterns), 뷰 패턴(View Patterns) 등의 논의 진행 중
- **함수형 C# 라이브러리**: LanguageExt, Optional 등의 라이브러리로 함수형 개념 확장

**실습 과제:**

이론을 실전에 적용하기 위한 과제들:

1. **초급 - 도형 계산기 구현**
   - Shape 추상 클래스와 Circle, Rectangle, Triangle 파생 클래스 생성
   - 타입 패턴으로 넓이, 둘레 계산 함수 구현
   - 속성 패턴으로 "큰 도형" (넓이 > 100) 필터링

2. **중급 - 전자상거래 주문 시스템**
   - Order, Customer, Product 클래스 설계
   - 속성 패턴으로 복잡한 할인 규칙 구현 (회원 등급, 구매액, 지역 등 고려)
   - 관계 패턴으로 배송비 계산 (금액별 차등)
   - 논리 패턴으로 결제 방법 검증

3. **고급 - 간단한 표현식 평가기(Expression Evaluator)**
   - 산술 표현식을 나타내는 레코드 계층 설계
     ```csharp
     abstract record Expr;
     record Const(int Value) : Expr;
     record Add(Expr Left, Expr Right) : Expr;
     record Mult(Expr Left, Expr Right) : Expr;
     ```
   - 위치 패턴과 재귀를 사용한 평가 함수 구현
   - 타입 패턴으로 최적화 규칙 적용 (예: `Add(Const(0), x) => x`)

4. **심화 - 상태 머신 구현**
   - 트래픽 신호등, 자판기, 게임 캐릭터 상태 등 선택
   - 속성 패턴으로 상태 전이 로직 구현
   - 논리 패턴으로 복잡한 전이 조건 표현

**마무리 생각:**

> "프로그래밍 언어는 단순히 컴퓨터에게 명령을 내리는 도구가 아니라, 
> 프로그래머가 문제를 생각하고 해결책을 표현하는 사고의 틀(framework of thought)이다."
> — Alan Perlis (첫 튜링상 수상자)

패턴 매칭은 바로 이러한 "사고의 틀"을 확장하는 강력한 도구입니다. 데이터의 형태를 선언적으로 기술하고, 그에 따른 처리를 명확하게 매핑함으로써, 우리는 더 이해하기 쉽고 유지보수 가능한 코드를 작성할 수 있습니다.

C#의 패턴 매칭은 아직도 진화하고 있습니다. 커뮤니티의 피드백과 함수형 프로그래밍 언어들의 모범 사례를 반영하여, 매 버전마다 새로운 기능이 추가되고 있습니다. 이 강력한 도구를 마스터하여, 여러분의 코드를 다음 수준으로 끌어올리시기 바랍니다.

**추가 학습 자원:**

- **공식 문서**: [Microsoft Docs - Pattern Matching](https://docs.microsoft.com/dotnet/csharp/fundamentals/functional/pattern-matching)
- **C# 언어 사양**: [C# Language Specification - Patterns](https://github.com/dotnet/csharplang/blob/main/spec/patterns.md)
- **함수형 C# 서적**: 
  - "Functional Programming in C#" by Enrico Buonanno
  - "C# in Depth" by Jon Skeet (패턴 매칭 챕터)
- **온라인 리소스**:
  - Mads Torgersen (C# 수석 디자이너)의 블로그와 발표 자료
  - .NET Blog의 패턴 매칭 관련 포스트
  - Channel 9의 C# 언어 디자인 미팅 녹화

**독자 여러분께:**

이 장을 통해 패턴 매칭의 개념과 실전 활용법을 익혔다면, 다음 장인 "Nullable 참조 타입"으로 넘어가기 전에 실습 과제를 직접 구현해보시기 바랍니다. 이론을 실제로 적용해보는 과정에서 진정한 이해가 시작됩니다.

패턴 매칭은 여러분의 C# 프로그래밍을 한 단계 업그레이드할 것입니다. 행운을 빕니다!
