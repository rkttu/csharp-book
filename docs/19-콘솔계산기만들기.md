# 19장. 콘솔 계산기 만들기

콘솔 계산기는 컴퓨터 과학 교육의 오랜 전통을 따르는 첫 번째 실전 통합 프로젝트로, 단순히 코드를 작성하는 것을 넘어서 **소프트웨어 공학(Software Engineering)**의 전체 생명주기를 경험하게 합니다. 변수, 연산자, 제어문, 메서드, 예외 처리, 컬렉션 등 1장부터 18장까지 학습한 모든 개념을 하나의 실용적인 애플리케이션으로 융합하며, 이론적 지식을 실무 능력으로 전환하는 결정적 전환점이 됩니다.

**계산기의 역사적 맥락과 컴퓨터 과학에서의 위치:**

계산기는 인류 문명과 함께 진화해 온 도구입니다. 기원전 2700년경 바빌로니아의 주판(Abacus)부터 시작하여, 1642년 블레즈 파스칼(Blaise Pascal)의 기계식 계산기 파스칼린(Pascaline), 1820년 토마스 드 콜마르(Thomas de Colmar)의 아리스모미터(Arithmometer), 그리고 1970년대 휴대용 전자계산기의 대중화까지, 계산 도구의 역사는 곧 컴퓨팅 역사의 핵심입니다.

현대 컴퓨터의 아버지 앨런 튜링(Alan Turing)의 튜링 기계(Turing Machine)는 본질적으로 추상화된 계산기이며, 폰 노이만 아키텍처(Von Neumann Architecture)의 ALU(Arithmetic Logic Unit)는 하드웨어 계산기입니다. 우리가 만들 콘솔 계산기는 이러한 계산 메커니즘을 소프트웨어로 추상화하여, **알고리즘(Algorithm)**과 **데이터 구조(Data Structure)**가 어떻게 실제 문제를 해결하는지 보여줍니다.

**소프트웨어 공학의 핵심 원칙 체험:**

이 프로젝트는 단순한 코딩 연습이 아니라 소프트웨어 공학의 핵심 원칙들을 체험하는 실습장입니다:

1. **요구사항 공학(Requirements Engineering)**: Frederick Brooks의 "The Mythical Man-Month"에서 강조한 "올바른 것을 만드는 것이 올바르게 만드는 것보다 중요하다"는 원칙을 실천합니다. 요구사항을 명확히 정의하고, 우선순위를 정하며, 추적 가능성을 확보하는 방법을 배웁니다.

2. **모듈화와 추상화(Modularization and Abstraction)**: David Parnas의 정보 은닉(Information Hiding) 원칙을 적용하여, 각 기능을 독립적인 메서드로 분리합니다. 이는 단일 책임 원칙(Single Responsibility Principle)과 개방-폐쇄 원칙(Open-Closed Principle)을 실현하는 SOLID 설계의 기초입니다.

3. **증분 개발과 반복(Incremental Development and Iteration)**: Barry Boehm의 나선형 모델(Spiral Model)과 현대 애자일(Agile) 방법론의 핵심인 "일찍, 자주 테스트하라(Test Early, Test Often)"를 실천합니다. MVP(Minimum Viable Product)에서 시작하여 점진적으로 기능을 확장하며, 각 단계마다 동작하는 프로그램을 유지합니다.

4. **방어적 프로그래밍(Defensive Programming)**: Barbara Liskov의 "프로그램은 실패할 수 있음을 가정하라"는 원칙에 따라, 모든 사용자 입력을 검증하고, 예외를 처리하며, 프로그램의 불변조건(Invariant)을 유지합니다. 이는 Bertrand Meyer의 계약에 의한 설계(Design by Contract) 패러다임을 실무에 적용하는 것입니다.

5. **사용자 중심 설계(User-Centered Design)**: Donald Norman의 "The Design of Everyday Things"에서 제시한 발견 가능성(Discoverability)과 피드백(Feedback) 원칙을 콘솔 인터페이스에 적용합니다. 명확한 프롬프트, 즉각적인 오류 메시지, 그리고 직관적인 메뉴 구조를 통해 사용성(Usability)을 극대화합니다.

**프로젝트 기반 학습(Project-Based Learning)의 인지과학적 기반:**

John Dewey의 "learning by doing"과 Jean Piaget의 구성주의(Constructivism)에 기반한 프로젝트 기반 학습은 단순한 지식 전달을 넘어 **깊은 이해(Deep Understanding)**를 촉진합니다. Bloom의 교육 목표 분류학(Bloom's Taxonomy)에서 가장 높은 단계인 **창조(Create)**를 경험하게 됩니다:

- **기억(Remember)**: 1-18장에서 학습한 문법과 개념을 상기합니다
- **이해(Understand)**: 각 개념이 계산기에서 어떤 역할을 하는지 파악합니다
- **적용(Apply)**: 학습한 내용을 실제 코드로 구현합니다
- **분석(Analyze)**: 프로그램 구조를 분해하고 관계를 이해합니다
- **평가(Evaluate)**: 다양한 구현 방식의 장단점을 판단합니다
- **창조(Create)**: 완전히 새로운 기능을 설계하고 구현합니다

이는 Anders Ericsson의 의도적 연습(Deliberate Practice) 이론과도 일치하며, 단순 반복이 아닌 **도전적이고 목표 지향적인 학습**을 통해 전문성을 개발합니다.

**계산기 프로젝트가 가르치는 보편적 프로그래밍 패턴:**

이 프로젝트를 통해 배우는 패턴들은 거의 모든 소프트웨어에 적용됩니다:

1. **REPL 패턴(Read-Eval-Print Loop)**: 대화형 인터프리터와 쉘(Shell)의 기본 구조로, Python, Ruby, Lisp 등의 REPL와 동일한 메커니즘입니다. 입력 → 처리 → 출력 → 반복의 순환은 **이벤트 주도 프로그래밍(Event-Driven Programming)**의 기초입니다.

2. **명령 패턴(Command Pattern)**: Gang of Four의 디자인 패턴 중 하나로, 사용자의 선택을 객체화하여 실행을 지연시키거나 로깅할 수 있습니다. 우리의 메뉴 시스템은 이 패턴의 단순화된 형태입니다.

3. **전략 패턴(Strategy Pattern)**: 각 연산(+, -, *, /)을 독립적인 알고리즘으로 캡슐화하여, 런타임에 선택할 수 있게 합니다. `switch` 문과 람다 식을 통해 이를 간결하게 구현합니다.

4. **저장소 패턴(Repository Pattern)**: 연산 기록을 `List<string>`에 저장하는 것은 데이터 영속성(Data Persistence)의 메모리 기반 구현입니다. 이는 나중에 데이터베이스나 파일 시스템으로 확장할 수 있는 추상화 계층입니다.

5. **파사드 패턴(Facade Pattern)**: 복잡한 하위 시스템(입력 검증, 계산, 기록 관리)을 단순한 인터페이스(메뉴)로 감싸는 것은 복잡도 관리(Complexity Management)의 핵심 기법입니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 소프트웨어 개발의 전체 주기를 경험하며 다음을 습득하게 됩니다:

- **요구사항 공학의 실제**: IEEE 830 표준에 기반한 기능적/비기능적 요구사항 도출, 사용자 스토리(User Story) 작성, 인수 기준(Acceptance Criteria) 정의 방법을 학습합니다. Karl Wiegers의 "Software Requirements"에서 제시한 요구사항 추적 매트릭스(Requirements Traceability Matrix)를 간소화하여 적용하고, MoSCoW 우선순위 기법(Must/Should/Could/Won't)으로 기능을 분류합니다.

- **알고리즘과 계산 모델**: 사칙연산의 컴퓨터 과학적 기반을 탐구합니다. IEEE 754 부동소수점 표준의 한계와 정밀도 문제, 정수 오버플로우와 언더플로우, 그리고 `decimal` 타입이 금융 계산에 필수적인 이유를 이해합니다. 또한 연산자 우선순위(Operator Precedence)와 결합 규칙(Associativity)을 학습하며, Shunting-yard 알고리즘의 기초를 접합니다.

- **상호작용 설계와 REPL 패턴**: 명령줄 인터페이스(CLI)의 설계 원칙을 학습합니다. REPL(Read-Eval-Print Loop)의 구조, 상태 관리(State Management), 그리고 이벤트 루프(Event Loop)의 개념을 이해합니다. 이는 웹 서버, GUI 애플리케이션, 게임 엔진 등의 기본 구조와 동일하며, **반응성(Reactivity)**과 **응답성(Responsiveness)**을 학습합니다.

- **방어적 프로그래밍과 견고성(Robustness)**: 12장에서 학습한 예외 처리를 실전에 적용합니다. Fail-fast vs Fail-safe 전략, 입력 검증(Input Validation)의 OWASP 원칙, 그리고 정규화(Normalization)와 소독(Sanitization) 기법을 배웁니다. `TryParse` 패턴이 null 객체 패턴(Null Object Pattern)과 어떻게 연결되는지, 그리고 C# 8.0의 nullable 참조 타입이 이를 어떻게 개선하는지 이해합니다.

- **데이터 영속성의 기초**: 연산 기록을 메모리(`List<string>`)에 저장하는 것은 **캐싱(Caching)**의 가장 단순한 형태입니다. 이를 통해 CRUD(Create, Read, Update, Delete) 작업, 컬렉션 관리, 그리고 13장에서 배울 파일 I/O로의 확장 경로를 이해합니다. Memento 패턴을 통한 실행 취소(Undo) 기능의 가능성도 탐구합니다.

---

## 19.1 요구사항 분석

소프트웨어 개발의 첫 번째이자 가장 중요한 단계는 **무엇을 만들 것인가**를 명확히 정의하는 것입니다. 요구사항 분석(Requirements Analysis)은 이해관계자(Stakeholder)의 니즈를 기술적 명세(Technical Specification)로 변환하는 핵심 프로세스이며, Frederick Brooks가 "No Silver Bullet"에서 지적한 **본질적 복잡성(Essential Complexity)**과 **우발적 복잡성(Accidental Complexity)**을 구분하는 첫 단계입니다.

**요구사항 공학의 역사적 진화:**

요구사항 공학은 1960년대 소프트웨어 위기(Software Crisis)의 직접적인 산물입니다. 1968년 NATO 소프트웨어 공학 컨퍼런스에서 Edsger Dijkstra는 "프로그래밍은 더 이상 개인의 기술이 아니라 공학적 훈련이 필요하다"고 선언했습니다. 이후 1970년대 Barry Boehm의 나선형 모델(Spiral Model), 1980년대 IEEE 830 표준(Software Requirements Specification), 1990년대 Rational Unified Process(RUP), 그리고 2000년대 애자일의 사용자 스토리(User Story)로 진화했습니다.

현대의 요구사항 분석은 단순히 "무엇을 만들 것인가"를 넘어서, **왜(Why)**, **누구를 위해(For Whom)**, **어떻게(How)**, **언제(When)**를 포괄하는 전략적 활동입니다. Simon Sinek의 "Start With Why"와 Eric Ries의 "Lean Startup" 방법론은 요구사항이 비즈니스 가치(Business Value)와 직결되어야 함을 강조합니다.

**요구사항 분석의 이론적 토대:**

요구사항 분석은 여러 학문 분야의 통합입니다:

1. **인지 과학(Cognitive Science)**: Daniel Kahneman의 "Thinking, Fast and Slow"에서 제시한 System 1(직관)과 System 2(논리) 사고를 활용하여, 명시적(Explicit) 요구사항과 암묵적(Tacit) 요구사항을 모두 발굴합니다.

2. **시스템 이론(Systems Theory)**: Ludwig von Bertalanffy의 일반 시스템 이론을 적용하여, 계산기를 고립된 프로그램이 아닌 사용자-컴퓨터 상호작용 시스템의 일부로 봅니다. 입력(Input), 처리(Process), 출력(Output), 피드백(Feedback)의 순환을 설계합니다.

3. **형식 방법론(Formal Methods)**: Z 표기법이나 VDM(Vienna Development Method) 같은 수학적 명세 언어의 개념을 차용하여, 전제조건(Precondition)과 후조건(Postcondition)을 정의합니다. 예를 들어, 나눗셈 연산은 "분모 ≠ 0"이라는 전제조건을 가집니다.

4. **의사소통 이론(Communication Theory)**: Claude Shannon의 정보 이론을 적용하여, 사용자와 프로그램 간의 의사소통 채널(Console I/O)의 신호 대 잡음비(Signal-to-Noise Ratio)를 최적화합니다. 명확한 프롬프트와 오류 메시지는 잡음을 줄이고 신호를 강화합니다.

**요구사항의 분류와 우선순위:**

IEEE 830 표준에 따르면, 요구사항은 다음과 같이 분류됩니다:

**기능적 요구사항(Functional Requirements):**
시스템이 **무엇을 해야 하는지** 정의합니다. 우리 계산기의 경우:

- **FR-1**: 시스템은 두 개의 숫자를 입력받아 덧셈을 수행해야 한다
- **FR-2**: 시스템은 뺄셈, 곱셈, 나눗셈을 지원해야 한다
- **FR-3**: 시스템은 이전 계산 결과를 최소 10개 저장해야 한다
- **FR-4**: 시스템은 사용자가 메뉴를 통해 기능을 선택할 수 있어야 한다

**비기능적 요구사항(Non-Functional Requirements):**
시스템이 **어떻게 동작해야 하는지** 정의합니다:

- **NFR-1 (사용성)**: 평균 사용자가 설명서 없이 5분 내에 모든 기능을 사용할 수 있어야 한다
- **NFR-2 (성능)**: 모든 연산은 100ms 이내에 완료되어야 한다 (사람의 인지 반응 시간 고려)
- **NFR-3 (신뢰성)**: 잘못된 입력으로 인해 프로그램이 충돌해서는 안 된다 (가용성 99.9%)
- **NFR-4 (유지보수성)**: 새로운 연산 추가 시 기존 코드 변경이 최소화되어야 한다 (개방-폐쇄 원칙)
- **NFR-5 (이식성)**: Windows, Linux, macOS에서 동일하게 동작해야 한다 (.NET의 크로스 플랫폼 특성)

**MoSCoW 우선순위 기법 적용:**

Agile 방법론에서 유래한 MoSCoW(Must have, Should have, Could have, Won't have) 우선순위 기법으로 기능을 분류합니다:

**Must Have (필수):**
- 사칙연산 (+, -, *, /)
- 입력 검증 및 오류 처리
- 메뉴 기반 인터페이스

**Should Have (중요):**
- 연산 기록 저장 및 조회
- 계산 결과 포맷팅 (소수점 처리)

**Could Have (있으면 좋음):**
- 제곱, 제곱근, 삼각함수
- 계산식 파싱 (예: "2 + 3 * 4")
- 색상 있는 출력

**Won't Have (이번 버전에서 제외):**
- 그래프 그리기
- 네트워크 기능
- 데이터베이스 연동

이러한 우선순위는 Pareto 원칙(80/20 법칙)을 따릅니다: 20%의 핵심 기능이 80%의 가치를 제공합니다.

**요구사항 검증 기법(Requirements Validation):**

Tom Gilb의 요구사항 검증 체크리스트를 적용합니다:

1. **완전성(Completeness)**: 모든 필요한 기능이 정의되었는가?
2. **일관성(Consistency)**: 요구사항 간 모순이 없는가?
3. **명확성(Unambiguity)**: 한 가지 해석만 가능한가?
4. **검증 가능성(Verifiability)**: 테스트로 확인할 수 있는가?
5. **추적 가능성(Traceability)**: 각 요구사항이 코드와 매핑되는가?
6. **현실성(Feasibility)**: 주어진 시간과 자원으로 구현 가능한가?

**계산기의 사용자 스토리(User Stories):**

Mike Cohn의 "User Stories Applied"에 따라 사용자 관점에서 요구사항을 표현합니다:

```
As a [사용자 유형]
I want to [기능]
So that [이유/가치]
```

**US-1**: "학생으로서, 나는 빠르게 사칙연산을 수행하고 싶다. 그래서 숙제를 확인할 수 있다."
- 인수 기준: 두 숫자와 연산자를 입력하면 1초 이내에 결과를 본다

**US-2**: "사용자로서, 나는 이전 계산을 다시 보고 싶다. 그래서 실수를 찾을 수 있다."
- 인수 기준: 최소 10개의 이전 계산이 시간순으로 표시된다

**US-3**: "초보 사용자로서, 나는 명확한 안내를 받고 싶다. 그래서 어떻게 사용하는지 알 수 있다."
- 인수 기준: 모든 메뉴 항목에 설명이 있고, 잘못된 입력 시 도움말이 표시된다

### 단계별 개발 계획 - 증분 개발의 실천

Winston Royce의 폭포수 모델(Waterfall Model)이 한 번에 모든 것을 완성하려는 접근이었다면, **증분 개발(Incremental Development)**은 동작하는 소프트웨어를 반복적으로 개선하는 현대적 접근입니다. Kent Beck의 Extreme Programming(XP)과 Scrum의 Sprint 개념을 단순화하여 적용합니다.

**철학적 기반**: Donald Knuth의 "Premature optimization is the root of all evil"과 Eric Raymond의 "Release early, release often"을 결합하여, 완벽하지 않지만 **동작하는(Working)** 프로그램을 우선시합니다.

**단계 1: 최소 기능 제품(MVP - Minimum Viable Product)**

Eric Ries의 Lean Startup 방법론에서 차용한 MVP는 "가장 적은 노력으로 최대의 검증된 학습을 얻는 제품 버전"입니다. 우리의 MVP는:

```csharp
// MVP: 덧셈만 수행하는 가장 단순한 계산기
Console.Write("첫 번째 숫자: ");
double a = double.Parse(Console.ReadLine());
Console.Write("두 번째 숫자: ");
double b = double.Parse(Console.ReadLine());
Console.WriteLine($"결과: {a + b}");
```

**학습 목표**: 
- 콘솔 I/O의 기본 메커니즘 이해
- 문자열-숫자 변환의 필요성 인식
- 변수와 연산자의 실제 사용

**한계 인식**:
- 예외 처리 없음 (잘못된 입력 시 크래시)
- 반복 불가 (한 번 계산 후 종료)
- 확장성 없음 (다른 연산 추가 어려움)

이러한 한계를 인식하는 것 자체가 요구사항을 구체화하는 과정입니다.

**단계 2: 다형성 도입 - 네 가지 연산 지원**

객체지향의 **다형성(Polymorphism)** 개념을 활용하여 연산을 추상화합니다. Gang of Four의 **전략 패턴(Strategy Pattern)**의 단순화된 형태입니다:

```csharp
// 전략 선택: switch 문으로 연산 타입 선택
double result = operator switch
{
    "+" => a + b,
    "-" => a - b,
    "*" => a * b,
    "/" => a / b,
    _ => throw new InvalidOperationException("Unknown operator")
};
```

**설계 패턴**: C# 8.0의 **switch 식(Switch Expression)**은 Haskell의 패턴 매칭(Pattern Matching)에서 영감을 받았으며, 함수형 프로그래밍의 **표현식 중심(Expression-Oriented)** 스타일을 도입합니다.

**확장성 고려**: 나중에 새로운 연산(%, ^, sqrt 등)을 추가할 때 switch 문에 case만 추가하면 됩니다. 이는 **개방-폐쇄 원칙(Open-Closed Principle)**의 실천입니다.

**단계 3: REPL 패턴 구현 - 무한 루프와 메뉴**

Lisp, Python, Ruby 등의 대화형 인터프리터에서 사용하는 **REPL(Read-Eval-Print Loop)** 패턴을 콘솔 앱에 적용합니다:

```csharp
while (true)  // 이벤트 루프 (Event Loop)
{
    ShowMenu();           // Read: 사용자 의도 파악
    string choice = GetUserChoice();
    
    if (choice == "quit") break;  // 탈출 조건
    
    ExecuteCommand(choice);       // Eval: 명령 실행
                                  // Print: 결과 출력 (ExecuteCommand 내부)
}
```

**이론적 배경**:
- **상태 기계(State Machine)**: 프로그램은 "메뉴 표시" → "입력 대기" → "명령 실행" → "결과 출력" 상태를 순환합니다
- **이벤트 주도(Event-Driven)**: 사용자 입력이 "이벤트"로, 메뉴 선택이 "이벤트 핸들러"로 추상화됩니다
- **무한 루프의 철학**: Alan Kay의 "Simple things should be simple, complex things should be possible"을 실현합니다

**단계 4: 견고성 확보 - 방어적 프로그래밍**

Barbara Liskov의 "Design by Contract"와 Bertrand Meyer의 Eiffel 언어 철학을 적용하여, **불변조건(Invariant)**을 유지합니다:

```csharp
double GetNumberInput(string prompt)
{
    while (true)  // 유효한 입력을 얻을 때까지 반복 (재시도 패턴)
    {
        Console.Write(prompt);
        if (double.TryParse(Console.ReadLine(), out double result))
        {
            return result;  // 후조건: 반환값은 항상 유효한 double
        }
        Console.WriteLine("❌ 올바른 숫자를 입력하세요.");
        // 피드백을 통한 학습 (사용자 교육)
    }
}
```

**Fail-Fast vs Fail-Safe**:
- **Fail-Fast**: 오류를 즉시 감지하고 예외를 던짐 (`Parse` 사용)
- **Fail-Safe**: 오류를 허용하고 재시도 (`TryParse` 사용)

우리는 사용자 친화적인 Fail-Safe 접근을 선택합니다.

**단계 5: 상태 관리 - 연산 기록**

Martin Fowler의 "Patterns of Enterprise Application Architecture"에서 제시한 **도메인 모델 패턴(Domain Model Pattern)**을 단순화하여 적용합니다:

```csharp
class Calculator
{
    private List<string> history = new();  // 캡슐화된 상태
    
    public void AddToHistory(string calculation)
    {
        history.Add(calculation);
        // 불변조건: history는 항상 유효한 리스트
    }
    
    public IReadOnlyList<string> GetHistory()
    {
        return history.AsReadOnly();  // 방어적 복사 (Defensive Copy)
    }
}
```

**메모리 관리 고려사항**:
- `List<string>`은 동적 배열로 O(1) 추가, O(n) 공간
- 무한 증가 방지를 위해 최대 크기 제한 (예: 100개) 고려
- LRU(Least Recently Used) 캐시 알고리즘으로 고도화 가능

**각 단계의 테스트 전략**:

Kent Beck의 Test-Driven Development(TDD) 사이클을 적용합니다:
1. **Red**: 실패하는 테스트 작성
2. **Green**: 테스트를 통과하는 최소한의 코드 작성  
3. **Refactor**: 코드 개선 (중복 제거, 명확성 향상)

실전 적용:
- **단계 1**: 수동 테스트 (직접 실행하며 확인)
- **단계 2**: 경계값 테스트 (0, 음수, 매우 큰 수)
- **단계 3**: 사용자 시나리오 테스트 (실제 사용 패턴 모방)
- **단계 4**: 스트레스 테스트 (의도적으로 잘못된 입력)
- **단계 5**: 통합 테스트 (모든 기능의 상호작용)

### 프로그램 흐름 설계 - UML과 의사코드

소프트웨어 설계를 시각화하고 소통하기 위해 **UML(Unified Modeling Language)**과 **의사코드(Pseudocode)**를 활용합니다. Grady Booch, James Rumbaugh, Ivar Jacobson이 1990년대 정립한 UML은 소프트웨어 아키텍처의 공통 언어입니다.

**활동 다이어그램(Activity Diagram) 표현:**

```
[시작]
  ↓
[초기화: history = new List<string>()]
  ↓
[메뉴 표시]
  ↓
[사용자 선택 입력] ←────────┐
  ↓                        │
[선택 분기] ────────────────┤
  ├─ "계산" → [수식 입력] → [계산 실행] → [결과 출력 및 저장] ─┐
  ├─ "기록" → [history 출력] ────────────────────────────────┤
  ├─ "도움말" → [사용법 표시] ───────────────────────────────┤
  └─ "종료" → [종료 확인] → [프로그램 종료]                  │
                                                            │
                            └───────────────────────────────┘
                                       (루프백)
```

**상태 전이도(State Transition Diagram):**

```
     ┌──────────┐
     │   대기   │ ◄─────────┐
     └──────────┘           │
          │                 │
   [메뉴 표시]              │
          ↓                 │
     ┌──────────┐           │
     │ 입력 수신 │           │
     └──────────┘           │
          │                 │
    [사용자 입력]            │
          ↓                 │
     ┌──────────┐           │
     │ 명령 실행 │ ──────────┘
     └──────────┘
          │
     [종료 선택]
          ↓
     ┌──────────┐
     │   종료   │
     └──────────┘
```

**구조화된 의사코드 (Structured Pseudocode):**

Donald Knuth의 Literate Programming 철학에 따라, 코드는 컴퓨터만이 아니라 사람도 읽을 수 있어야 합니다:

```
PROGRAM CalculatorApp
BEGIN
    DECLARE history AS List<String>
    INITIALIZE history TO empty list
    
    PRINT "콘솔 계산기 v1.0"
    PRINT "==================="
    
    REPEAT
        CALL ShowMenu()
        
        READ userChoice FROM Console
        
        CASE userChoice OF
            "1": 
                CALL PerformCalculation()
                // 전제조건: 사용자가 유효한 숫자 입력
                // 후조건: 결과가 history에 추가됨
                
            "2":
                CALL ShowHistory()
                // 전제조건: history가 초기화됨
                // 후조건: 모든 항목이 출력됨
                
            "3":
                IF CONFIRM("정말 종료하시겠습니까?") THEN
                    PRINT "프로그램을 종료합니다."
                    EXIT REPEAT
                END IF
                
            DEFAULT:
                PRINT "잘못된 선택입니다."
                // 견고성: 예상치 못한 입력 처리
        END CASE
        
    UNTIL userChoice = "3"
    
END PROGRAM

PROCEDURE PerformCalculation()
BEGIN
    TRY
        num1 ← GetValidNumber("첫 번째 숫자: ")
        operator ← GetValidOperator("연산자 (+,-,*,/): ")
        num2 ← GetValidNumber("두 번째 숫자: ")
        
        // 도메인 규칙 검증
        IF operator = "/" AND num2 = 0 THEN
            THROW DivideByZeroException
        END IF
        
        result ← Calculate(num1, operator, num2)
        
        record ← FORMAT("{0} {1} {2} = {3}", num1, operator, num2, result)
        history.Add(record)
        
        PRINT "✓ ", record
        
    CATCH FormatException AS ex
        PRINT "❌ 잘못된 형식:", ex.Message
    CATCH DivideByZeroException
        PRINT "❌ 0으로 나눌 수 없습니다"
    CATCH Exception AS ex
        PRINT "❌ 예상치 못한 오류:", ex.Message
        // 로깅 시스템으로 전송 (프로덕션 환경)
    END TRY
END PROCEDURE
```

**시간 복잡도와 공간 복잡도 분석:**

Donald Knuth의 "The Art of Computer Programming"에서 강조한 알고리즘 효율성 분석을 적용합니다:

- **메뉴 표시**: O(1) - 고정된 문자열 출력
- **계산 수행**: O(1) - 단일 산술 연산
- **기록 추가**: O(1) amortized - `List.Add()`의 평균 복잡도
- **기록 출력**: O(n) - n개 항목 순회
- **전체 프로그램**: O(1) 메모리, O(n) 시간 (n = 사용자 상호작용 횟수)

이는 매우 효율적이며, 수백만 번의 계산도 문제없이 처리할 수 있습니다(메모리가 허용하는 한).

**보안 고려사항 (Security Considerations):**

OWASP(Open Web Application Security Project) Top 10의 원칙을 콘솔 앱에 적용합니다:

1. **입력 검증(Input Validation)**: 모든 사용자 입력은 신뢰할 수 없음
2. **에러 처리(Error Handling)**: 민감한 정보를 오류 메시지에 포함하지 않음
3. **최소 권한(Least Privilege)**: 파일 시스템 접근 최소화

우리 계산기는 네트워크나 파일 시스템에 접근하지 않으므로 공격 표면(Attack Surface)이 최소화되어 있습니다.
    사용자 선택 입력받기
    
    만약 선택 == 1이면:
      첫 번째 숫자 입력받기
      연산자 입력받기
      두 번째 숫자 입력받기
      계산 수행
      결과 출력 및 기록에 저장
    
    만약 선택 == 2이면:
      저장된 기록 모두 출력
    
    만약 선택 == 3이면:
      종료 메시지 출력
      반복 탈출
    
    그 외:
      잘못된 선택 메시지 출력
프로그램 종료
```

---

## 19.2 기본 연산 구현

사칙연산은 수학의 가장 기본적인 연산이지만, 컴퓨터에서 이를 구현하는 것은 예상보다 복잡한 **수치 계산(Numerical Computation)**의 문제입니다. 우리는 무한 정밀도의 수학적 실수(ℝ)를 유한 비트의 **부동소수점 표현(Floating-Point Representation)**으로 근사해야 하며, 이 과정에서 **반올림 오차(Rounding Error)**, **정밀도 손실(Loss of Precision)**, 그리고 **특수값 처리(Special Value Handling)**라는 근본적인 도전에 직면합니다.

**수치 계산의 이론적 기반:**

1754년 Leonhard Euler가 부동소수점 표기법을 제안한 이래, 컴퓨터 과학자들은 수치의 디지털 표현에 몰두해왔습니다. 1985년 IEEE 754 표준이 제정되면서 현대 거의 모든 컴퓨터는 동일한 부동소수점 산술을 사용하게 되었습니다. William Kahan이 주도한 이 표준은 ACM Turing Award(1989)를 받을 만큼 중요한 업적으로 평가받습니다.

**IEEE 754 부동소수점 표준:**

C#의 `double` 타입은 IEEE 754의 **binary64(배정밀도, Double Precision)** 형식을 따릅니다:

```
64 bits = 1 bit (부호) + 11 bits (지수) + 52 bits (가수)
표현 범위: ±5.0 × 10⁻³²⁴ ~ ±1.7 × 10³⁰⁸
정밀도: 약 15-17 자리 십진수
```

**특수값(Special Values)**:
- **NaN (Not a Number)**: 0/0, ∞-∞ 같은 정의되지 않은 연산 결과
- **+∞/-∞ (Infinity)**: 1.0/0.0 같은 오버플로우
- **+0/-0 (Signed Zero)**: IEEE 754만의 독특한 특징

이러한 특수값 처리는 David Goldberg의 논문 "What Every Computer Scientist Should Know About Floating-Point Arithmetic"(1991)에서 상세히 다뤄집니다.

**부동소수점의 함정(Pitfalls):**

```csharp
// 예상과 다른 결과
Console.WriteLine(0.1 + 0.2 == 0.3);  // False!
Console.WriteLine(0.1 + 0.2);          // 0.30000000000000004

// 이유: 0.1과 0.2는 이진 부동소수점으로 정확히 표현 불가
// 0.1₁₀ = 0.0001100110011...₂ (무한 반복)
```

이는 금융 계산에 `double`이 아닌 `decimal`(128비트 십진 부동소수점)을 사용해야 하는 이유입니다.

### 최소 기능 제품(MVP) - 덧셈 계산기

Eric Ries의 Lean Startup 철학을 적용하여, "학습을 최대화하는 최소한의 제품"부터 시작합니다. 이는 **프로토타입(Prototype)**이 아니라 **실제로 동작하는 제품**입니다:

```csharp
Console.WriteLine("=== 간단한 계산기 (MVP) ===");

Console.Write("첫 번째 숫자를 입력하세요: ");
string input1 = Console.ReadLine();

Console.Write("두 번째 숫자를 입력하세요: ");
string input2 = Console.ReadLine();

double num1 = double.Parse(input1);
double num2 = double.Parse(input2);

double result = num1 + num2;

Console.WriteLine($"{num1} + {num2} = {result}");
```

**실행 예시:**
```
=== 간단한 계산기 (MVP) ===
첫 번째 숫자를 입력하세요: 10
두 번째 숫자를 입력하세요: 5
10 + 5 = 15
```

**코드 분석 - 숨겨진 복잡성:**

겉보기에 단순한 이 코드는 여러 계층의 추상화를 포함합니다:

1. **Console.ReadLine()**: 
   - OS의 표준 입력 버퍼에서 개행 문자까지 읽기
   - UTF-8/UTF-16 문자 인코딩 처리
   - 블로킹 I/O (입력 대기 시 스레드 일시 정지)

2. **double.Parse()**:
   - 문자열 → 부동소수점 변환 (Lexing + Parsing)
   - 다양한 형식 지원 ("1e10", "1.5", "+123.456" 등)
   - CultureInfo 고려 (소수점 구분자: `.` vs `,`)
   - FormatException 발생 가능

3. **덧셈 연산 (+)**:
   - ALU(Arithmetic Logic Unit)의 부동소수점 덧셈 회로 호출
   - 지수 정렬(Exponent Alignment), 가수 덧셈, 정규화(Normalization)
   - 반올림 모드(Rounding Mode) 적용 (기본: Round to Nearest, Ties to Even)

4. **문자열 보간 ($"...")**:
   - String.Format()의 문법적 설탕(Syntactic Sugar)
   - 각 값의 ToString() 메서드 호출
   - StringBuilder를 통한 효율적 문자열 조합

**이 MVP의 한계 인식:**
- 잘못된 입력 시 `FormatException` 발생 → 프로그램 크래시
- 한 번 계산 후 종료 → 반복 사용 불가
- 덧셈만 지원 → 확장성 없음

이러한 한계를 명확히 인식하는 것이 다음 단계로의 동기부여가 됩니다.

### 다형성을 통한 확장 - 사칙연산 지원

Robert C. Martin의 SOLID 원칙 중 **개방-폐쇄 원칙(Open-Closed Principle)**을 적용합니다: "확장에는 열려있고 수정에는 닫혀있어야 한다." `switch` 문은 이를 간단히 구현합니다:

```csharp
Console.WriteLine("=== 사칙연산 계산기 ===");

Console.Write("첫 번째 숫자: ");
double num1 = double.Parse(Console.ReadLine());

Console.Write("연산자 (+, -, *, /): ");
string op = Console.ReadLine();

Console.Write("두 번째 숫자: ");
double num2 = double.Parse(Console.ReadLine());

double result = 0;

switch (op)
{
    case "+":
        result = num1 + num2;
        break;
    case "-":
        result = num1 - num2;
        break;
    case "*":
        result = num1 * num2;
        break;
    case "/":
        result = num1 / num2;
        break;
    default:
        Console.WriteLine("잘못된 연산자입니다.");
        return;
}

Console.WriteLine($"{num1} {op} {num2} = {result}");
```

**실행 예시:**
```
=== 사칙연산 계산기 ===
첫 번째 숫자: 20
연산자 (+, -, *, /): *
두 번째 숫자: 3
20 * 3 = 60
```

**연산별 주의사항:**

**덧셈(+)**: 
- 큰 수 + 작은 수 시 정밀도 손실 가능
- 예: `1e20 + 1.0 = 1e20` (1.0이 반올림으로 사라짐)

**뺄셈(-)**: 
- **상쇄(Catastrophic Cancellation)** 주의
- 비슷한 크기의 수를 빼면 유효 자릿수 대부분이 소실
- 예: `1.0000001 - 1.0 = 1.0000001 - 1 ≈ 1e-7` (정밀도 크게 저하)

**곱셈(*)**: 
- 오버플로우/언더플로우 가능성
- `double.MaxValue * 2.0 = ∞`
- `double.MinValue * 2.0 = -∞`

**나눗셈(/)**: 
- **0으로 나누기**: `1.0/0.0 = ∞`, `-1.0/0.0 = -∞`, `0.0/0.0 = NaN`
- C#은 정수 나눗셈과 달리 예외를 던지지 않음 (IEEE 754 준수)

### 함수형 프로그래밍과 메서드 분리

David Parnas의 "On the Criteria To Be Used in Decomposing Systems into Modules"(1972)에 따라, 각 연산을 **독립적이고 테스트 가능한 모듈**로 분리합니다. 이는 **단일 책임 원칙(Single Responsibility Principle)**의 적용입니다:

```csharp
double Add(double a, double b)
{
    return a + b;
}

double Subtract(double a, double b)
{
    return a - b;
}

double Multiply(double a, double b)
{
    return a * b;
}

double Divide(double a, double b)
{
    return a / b;
}

// 메인 로직 - C# 8.0 switch 식 사용
Console.Write("첫 번째 숫자: ");
double num1 = double.Parse(Console.ReadLine());

Console.Write("연산자 (+, -, *, /): ");
string op = Console.ReadLine();

Console.Write("두 번째 숫자: ");
double num2 = double.Parse(Console.ReadLine());

double result = op switch
{
    "+" => Add(num1, num2),
    "-" => Subtract(num1, num2),
    "*" => Multiply(num1, num2),
    "/" => Divide(num1, num2),
    _ => throw new InvalidOperationException("잘못된 연산자")
};

Console.WriteLine($"{num1} {op} {num2} = {result}");
```

**Switch 식 vs Switch 문:**

C# 8.0의 **switch 식(Switch Expression)**은 Haskell, F#, Scala 등 함수형 언어의 **패턴 매칭(Pattern Matching)**에서 영감을 받았습니다:

**전통적 switch 문 (명령형)**:
```csharp
double result;
switch (op) {
    case "+": result = a + b; break;
    // ...
}
```

**현대적 switch 식 (함수형)**:
```csharp
double result = op switch {
    "+" => a + b,
    // ...
};
```

차이점:
- **식(Expression)**: 값을 반환하므로 변수 할당이나 return에 직접 사용 가능
- **간결성**: `break` 불필요, 세미콜론 하나로 완결
- **완전성 검사**: 모든 경우를 다루지 않으면 컴파일 경고
- **불변성**: `result` 변수를 재할당하지 않으므로 `readonly` 가능

**고차 함수를 통한 더 나은 추상화:**

함수형 프로그래밍의 **고차 함수(Higher-Order Function)** 개념을 적용하면 더욱 우아한 코드가 가능합니다:

```csharp
// 연산을 함수로 추상화
Func<double, double, double> GetOperation(string op) => op switch
{
    "+" => (a, b) => a + b,
    "-" => (a, b) => a - b,
    "*" => (a, b) => a * b,
    "/" => (a, b) => a / b,
    _ => throw new InvalidOperationException($"Unknown operator: {op}")
};

// 사용
var operation = GetOperation("+");
double result = operation(10, 5);  // 15
```

이는 **전략 패턴(Strategy Pattern)**의 함수형 구현으로, 런타임에 알고리즘을 선택할 수 있게 합니다.

**메서드 분리의 소프트웨어 공학적 이점:**

1. **테스트 용이성(Testability)**: 각 연산을 독립적으로 단위 테스트 가능
   ```csharp
   Assert.Equal(15, Add(10, 5));
   Assert.Equal(5, Subtract(10, 5));
   ```

2. **재사용성(Reusability)**: 다른 컨텍스트에서도 동일한 메서드 사용
   ```csharp
   double CalcGST(double price) => Multiply(price, 1.1);
   ```

3. **가독성(Readability)**: 메서드 이름이 의도를 명확히 전달
   ```csharp
   double tax = Multiply(price, TAX_RATE);  // 의도 명확
   // vs
   double tax = price * TAX_RATE;          // 계산일 뿐
   ```

4. **유지보수성(Maintainability)**: 특정 연산의 로직 변경이 국소화됨
   ```csharp
   double Divide(double a, double b)
   {
       // 나중에 로깅, 검증, 특수 처리 추가 가능
       if (b == 0) throw new DivideByZeroException();
       return a / b;
   }
   ```

5. **문서화(Documentation)**: XML 주석으로 각 메서드 설명 가능
   ```csharp
   /// <summary>
   /// 두 부동소수점 숫자의 합을 계산합니다.
   /// </summary>
   /// <param name="a">첫 번째 피연산자</param>
   /// <param name="b">두 번째 피연산자</param>
   /// <returns>a + b의 결과</returns>
   /// <remarks>
   /// IEEE 754 표준에 따라 반올림 오차가 발생할 수 있습니다.
   /// </remarks>
   double Add(double a, double b) => a + b;
   ```

**성능 고려사항:**

메서드 호출은 오버헤드가 있지만, 현대 JIT 컴파일러는 **인라인(Inline)** 최적화를 수행합니다. 짧은 메서드는 호출 대신 본문이 호출 위치에 직접 삽입되어, 사실상 오버헤드가 0에 가깝습니다:

```csharp
// 소스 코드
double result = Add(10, 5);

// JIT 컴파일 후 (인라인됨)
double result = 10 + 5;
```

이는 Donald Knuth의 "Premature optimization is the root of all evil"의 실천이며, 가독성을 위해 메서드를 분리해도 성능 희생이 없음을 의미합니다.

---

## 19.3 메뉴 시스템 추가

사용자가 여러 작업 중에서 원하는 것을 선택할 수 있도록 메뉴 시스템을 구현합니다. 이는 프로그램의 **사용자 인터페이스(UI)**를 개선하는 중요한 단계입니다.

### 기본 메뉴 구조

메뉴는 일반적으로 무한 루프(`while (true)`)와 `switch` 문을 조합하여 구현합니다:

```csharp
Console.WriteLine("=== 콘솔 계산기 ===");

while (true)
{
    Console.WriteLine("\n메뉴:");
    Console.WriteLine("1. 계산하기");
    Console.WriteLine("2. 종료");
    Console.Write("선택: ");
    
    string choice = Console.ReadLine();
    
    switch (choice)
    {
        case "1":
            PerformCalculation();
            break;
        case "2":
            Console.WriteLine("프로그램을 종료합니다.");
            return;
        default:
            Console.WriteLine("잘못된 선택입니다. 다시 선택해주세요.");
            break;
    }
}

void PerformCalculation()
{
    Console.Write("첫 번째 숫자: ");
    double num1 = double.Parse(Console.ReadLine());
    
    Console.Write("연산자 (+, -, *, /): ");
    string op = Console.ReadLine();
    
    Console.Write("두 번째 숫자: ");
    double num2 = double.Parse(Console.ReadLine());
    
    double result = op switch
    {
        "+" => num1 + num2,
        "-" => num1 - num2,
        "*" => num1 * num2,
        "/" => num1 / num2,
        _ => throw new InvalidOperationException("잘못된 연산자")
    };
    
    Console.WriteLine($"\n결과: {num1} {op} {num2} = {result}");
}
```

**실행 예시:**
```
=== 콘솔 계산기 ===

메뉴:
1. 계산하기
2. 종료
선택: 1
첫 번째 숫자: 15
연산자 (+, -, *, /): -
두 번째 숫자: 7

결과: 15 - 7 = 8

메뉴:
1. 계산하기
2. 종료
선택: 2
프로그램을 종료합니다.
```

### 확장된 메뉴 - 여러 기능 추가

계산 외에 다른 기능도 추가해봅니다:

```csharp
Console.WriteLine("=== 고급 계산기 ===");

while (true)
{
    Console.WriteLine("\n━━━━━━━━━━━━━━━━━━━━━━");
    Console.WriteLine("  1. 사칙연산");
    Console.WriteLine("  2. 제곱 계산");
    Console.WriteLine("  3. 제곱근 계산");
    Console.WriteLine("  4. 도움말");
    Console.WriteLine("  5. 종료");
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━");
    Console.Write("선택 (1-5): ");
    
    string choice = Console.ReadLine();
    
    switch (choice)
    {
        case "1":
            BasicCalculation();
            break;
        case "2":
            PowerCalculation();
            break;
        case "3":
            SqrtCalculation();
            break;
        case "4":
            ShowHelp();
            break;
        case "5":
            Console.WriteLine("\n프로그램을 종료합니다. 감사합니다!");
            return;
        default:
            Console.WriteLine("\n❌ 잘못된 선택입니다. 1-5 사이의 숫자를 입력하세요.");
            break;
    }
}

void BasicCalculation()
{
    Console.WriteLine("\n[ 사칙연산 ]");
    Console.Write("첫 번째 숫자: ");
    double a = double.Parse(Console.ReadLine());
    
    Console.Write("연산자 (+, -, *, /): ");
    string op = Console.ReadLine();
    
    Console.Write("두 번째 숫자: ");
    double b = double.Parse(Console.ReadLine());
    
    double result = op switch
    {
        "+" => a + b,
        "-" => a - b,
        "*" => a * b,
        "/" => a / b,
        _ => throw new InvalidOperationException("지원하지 않는 연산자")
    };
    
    Console.WriteLine($"✓ 결과: {a} {op} {b} = {result}");
}

void PowerCalculation()
{
    Console.WriteLine("\n[ 제곱 계산 ]");
    Console.Write("밑(base): ");
    double baseNum = double.Parse(Console.ReadLine());
    
    Console.Write("지수(exponent): ");
    double exp = double.Parse(Console.ReadLine());
    
    double result = Math.Pow(baseNum, exp);
    Console.WriteLine($"✓ 결과: {baseNum}^{exp} = {result}");
}

void SqrtCalculation()
{
    Console.WriteLine("\n[ 제곱근 계산 ]");
    Console.Write("숫자: ");
    double num = double.Parse(Console.ReadLine());
    
    double result = Math.Sqrt(num);
    Console.WriteLine($"✓ 결과: √{num} = {result}");
}

void ShowHelp()
{
    Console.WriteLine("\n[ 도움말 ]");
    Console.WriteLine("이 계산기는 다음 기능을 제공합니다:");
    Console.WriteLine("• 사칙연산: 덧셈(+), 뺄셈(-), 곱셈(*), 나눗셈(/)");
    Console.WriteLine("• 제곱: 밑과 지수를 입력하여 거듭제곱 계산");
    Console.WriteLine("• 제곱근: 양수의 제곱근 계산");
    Console.WriteLine("\n💡 팁: 소수점 숫자도 입력할 수 있습니다!");
}
```

**메뉴 설계의 원칙:**
- **일관성**: 메뉴 번호와 기능이 논리적으로 연결됩니다
- **명확성**: 각 메뉴 항목이 무엇을 하는지 분명합니다
- **피드백**: 사용자의 선택에 대한 결과를 즉시 보여줍니다
- **오류 처리**: 잘못된 입력에 대한 안내 메시지를 제공합니다

---

## 19.4 예외 처리 및 유효성 검증

실제 프로그램에서는 사용자가 예상치 못한 입력을 할 수 있습니다. **방어적 프로그래밍(Defensive Programming)**을 통해 모든 예외 상황을 처리해야 합니다.

### 숫자 입력의 유효성 검증

`double.Parse()`는 숫자가 아닌 문자열이 입력되면 예외를 발생시킵니다. `TryParse()`를 사용하여 안전하게 처리합니다:

```csharp
double GetNumberInput(string prompt)
{
    while (true)
    {
        Console.Write(prompt);
        string input = Console.ReadLine();
        
        if (double.TryParse(input, out double result))
        {
            return result;
        }
        
        Console.WriteLine("❌ 올바른 숫자를 입력하세요.");
    }
}

// 사용 예시
double num1 = GetNumberInput("첫 번째 숫자: ");
double num2 = GetNumberInput("두 번째 숫자: ");
```

**실행 예시:**
```
첫 번째 숫자: abc
❌ 올바른 숫자를 입력하세요.
첫 번째 숫자: 12.5
두 번째 숫자: 3
```

### 0으로 나누기 방지

나눗셈 연산 시 0으로 나누는 경우를 반드시 검사해야 합니다:

```csharp
double SafeDivide(double a, double b)
{
    if (b == 0)
    {
        Console.WriteLine("❌ 오류: 0으로 나눌 수 없습니다.");
        return 0; // 또는 특별한 값 반환
    }
    return a / b;
}

// 또는 예외를 발생시키는 방식
double Divide(double a, double b)
{
    if (b == 0)
    {
        throw new DivideByZeroException("0으로 나눌 수 없습니다.");
    }
    return a / b;
}
```

### 종합적인 예외 처리

모든 연산 과정에서 발생할 수 있는 예외를 처리합니다:

```csharp
void PerformSafeCalculation()
{
    try
    {
        Console.WriteLine("\n[ 계산하기 ]");
        
        double num1 = GetNumberInput("첫 번째 숫자: ");
        
        Console.Write("연산자 (+, -, *, /): ");
        string op = Console.ReadLine()?.Trim();
        
        if (string.IsNullOrEmpty(op))
        {
            Console.WriteLine("❌ 연산자를 입력해주세요.");
            return;
        }
        
        double num2 = GetNumberInput("두 번째 숫자: ");
        
        // 0으로 나누기 검사
        if (op == "/" && num2 == 0)
        {
            Console.WriteLine("❌ 오류: 0으로 나눌 수 없습니다.");
            return;
        }
        
        double result = op switch
        {
            "+" => num1 + num2,
            "-" => num1 - num2,
            "*" => num1 * num2,
            "/" => num1 / num2,
            _ => throw new InvalidOperationException($"지원하지 않는 연산자: {op}")
        };
        
        Console.WriteLine($"✓ 결과: {num1} {op} {num2} = {result}");
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine($"❌ 오류: {ex.Message}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"❌ 예상치 못한 오류: {ex.Message}");
    }
}

double GetNumberInput(string prompt)
{
    while (true)
    {
        Console.Write(prompt);
        string input = Console.ReadLine();
        
        if (double.TryParse(input, out double result))
        {
            return result;
        }
        
        Console.WriteLine("❌ 올바른 숫자를 입력하세요.");
    }
}
```

**예외 처리의 계층:**
1. **예방(Prevention)**: 입력 검증으로 예외가 발생하지 않도록 함
2. **감지(Detection)**: 예외 발생 시 적절히 포착
3. **복구(Recovery)**: 사용자가 작업을 계속할 수 있도록 함
4. **보고(Reporting)**: 명확한 오류 메시지 제공

---

## 19.5 연산 기록 기능

이전 계산 결과를 저장하고 조회하는 기능을 추가하면 계산기가 더욱 유용해집니다. **리스트(List<T>)**를 사용하여 기록을 관리합니다.

### 계산 기록 저장

각 계산 결과를 문자열로 저장합니다:

```csharp
List<string> history = new List<string>();

void PerformCalculationWithHistory()
{
    Console.WriteLine("\n[ 계산하기 ]");
    
    double num1 = GetNumberInput("첫 번째 숫자: ");
    
    Console.Write("연산자 (+, -, *, /): ");
    string op = Console.ReadLine()?.Trim();
    
    double num2 = GetNumberInput("두 번째 숫자: ");
    
    if (op == "/" && num2 == 0)
    {
        Console.WriteLine("❌ 0으로 나눌 수 없습니다.");
        return;
    }
    
    double result = op switch
    {
        "+" => num1 + num2,
        "-" => num1 - num2,
        "*" => num1 * num2,
        "/" => num1 / num2,
        _ => throw new InvalidOperationException("지원하지 않는 연산자")
    };
    
    // 기록에 추가
    string record = $"{num1} {op} {num2} = {result}";
    history.Add(record);
    
    Console.WriteLine($"✓ 결과: {record}");
}
```

### 기록 조회 기능

저장된 모든 계산 기록을 보여줍니다:

```csharp
void ShowHistory()
{
    Console.WriteLine("\n[ 계산 기록 ]");
    
    if (history.Count == 0)
    {
        Console.WriteLine("기록이 없습니다.");
        return;
    }
    
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━");
    for (int i = 0; i < history.Count; i++)
    {
        Console.WriteLine($"{i + 1}. {history[i]}");
    }
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━");
    Console.WriteLine($"총 {history.Count}개의 기록");
}
```

### 기록 관리 기능

기록을 지우는 기능도 추가합니다:

```csharp
void ClearHistory()
{
    Console.Write("\n정말로 모든 기록을 삭제하시겠습니까? (y/n): ");
    string confirm = Console.ReadLine()?.ToLower();
    
    if (confirm == "y" || confirm == "yes")
    {
        history.Clear();
        Console.WriteLine("✓ 모든 기록이 삭제되었습니다.");
    }
    else
    {
        Console.WriteLine("취소되었습니다.");
    }
}
```

### 완전한 계산기 프로그램

모든 기능을 통합한 최종 버전:

```csharp
// 전역 변수
List<string> history = new List<string>();

// 메인 프로그램
Console.WriteLine("╔═══════════════════════════╗");
Console.WriteLine("║   콘솔 계산기 v1.0       ║");
Console.WriteLine("╚═══════════════════════════╝");

while (true)
{
    Console.WriteLine("\n메뉴:");
    Console.WriteLine("  1. 계산하기");
    Console.WriteLine("  2. 계산 기록 보기");
    Console.WriteLine("  3. 기록 삭제");
    Console.WriteLine("  4. 도움말");
    Console.WriteLine("  5. 종료");
    Console.Write("선택: ");
    
    string choice = Console.ReadLine();
    
    switch (choice)
    {
        case "1":
            PerformCalculationWithHistory();
            break;
        case "2":
            ShowHistory();
            break;
        case "3":
            ClearHistory();
            break;
        case "4":
            ShowHelp();
            break;
        case "5":
            Console.WriteLine("\n계산기를 종료합니다. 안녕히 가세요!");
            return;
        default:
            Console.WriteLine("❌ 1-5 사이의 숫자를 선택하세요.");
            break;
    }
}

// 메서드들
void PerformCalculationWithHistory()
{
    try
    {
        Console.WriteLine("\n[ 계산하기 ]");
        
        double num1 = GetNumberInput("첫 번째 숫자: ");
        
        Console.Write("연산자 (+, -, *, /): ");
        string op = Console.ReadLine()?.Trim();
        
        if (string.IsNullOrEmpty(op) || !(op == "+" || op == "-" || op == "*" || op == "/"))
        {
            Console.WriteLine("❌ 올바른 연산자를 입력하세요 (+, -, *, /)");
            return;
        }
        
        double num2 = GetNumberInput("두 번째 숫자: ");
        
        if (op == "/" && num2 == 0)
        {
            Console.WriteLine("❌ 0으로 나눌 수 없습니다.");
            return;
        }
        
        double result = op switch
        {
            "+" => num1 + num2,
            "-" => num1 - num2,
            "*" => num1 * num2,
            "/" => num1 / num2,
            _ => 0
        };
        
        string record = $"{num1} {op} {num2} = {result}";
        history.Add(record);
        
        Console.WriteLine($"✓ {record}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"❌ 오류: {ex.Message}");
    }
}

double GetNumberInput(string prompt)
{
    while (true)
    {
        Console.Write(prompt);
        string input = Console.ReadLine();
        
        if (double.TryParse(input, out double result))
        {
            return result;
        }
        
        Console.WriteLine("❌ 올바른 숫자를 입력하세요.");
    }
}

void ShowHistory()
{
    Console.WriteLine("\n[ 계산 기록 ]");
    
    if (history.Count == 0)
    {
        Console.WriteLine("📝 아직 계산 기록이 없습니다.");
        return;
    }
    
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━━━━━━━━");
    for (int i = 0; i < history.Count; i++)
    {
        Console.WriteLine($"  {i + 1}. {history[i]}");
    }
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━━━━━━━━");
    Console.WriteLine($"📊 총 {history.Count}개의 계산 기록");
}

void ClearHistory()
{
    if (history.Count == 0)
    {
        Console.WriteLine("\n삭제할 기록이 없습니다.");
        return;
    }
    
    Console.Write($"\n정말로 {history.Count}개의 기록을 모두 삭제하시겠습니까? (y/n): ");
    string confirm = Console.ReadLine()?.ToLower();
    
    if (confirm == "y" || confirm == "yes")
    {
        history.Clear();
        Console.WriteLine("✓ 모든 기록이 삭제되었습니다.");
    }
    else
    {
        Console.WriteLine("취소되었습니다.");
    }
}

void ShowHelp()
{
    Console.WriteLine("\n[ 도움말 ]");
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━");
    Console.WriteLine("📖 콘솔 계산기 사용 방법");
    Console.WriteLine();
    Console.WriteLine("지원하는 연산:");
    Console.WriteLine("  • 덧셈 (+)");
    Console.WriteLine("  • 뺄셈 (-)");
    Console.WriteLine("  • 곱셈 (*)");
    Console.WriteLine("  • 나눗셈 (/)");
    Console.WriteLine();
    Console.WriteLine("기능:");
    Console.WriteLine("  • 계산 결과는 자동으로 기록에 저장됩니다");
    Console.WriteLine("  • 소수점 숫자를 입력할 수 있습니다");
    Console.WriteLine("  • 음수를 입력할 수 있습니다");
    Console.WriteLine();
    Console.WriteLine("💡 팁: 잘못 입력한 경우 다시 입력할 수 있습니다");
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━");
}
```

**실행 예시:**
```
╔═══════════════════════════╗
║   콘솔 계산기 v1.0       ║
╚═══════════════════════════╝

메뉴:
  1. 계산하기
  2. 계산 기록 보기
  3. 기록 삭제
  4. 도움말
  5. 종료
선택: 1

[ 계산하기 ]
첫 번째 숫자: 100
연산자 (+, -, *, /): /
두 번째 숫자: 4
✓ 100 / 4 = 25

메뉴:
  1. 계산하기
  2. 계산 기록 보기
  3. 기록 삭제
  4. 도움말
  5. 종료
선택: 2

[ 계산 기록 ]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1. 100 / 4 = 25
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 총 1개의 계산 기록
```

---

## 19장 정리 및 요약

이 장에서는 지금까지 배운 C#의 다양한 개념들을 통합하여 실용적인 콘솔 계산기를 만들어 보았습니다.

### 핵심 개념 정리

**1. 요구사항 분석**
- 프로그램을 만들기 전에 무엇을 만들지 명확히 정의
- 큰 문제를 작은 단계로 나누어 접근 (증분 개발)
- 의사코드와 순서도로 흐름 설계

**2. 기본 연산 구현**
- 사용자 입력을 받아 처리 (`Console.ReadLine()`, `double.Parse()`)
- `switch` 문과 switch 식으로 연산자 처리
- 메서드로 기능을 분리하여 가독성과 재사용성 향상

**3. 메뉴 시스템**
- `while (true)` 루프로 반복 실행
- 사용자 선택에 따라 다른 기능 실행
- 명확하고 직관적인 인터페이스 제공

**4. 예외 처리**
- `TryParse()`로 안전한 입력 검증
- 0으로 나누기 등 특수 상황 처리
- `try-catch`로 예외를 잡아 프로그램 안정성 확보

**5. 연산 기록**
- `List<string>`으로 계산 결과 저장
- 기록 조회 및 관리 기능
- 사용자 확인 메시지로 실수 방지

### 프로그래밍 원칙

이 프로젝트를 통해 배운 중요한 프로그래밍 원칙:

**단순함에서 복잡함으로 (Incremental Development)**
- 작동하는 간단한 버전부터 시작
- 한 번에 하나의 기능씩 추가
- 각 단계마다 테스트

**방어적 프로그래밍 (Defensive Programming)**
- 사용자가 무엇을 입력할지 예측 불가
- 모든 입력을 검증
- 오류 발생 시 명확한 메시지 제공

**코드 구조화 (Code Organization)**
- 메서드로 기능 분리
- 관련된 코드를 함께 배치
- 의미 있는 이름 사용

**사용자 경험 (User Experience)**
- 직관적인 메뉴 구조
- 명확한 안내 메시지
- 오류 시 재시도 기회 제공

### 확장 아이디어

이 계산기를 더욱 발전시킬 수 있는 방법:

**1. 더 많은 연산 추가**
- 제곱, 제곱근, 삼각함수
- 팩토리얼, 조합, 순열
- 단위 변환 (온도, 길이, 무게 등)

**2. 기록 관리 개선**
- 파일에 기록 저장 및 불러오기 (13장 참고)
- 특정 기록 삭제
- 기록 검색 및 필터링

**3. 고급 기능**
- 수식 파싱 (예: "2 + 3 * 4" 한 번에 계산)
- 변수 저장 및 사용 (예: "x = 10", "x + 5")
- 그래프 그리기 (ASCII 아트)

**4. 사용자 경험 개선**
- 색상 있는 출력 (`Console.ForegroundColor`)
- 통계 정보 (평균, 최댓값, 최솟값)
- 테마 및 설정 저장

### 다음 단계

이제 여러분은 실용적인 프로그램을 만들 수 있는 기초를 다졌습니다. 다음 장에서는 더 복잡한 프로젝트인 **할 일 관리 앱**을 만들어 보며, 파일 입출력, 데이터 모델, LINQ 등을 활용하는 방법을 배울 것입니다.

**실습 과제:**

1. 계산기에 새로운 연산(제곱, 나머지 등)을 추가해보세요
2. 계산 기록을 파일에 저장하고 불러오는 기능을 구현해보세요
3. 사용자가 연속으로 계산할 수 있도록 "이전 결과 사용" 기능을 추가해보세요
4. 잘못된 입력에 대한 더 상세한 오류 메시지를 추가해보세요

이 장에서 만든 계산기는 여러분의 첫 번째 완전한 프로그램입니다. 작은 시작이지만, 여기서 배운 원칙들은 앞으로 만들 모든 프로그램의 기초가 될 것입니다!

---

## 마무리: 소프트웨어 공학의 통합적 이해

이 장에서 만든 콘솔 계산기는 단순한 프로그래밍 연습을 넘어서, **소프트웨어 공학(Software Engineering)**의 핵심 원칙들이 실제 코드로 구현되는 과정을 보여주었습니다. Fred Brooks가 "The Mythical Man-Month"에서 언급한 "프로그래밍의 본질은 복잡성의 관리"라는 명제를 직접 경험했습니다.

**계산기에서 배운 보편적 원칙들:**

### 1. 추상화 계층(Layers of Abstraction)

우리의 계산기는 여러 추상화 계층으로 구성되어 있습니다:

```
사용자 인터페이스 계층 (메뉴, 프롬프트)
    ↓
비즈니스 로직 계층 (연산 선택, 기록 관리)
    ↓
계산 엔진 계층 (사칙연산 메서드)
    ↓
플랫폼 계층 (Console I/O, IEEE 754 FPU)
    ↓
하드웨어 계층 (ALU, 메모리, CPU)
```

각 계층은 하위 계층의 세부사항을 숨기고(Information Hiding), 상위 계층에 간단한 인터페이스를 제공합니다. 이는 Edsger Dijkstra의 "Separation of Concerns" 원칙을 실천한 것입니다.

### 2. 소프트웨어 패턴의 실전 적용

우리가 구현한 패턴들은 Gang of Four의 디자인 패턴 카탈로그와 Martin Fowler의 엔터프라이즈 패턴의 단순화된 형태입니다:

- **REPL 패턴**: Python, Ruby, Lisp 인터프리터의 핵심
- **명령 패턴(Command Pattern)**: 메뉴 선택이 명령 객체로 추상화됨
- **전략 패턴(Strategy Pattern)**: 연산자에 따라 알고리즘 선택
- **저장소 패턴(Repository Pattern)**: `List<string>`이 데이터 저장소
- **파사드 패턴(Facade Pattern)**: 메뉴가 복잡한 기능을 단순화

이러한 패턴들은 **재사용 가능한 해결책(Reusable Solutions)**으로, 다른 프로젝트에도 적용할 수 있습니다.

### 3. 계산 이론과 실제

Alan Turing의 튜링 기계는 추상적 계산 모델이었지만, 우리의 계산기는 이를 구체화한 것입니다. 계산 가능성 이론(Computability Theory)에서:

- **입력(Input)**: 사용자가 제공하는 숫자와 연산자
- **상태(State)**: 프로그램 변수와 연산 기록
- **전이 함수(Transition Function)**: 메뉴 선택에 따른 상태 변화
- **출력(Output)**: 계산 결과와 메시지
- **정지 조건(Halting Condition)**: 사용자가 종료를 선택

우리의 계산기는 **결정론적 유한 상태 기계(Deterministic Finite State Machine)**로 모델링할 수 있으며, 이는 모든 소프트웨어의 이론적 기반입니다.

### 4. 품질 속성(Quality Attributes)의 균형

소프트웨어 아키텍처는 Len Bass의 "Software Architecture in Practice"에서 제시한 품질 속성들 간의 트레이드오프입니다:

| 품질 속성 | 우리의 구현 | 설계 결정 |
|----------|------------|---------|
| **사용성(Usability)** | 높음 | 명확한 메뉴, 즉각적 피드백 |
| **신뢰성(Reliability)** | 높음 | TryParse, 예외 처리, 입력 검증 |
| **성능(Performance)** | 매우 높음 | O(1) 계산, 인라인 최적화 |
| **유지보수성(Maintainability)** | 높음 | 모듈화된 메서드, 명확한 책임 분리 |
| **확장성(Extensibility)** | 중간 | Switch 문으로 연산 추가 용이, 파일 I/O 확장 가능 |
| **이식성(Portability)** | 매우 높음 | .NET의 크로스 플랫폼 지원 |
| **보안(Security)** | 높음 | 입력 검증, 예외 처리, 최소 권한 |

### 5. 테스트 전략과 품질 보증

Kent Beck의 TDD와 Martin Fowler의 리팩토링 원칙을 적용하면, 우리 계산기의 각 부분을 테스트할 수 있습니다:

```csharp
// 단위 테스트 (Unit Test)
[Fact]
public void Add_TwoPositiveNumbers_ReturnsCorrectSum()
{
    // Arrange (준비)
    double a = 10.0;
    double b = 5.0;
    
    // Act (실행)
    double result = Add(a, b);
    
    // Assert (검증)
    Assert.Equal(15.0, result);
}

// 경계값 테스트 (Boundary Test)
[Fact]
public void Divide_ByZero_ReturnsInfinity()
{
    double result = Divide(1.0, 0.0);
    Assert.True(double.IsInfinity(result));
}

// 통합 테스트 (Integration Test)
[Fact]
public void Calculator_FullWorkflow_Success()
{
    // 메뉴 선택 → 계산 → 기록 저장 → 기록 조회
    // 전체 시나리오 검증
}
```

### 6. 성능 분석과 최적화

Donald Knuth의 "The Art of Computer Programming"에서 강조한 성능 분석을 우리 계산기에 적용하면:

**시간 복잡도(Time Complexity)**:
- 사칙연산: O(1) - 단일 CPU 명령
- 메뉴 표시: O(1) - 고정 문자열 출력
- 기록 추가: O(1) amortized - List의 동적 확장
- 기록 조회: O(n) - n개 항목 순회
- 전체 프로그램: O(m × n) - m번 상호작용, 각각 최대 n개 기록 조회

**공간 복잡도(Space Complexity)**:
- 기록 저장: O(n) - n개 계산 결과
- 스택 메모리: O(1) - 재귀 없음
- 전체: O(n) linear

**최적화 기회**:
- **캐싱(Caching)**: 자주 사용되는 계산 결과 저장
- **메모이제이션(Memoization)**: 피보나치처럼 중복 계산 방지
- **지연 평가(Lazy Evaluation)**: 필요할 때만 기록 포맷팅
- **병렬화(Parallelization)**: 여러 계산을 동시 수행 (PLINQ)

하지만 Knuth의 "Premature optimization is the root of all evil"에 따라, 현재 성능이 충분하므로 최적화는 불필요합니다.

### 7. 확장 가능성과 진화 경로

Robert C. Martin의 SOLID 원칙을 따랐으므로, 계산기는 다음과 같이 확장할 수 있습니다:

**단기 확장**:
- 과학 계산기 모드: 삼각함수, 로그, 지수
- 단위 변환: 온도, 길이, 무게, 시간
- 통계 기능: 평균, 중앙값, 표준편차
- 복소수 연산: a+bi 형식 지원

**중기 확장**:
- 수식 파싱: "2 + 3 * (4 - 1)" 같은 중위 표기법
  - Shunting-yard 알고리즘 (Dijkstra)
  - 재귀 하강 파서(Recursive Descent Parser)
- 변수 저장: "x = 10", "y = x + 5"
  - 심볼 테이블(Symbol Table)
  - 스코프(Scope) 관리
- 파일 저장: 기록을 CSV/JSON으로 내보내기 (13장 활용)

**장기 확장**:
- GUI 버전: WPF, Windows Forms, Avalonia
  - MVVM 패턴 적용
- 웹 버전: ASP.NET Core, Blazor
  - RESTful API 설계
- 모바일 앱: .NET MAUI
  - 터치 인터페이스 최적화
- 프로그래밍 언어: 자체 DSL(Domain-Specific Language) 개발
  - 렉서(Lexer), 파서(Parser), 인터프리터(Interpreter)

### 8. 컴퓨터 과학 원리의 통합

이 계산기 프로젝트는 컴퓨터 과학의 여러 분야를 통합합니다:

- **이론 컴퓨터 과학**: 오토마타 이론, 계산 가능성
- **알고리즘**: 시간/공간 복잡도 분석
- **데이터 구조**: List, Stack(호출 스택)
- **프로그래밍 언어**: 구문론, 의미론, 타입 시스템
- **소프트웨어 공학**: 요구사항, 설계, 테스트, 유지보수
- **인간-컴퓨터 상호작용**: 사용성, 접근성, 피드백
- **수치 해석**: 부동소수점, 오차 분석

### 9. 전문가로의 여정

Peter Norvig는 "Teach Yourself Programming in Ten Years"에서 전문성은 시간과 의도적 연습이 필요하다고 강조했습니다. 이 계산기는 그 여정의 첫걸음입니다:

**초급 단계 (현재)**:
- 기본 문법과 제어 구조 이해
- 메서드와 모듈화 개념
- 단순한 예외 처리

**중급 단계 (다음 목표)**:
- 객체지향 설계 (20-21장)
- 복잡한 데이터 구조 (Tree, Graph)
- 디자인 패턴의 체계적 적용

**고급 단계 (장기 목표)**:
- 아키텍처 설계 (마이크로서비스, 이벤트 주도)
- 함수형 프로그래밍 (모나드, 펑터)
- 분산 시스템 (CAP 정리, 합의 알고리즘)

**전문가 단계 (경력 개발)**:
- 도메인 전문성 (금융, 의료, 게임 등)
- 리더십과 아키텍처 결정
- 오픈소스 기여와 커뮤니티 참여

### 10. 철학적 성찰

Dijkstra는 "프로그래밍은 어떻게 생각하는지를 배우는 것"이라고 했습니다. 이 계산기 프로젝트를 통해:

- **분해(Decomposition)**: 복잡한 문제를 작은 부분으로 나누기
- **패턴 인식(Pattern Recognition)**: 재사용 가능한 구조 발견
- **추상화(Abstraction)**: 세부사항을 숨기고 본질만 드러내기
- **알고리즘적 사고(Algorithmic Thinking)**: 단계별 해결책 설계

이러한 **계산적 사고(Computational Thinking)**는 Jeannette Wing이 2006년 제안한 개념으로, 21세기의 필수 역량입니다. 프로그래밍을 넘어서 문제 해결, 시스템 설계, 인간의 행동 이해에도 적용됩니다.

---

## 다음 장 예고

20장 "할 일 관리 앱"에서는 계산기보다 복잡한 **CRUD(Create, Read, Update, Delete)** 애플리케이션을 만듭니다. 파일 영속성(13장), LINQ 쿼리(14-15장), 그리고 객체지향 설계를 통합하여, 실무에 가까운 애플리케이션을 개발합니다.

여러분은 이제 **프로그래머(Programmer)**에서 **소프트웨어 엔지니어(Software Engineer)**로 진화하는 중요한 단계를 넘었습니다. 작은 계산기 프로젝트지만, 여기서 배운 원칙들은 대규모 시스템에도 동일하게 적용됩니다. 

Brooks가 말했듯이, "소프트웨어 개발에 은탄환(Silver Bullet)은 없습니다." 하지만 견고한 기초, 명확한 원칙, 그리고 지속적인 학습이 있다면, 어떤 복잡한 시스템도 만들 수 있습니다.

**"The only way to learn a new programming language is by writing programs in it." - Dennis Ritchie**

계속해서 코드를 작성하고, 실수하고, 배우고, 개선하세요. 그것이 마스터로 가는 유일한 길입니다.
