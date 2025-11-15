# 24장. 단위 테스트

단위 테스트(Unit Testing)는 현대 소프트웨어 공학의 필수 기반으로, 소프트웨어의 가장 작은 테스트 가능한 단위를 독립적으로 검증하여 코드의 품질과 신뢰성을 보장하는 체계적인 실천 방법입니다. Kent Beck이 1990년대 후반 Extreme Programming(XP)의 일환으로 대중화한 이래, 단위 테스트는 테스트 주도 개발(TDD, Test-Driven Development), 지속적 통합(CI, Continuous Integration), 그리고 DevOps 문화의 핵심 요소로 자리잡았습니다.

**단위 테스트의 역사적 진화:**

소프트웨어 테스팅의 개념은 1950년대 초기 컴퓨터 시대부터 존재했지만, 체계적인 단위 테스트 방법론의 발전은 다음과 같은 주요 이정표를 거쳤습니다:

- **1970년대**: Glenford Myers의 "The Art of Software Testing" 출간으로 테스팅 이론 정립
- **1989년**: Kent Beck이 Smalltalk용 SUnit 프레임워크 개발 (최초의 xUnit 계열)
- **1997년**: Kent Beck과 Erich Gamma가 JUnit 발표로 Java 커뮤니티에 단위 테스트 확산
- **2002년**: .NET 플랫폼에 NUnit 도입으로 C# 테스팅 생태계 형성
- **2004년**: Kent Beck의 "Test-Driven Development: By Example" 출간으로 TDD 대중화
- **2007년**: Brad Wilson과 Jim Newkirk이 xUnit.net 프로젝트 시작
- **2016년**: .NET Core 등장과 함께 xUnit이 Microsoft 공식 권장 프레임워크로 부상

xUnit.net의 탄생은 NUnit의 설계 한계를 극복하고자 하는 목적에서 시작되었습니다. Brad Wilson과 Jim Newkirk(NUnit의 원래 작성자 중 한 명)은 .NET의 현대적 기능을 활용하고, 확장 가능한 아키텍처를 제공하며, 병렬 테스트 실행을 최적화한 새로운 프레임워크를 설계했습니다. 이러한 혁신은 xUnit을 ASP.NET Core, Entity Framework Core 등 Microsoft의 주요 오픈소스 프로젝트에서 채택되는 표준 테스트 프레임워크로 만들었습니다.

**단위 테스트와 소프트웨어 공학 원칙:**

단위 테스트는 여러 소프트웨어 공학 원칙과 깊이 연관되어 있습니다:

- **SOLID 원칙의 실현**: 단일 책임 원칙(SRP)을 따르는 작은 단위들은 테스트하기 쉽습니다. 의존성 역전 원칙(DIP)은 Mock 객체를 통한 테스트 격리를 가능하게 합니다.

- **관심사의 분리(Separation of Concerns)**: 비즈니스 로직, 데이터 접근, UI를 분리하면 각 계층을 독립적으로 테스트할 수 있습니다.

- **계약에 의한 설계(Design by Contract)**: 테스트는 메서드의 사전조건(precondition)과 사후조건(postcondition)을 명시적으로 검증합니다.

- **실패 방지 설계(Fail-Fast Design)**: 테스트는 문제를 조기에 발견하여 디버깅 비용을 줄입니다.

**단위 테스트의 경제적 가치:**

IBM과 NIST(National Institute of Standards and Technology)의 연구에 따르면, 소프트웨어 결함의 수정 비용은 발견 시점에 따라 기하급수적으로 증가합니다:

- **개발 단계**: 1x (기준 비용)
- **통합 테스트 단계**: 10x
- **시스템 테스트 단계**: 15x
- **운영 환경**: 100x 이상

Microsoft의 내부 연구에 따르면, Visual Studio 팀에서 단위 테스트를 도입한 후 프로덕션 버그가 60-90% 감소했으며, Google은 전체 코드베이스의 테스트 커버리지를 75% 이상 유지하여 높은 소프트웨어 품질을 달성하고 있습니다.

**테스트 주도 개발(TDD)의 부상:**

Kent Beck의 "Red-Green-Refactor" 사이클은 현대 소프트웨어 개발의 기본 워크플로우가 되었습니다:

1. **Red**: 실패하는 테스트 작성 (요구사항 명확화)
2. **Green**: 테스트를 통과하는 최소한의 코드 작성 (구현)
3. **Refactor**: 코드 개선 (품질 향상)

이 접근 방식은 다음과 같은 이점을 제공합니다:

- **요구사항 우선 설계**: 테스트가 인터페이스 설계를 주도
- **과도한 엔지니어링 방지**: 필요한 기능만 구현
- **지속적인 피드백**: 몇 초마다 코드 검증
- **리팩토링 자신감**: 테스트가 안전망 제공

이 장에서는 .NET 생태계에서 가장 널리 사용되는 xUnit 프레임워크를 통해 단위 테스트의 기초부터 고급 기법까지 체계적으로 학습합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 단위 테스트의 이론적 기반부터 실무 적용까지 포괄적으로 습득하게 됩니다:

- **단위 테스트의 철학과 역사**: Kent Beck의 SUnit에서 시작된 xUnit 계열 프레임워크의 진화, 테스트 주도 개발(TDD)의 탄생 배경, 그리고 현대 소프트웨어 개발에서 단위 테스트가 차지하는 핵심적 위치를 이해합니다.

- **xUnit.net 아키텍처와 생태계**: .NET Core 시대의 표준 테스트 프레임워크로 자리잡은 xUnit의 설계 철학, 확장 메커니즘, 그리고 Microsoft의 주요 프로젝트들이 xUnit을 선택한 이유를 깊이 있게 탐구합니다.

- **테스트 작성의 예술과 과학**: Arrange-Act-Assert 패턴의 이론적 근거, FIRST 원칙(Fast, Independent, Repeatable, Self-validating, Timely), 그리고 테스트 더블(Test Double)의 다양한 형태를 실전 예제와 함께 학습합니다.

- **매개변수화된 테스트와 데이터 주도 테스트**: xUnit의 Theory와 데이터 제공 메커니즘을 활용하여 코드 중복 없이 포괄적인 테스트 케이스를 작성하는 고급 기법을 익힙니다.

- **Mock 객체와 의존성 격리**: Moq 라이브러리의 내부 동작 원리, 동적 프록시 생성 메커니즘, 그리고 테스트 가능한 아키텍처 설계 전략을 실무 시나리오를 통해 체득합니다.

- **테스트 커버리지와 품질 메트릭**: 코드 커버리지 도구 활용, 의미 있는 테스트 작성, 그리고 테스트 스위트 유지보수 전략을 배웁니다.

**학습 목표:**

- xUnit.net의 아키텍처와 확장 메커니즘 이해
- AAA 패턴과 FIRST 원칙을 적용한 고품질 테스트 작성
- 매개변수화된 테스트로 효율적인 테스트 스위트 구축
- Moq를 활용한 의존성 격리와 테스트 더블 패턴 습득
- 테스트 주도 개발(TDD) 워크플로우 실천
- 테스트 가능한 코드 설계 원칙 적용

---

## 24.1 단위 테스트란?

단위 테스트는 소프트웨어의 가장 작은 테스트 가능한 단위를 독립적으로 검증하는 자동화된 테스트 방법입니다. Michael Feathers의 정의에 따르면, "단위 테스트는 코드의 한 단위를 실행하고, 그 결과가 예상과 일치하는지 확인하는 코드"입니다. 일반적으로 "단위"는 하나의 메서드나 함수를 의미하지만, 문맥에 따라 작은 규모의 클래스 전체를 단위로 볼 수도 있습니다.

**단위 테스트의 철학적 기반:**

Martin Fowler는 단위 테스트를 "격리 테스트(Solitary Test)"와 "사회적 테스트(Sociable Test)" 두 가지 철학으로 구분합니다. 격리 테스트는 모든 협력 객체를 Test Double로 대체하여 테스트 대상만을 순수하게 검증하는 반면, 사회적 테스트는 실제 협력 객체와 함께 동작을 검증합니다. 두 접근 방식 모두 장단점이 있으며, 상황에 따라 적절히 선택해야 합니다.

### 단위 테스트의 핵심 개념

**테스트 가능한 단위의 정의:**

C#과 .NET 환경에서 단위 테스트의 대상이 되는 "단위"는 다음과 같이 분류됩니다:

- **메서드(Methods)**: 클래스의 public 메서드가 주요 테스트 대상입니다. 비즈니스 로직을 캡슐화한 메서드는 입력과 출력이 명확하여 테스트하기 이상적입니다.

- **함수(Functions)**: 정적 메서드(static method)나 확장 메서드(extension method)도 독립적인 테스트 단위가 됩니다. 특히 순수 함수(Pure Function)는 부작용(side effect)이 없어 테스트가 매우 간단합니다.

- **속성(Properties)**: 계산 로직이 포함된 속성(computed property)은 테스트해야 합니다. 예를 들어, `FullName` 속성이 `FirstName`과 `LastName`을 결합하는 로직을 포함한다면 이는 테스트 대상입니다.

- **클래스(Classes)**: 단일 책임 원칙(SRP)을 따르는 작은 규모의 클래스는 전체를 하나의 논리적 단위로 테스트할 수 있습니다.

**FIRST 원칙 - 좋은 단위 테스트의 특징:**

Robert C. Martin(Uncle Bob)과 Tim Ottinger는 좋은 단위 테스트가 갖춰야 할 다섯 가지 핵심 특성을 FIRST라는 약어로 정리했습니다:

1. **Fast (빠른 실행)**: 단위 테스트는 밀리초 단위로 실행되어야 합니다. 전체 테스트 스위트가 몇 초 내에 완료되어야 개발자가 자주 실행할 수 있습니다. Google의 연구에 따르면, 테스트가 10초 이상 걸리면 개발자가 실행을 미루게 되어 효과가 감소합니다.

2. **Independent (독립적)**: 각 테스트는 다른 테스트에 의존하지 않고 독립적으로 실행되어야 합니다. 테스트 간 의존성은 디버깅을 어렵게 만들고, 병렬 실행을 불가능하게 합니다. xUnit은 기본적으로 각 테스트 클래스의 새 인스턴스를 생성하여 격리를 보장합니다.

3. **Repeatable (반복 가능)**: 동일한 입력에 대해 항상 동일한 결과를 반환해야 합니다. 현재 시간, 랜덤 값, 외부 시스템 상태에 의존하는 테스트는 "Flaky Test"가 되어 신뢰성을 떨어뜨립니다.

4. **Self-validating (자체 검증)**: 테스트 결과(성공/실패)가 명확해야 하며, 수동 확인이 필요하지 않습니다. Assert 문을 통해 기대값과 실제값을 자동으로 비교합니다.

5. **Timely (적시성)**: 테스트는 프로덕션 코드를 작성하기 직전이나 직후에 즉시 작성되어야 합니다. TDD에서는 테스트를 먼저 작성합니다.

**추가적인 품질 특성:**

FIRST 외에도 고품질 단위 테스트는 다음 특성을 가져야 합니다:

- **Isolation (격리성)**: 외부 시스템(데이터베이스, 파일 시스템, 네트워크)에 의존하지 않아야 합니다.
- **Trustworthy (신뢰성)**: 테스트가 실패하면 반드시 코드에 문제가 있어야 하고, 통과하면 코드가 올바르게 동작해야 합니다.
- **Maintainable (유지보수성)**: 프로덕션 코드만큼 깔끔하고 읽기 쉬워야 합니다.
- **Focused (집중성)**: 하나의 테스트는 하나의 개념만 검증해야 합니다.

### 단위 테스트의 필요성과 가치

**1. 코드 품질 향상과 결함 예방:**

단위 테스트는 코드의 정확성을 검증하고, 예상치 못한 동작을 조기에 발견합니다. 다음 예제는 테스트가 어떻게 숨겨진 버그를 드러내는지 보여줍니다:

```csharp
// 테스트 없이 작성한 코드 - 버그가 숨어있습니다
public class Calculator
{
    public int Divide(int a, int b)
    {
        return a / b;  // b가 0일 때 DivideByZeroException 발생!
    }
}

// 단위 테스트를 통해 엣지 케이스를 발견합니다
[Fact]
public void Divide_ByZero_ThrowsException()
{
    // Arrange
    var calculator = new Calculator();
    
    // Act & Assert
    Assert.Throws<DivideByZeroException>(() => calculator.Divide(10, 0));
}

// 테스트를 통과하도록 코드 개선
public class Calculator
{
    public double Divide(int a, int b)
    {
        if (b == 0)
        {
            throw new ArgumentException("0으로 나눌 수 없습니다.", nameof(b));
        }
        return (double)a / b;
    }
}
```

**2. 리팩토링 안전망 (Refactoring Safety Net):**

Martin Fowler는 "리팩토링의 첫 단계는 견고한 테스트 스위트를 구축하는 것"이라고 강조합니다. 테스트가 있으면 코드를 개선하거나 구조를 변경할 때 기존 기능이 여전히 작동하는지 즉시 확인할 수 있습니다. 이는 레거시 코드를 현대화하거나 성능 최적화를 수행할 때 특히 중요합니다.

```csharp
// 리팩토링 전: 복잡한 조건문
public class OrderValidator
{
    public bool IsValid(Order order)
    {
        if (order != null && order.Items != null && order.Items.Count > 0 && 
            order.TotalAmount > 0 && !string.IsNullOrEmpty(order.CustomerName))
        {
            return true;
        }
        return false;
    }
}

// 리팩토링 후: 명확한 검증 로직
public class OrderValidator
{
    public bool IsValid(Order order)
    {
        return order != null &&
               HasValidItems(order) &&
               HasValidAmount(order) &&
               HasValidCustomer(order);
    }
    
    private bool HasValidItems(Order order) => 
        order.Items != null && order.Items.Count > 0;
    
    private bool HasValidAmount(Order order) => 
        order.TotalAmount > 0;
    
    private bool HasValidCustomer(Order order) => 
        !string.IsNullOrEmpty(order.CustomerName);
}

// 테스트가 있으면 리팩토링 후에도 동작이 동일함을 즉시 확인 가능
[Theory]
[InlineData(null, false)]
[InlineData("ValidOrder", true)]
public void IsValid_VariousScenarios_ReturnsExpectedResult(string scenario, bool expected)
{
    // 테스트 코드...
}
```

**3. 살아있는 문서 (Living Documentation):**

잘 작성된 테스트는 코드의 의도와 사용 방법을 명확하게 보여주는 실행 가능한 문서 역할을 합니다. 주석이나 문서와 달리, 테스트는 항상 코드와 동기화되어 있습니다. 새로운 팀원이 레거시 코드를 이해하거나, API 사용법을 학습할 때 테스트 코드가 가장 신뢰할 수 있는 참조 자료가 됩니다.

```csharp
// 테스트 이름만 읽어도 API 사용법을 이해할 수 있습니다
[Fact]
public void DateRange_CreateWithStartAndEnd_CreatesValidRange()
{
    var start = new DateTime(2024, 1, 1);
    var end = new DateTime(2024, 12, 31);
    
    var range = new DateRange(start, end);
    
    Assert.Equal(start, range.Start);
    Assert.Equal(end, range.End);
    Assert.Equal(365, range.DurationInDays);
}

[Fact]
public void DateRange_EndBeforeStart_ThrowsArgumentException()
{
    var start = new DateTime(2024, 12, 31);
    var end = new DateTime(2024, 1, 1);
    
    Assert.Throws<ArgumentException>(() => new DateRange(start, end));
}
```

**4. 버그 조기 발견과 경제적 가치:**

IBM, NIST, Capers Jones의 연구에 따르면, 소프트웨어 결함의 수정 비용은 발견 시점에 따라 기하급수적으로 증가합니다:

| 발견 단계 | 상대적 수정 비용 | 실제 영향 |
|----------|----------------|---------|
| 개발 단계 (단위 테스트) | 1x | 개발자가 즉시 수정, 컨텍스트 유지 |
| 통합 테스트 단계 | 10x | 여러 컴포넌트 분석 필요 |
| 시스템 테스트 단계 | 15x | 전체 시스템 영향 파악 필요 |
| 운영 환경 (프로덕션) | 100x+ | 고객 영향, 긴급 패치, 신뢰도 손상 |

Microsoft의 Visual Studio 팀은 단위 테스트 도입 후 다음과 같은 개선을 보고했습니다:

- 프로덕션 버그 60-90% 감소
- 버그 수정 시간 40% 단축
- 리팩토링 자신감 증가로 코드 품질 향상

Google의 내부 연구(2017)에 따르면:

- 테스트 커버리지 75% 이상 프로젝트는 프로덕션 결함이 평균 45% 적음
- 1000줄의 프로덕션 코드당 평균 1.5개의 단위 테스트 존재
- 전체 코드베이스의 약 30%가 테스트 코드

**5. 설계 품질 향상 (Design Feedback):**

테스트하기 어려운 코드는 대부분 설계에 문제가 있습니다. 단위 테스트를 작성하는 과정은 다음과 같은 설계 문제를 조기에 발견하게 합니다:

- **강한 결합(Tight Coupling)**: 의존성이 많으면 테스트 설정이 복잡해집니다
- **낮은 응집도(Low Cohesion)**: 클래스가 너무 많은 책임을 가지면 테스트가 산만해집니다
- **전역 상태(Global State)**: 전역 변수나 싱글톤은 테스트 간 간섭을 일으킵니다
- **숨겨진 의존성**: 생성자나 메서드 시그니처에 드러나지 않은 의존성은 테스트를 불안정하게 만듭니다

### 테스트 피라미드 - 균형잡힌 테스트 전략

Mike Cohn이 제안한 **테스트 피라미드(Test Pyramid)** 모델은 효과적인 자동화 테스트 전략의 지침을 제공합니다:

```
       /\
      /  \    E2E/UI Tests
     /    \   (가장 느림, 가장 비싸, 가장 불안정)
    /------\  
   /        \  Integration Tests
  /          \ (중간 속도, 중간 비용)
 /            \
/______________\ Unit Tests
(가장 빠름, 가장 저렴, 가장 안정적)
```

**각 계층의 특성과 비율:**

1. **단위 테스트(Unit Tests)**: 약 70%
   - 개별 클래스나 메서드의 로직 검증
   - 실행 시간: 밀리초
   - 실패 시 원인 파악: 매우 쉬움
   - 유지보수 비용: 낮음

2. **통합 테스트(Integration Tests)**: 약 20%
   - 여러 컴포넌트 간 상호작용 검증
   - 데이터베이스, 외부 API, 메시지 큐 등
   - 실행 시간: 초 단위
   - 실패 시 원인 파악: 중간
   - 유지보수 비용: 중간

3. **E2E/UI 테스트(End-to-End Tests)**: 약 10%
   - 사용자 시나리오 전체 검증
   - 브라우저 자동화, UI 상호작용
   - 실행 시간: 분 단위
   - 실패 시 원인 파악: 어려움 (Flaky Tests 빈번)
   - 유지보수 비용: 높음

**안티패턴: 역삼각형 (Ice Cream Cone):**

많은 프로젝트가 실수로 다음과 같은 역삼각형 구조를 가집니다:

```
______________ E2E Tests (많음)
 \            /
  \          /  Integration Tests (적음)
   \        /
    \------/   Unit Tests (거의 없음)
     \    /
      \  /
       \/
```

이는 다음 문제를 야기합니다:
- 테스트 실행이 너무 느려 피드백 주기 증가
- Flaky Tests로 인한 신뢰도 저하
- 높은 유지보수 비용
- 실패 원인 파악의 어려움

### .NET 테스트 프레임워크 비교와 선택

.NET 생태계에는 세 가지 주요 단위 테스트 프레임워크가 경쟁하고 있습니다. 각각은 고유한 역사와 설계 철학을 가지며, 특정 시나리오에서 장단점이 있습니다.

**프레임워크별 상세 비교:**

| 특성 | xUnit.net | NUnit | MSTest |
|------|-----------|-------|--------|
| **첫 릴리스** | 2007년 | 2002년 | 2005년 |
| **설계 철학** | 현대적, 확장 가능 | JUnit 스타일 | Visual Studio 통합 |
| **병렬 실행** | 기본 지원 (TestCollection) | 기본 지원 (Parallelizable) | 제한적 지원 |
| **Test Fixture** | IClassFixture, ICollectionFixture | [SetUp], [TearDown] | [TestInitialize], [TestCleanup] |
| **매개변수화** | [Theory] + [InlineData] | [TestCase] | [DataRow] |
| **Assertion 스타일** | Assert.Equal(expected, actual) | Assert.That(actual, Is.EqualTo(expected)) | Assert.AreEqual(expected, actual) |
| **확장성** | 매우 높음 (ITestFramework) | 높음 | 제한적 |
| **.NET Core 지원** | 1급 시민 | 우수 | 우수 |
| **Microsoft 프로젝트 사용** | ASP.NET Core, EF Core | - | - |
| **커뮤니티 크기** | 매우 큼 | 큼 | 중간 |
| **학습 곡선** | 낮음 | 중간 | 낮음 |

**xUnit.net의 핵심 설계 원칙:**

1. **테스트 격리(Test Isolation)**: xUnit은 각 테스트마다 테스트 클래스의 새 인스턴스를 생성합니다. 이는 테스트 간 상태 공유를 방지하고 완전한 격리를 보장합니다.

```csharp
public class CounterTests
{
    private int _counter = 0;  // 각 테스트마다 독립적
    
    [Fact]
    public void Test1() 
    {
        _counter++;
        Assert.Equal(1, _counter);  // 항상 1
    }
    
    [Fact]
    public void Test2() 
    {
        _counter++;
        Assert.Equal(1, _counter);  // 항상 1 (새 인스턴스)
    }
}
```

2. **확장 가능한 아키텍처**: xUnit의 핵심은 확장 포인트로 설계되어 있어, 사용자 정의 Test Framework, Test Discovery, Test Execution을 구현할 수 있습니다.

3. **병렬 실행 최적화**: xUnit은 기본적으로 테스트를 병렬로 실행하여 전체 테스트 스위트의 실행 시간을 크게 단축합니다. TestCollection을 통해 병렬 실행 제어가 가능합니다.

4. **특성 기반 확장(Attribute-based Extension)**: [Fact], [Theory] 외에도 사용자 정의 특성을 만들어 테스트 동작을 확장할 수 있습니다.

**왜 이 책에서 xUnit을 선택했는가:**

1. **Microsoft 공식 권장**: .NET Core 이후 모든 Microsoft의 주요 오픈소스 프로젝트(ASP.NET Core, Entity Framework Core, .NET Runtime)가 xUnit을 표준으로 채택했습니다.

2. **현대적 C# 기능 활용**: xUnit은 .NET의 최신 기능(async/await, IAsyncLifetime, 제네릭, 속성 주입 등)을 적극 활용합니다.

3. **명확한 Assert 문법**: `Assert.Equal(expected, actual)` 순서가 직관적이고, IntelliSense가 잘 작동합니다.

4. **활발한 커뮤니티**: GitHub에서 7,000+ 스타, 활발한 이슈 관리, 빠른 .NET 버전 지원

5. **성능**: 병렬 실행 최적화로 대규모 테스트 스위트에서 뛰어난 성능을 보입니다.

**다른 프레임워크를 선택해야 하는 경우:**

- **NUnit**: 레거시 프로젝트, 또는 복잡한 Constraint 기반 Assertion을 선호하는 경우
- **MSTest**: Visual Studio Test Explorer와의 깊은 통합이 필수적인 기업 환경

이 책에서는 **.NET Core/10의 표준**이자 **Microsoft가 공식 권장**하는 **xUnit.net**을 사용하여 모든 예제를 작성합니다.

---

## 24.2 xUnit 설정

xUnit.net은 .NET 커뮤니티에서 가장 널리 사용되는 단위 테스트 프레임워크로, 간결한 문법과 뛰어난 확장성을 제공합니다. Brad Wilson과 Jim Newkirk이 2007년에 시작한 이 프로젝트는 NUnit과 MSTest의 한계를 극복하고, .NET의 현대적 기능을 최대한 활용하도록 설계되었습니다. 이 섹션에서는 .NET 10 환경에서 xUnit을 설정하고, 그 내부 동작 원리를 이해하는 전체 과정을 다룹니다.

**xUnit.net의 아키텍처 이해:**

xUnit은 다음과 같은 계층적 아키텍처로 구성되어 있습니다:

```
┌─────────────────────────────────────────────────────────┐
│ Test Runner (실행 환경)                                   │
│ • dotnet test (CLI)                                     │
│ • Visual Studio Test Explorer                           │
│ • Visual Studio Code Test Explorer                      │
│ • ReSharper, Rider                                      │
└─────────────────────────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────┐
│ xUnit Test Framework (핵심 엔진)                         │
│ • Test Discovery: 테스트 메서드 찾기                       │
│ • Test Execution: 테스트 실행 및 격리                     │
│ • Parallelization: 병렬 실행 관리                        │
└─────────────────────────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────┐
│ Extensibility Layer (확장 계층)                          │
│ • ITestFramework: 프레임워크 확장                         │
│ • ITestCaseOrderer: 테스트 순서 제어                     │
│ • ITestCollectionOrderer: 컬렉션 순서 제어               │
└─────────────────────────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────┐
│ Test Code (사용자 코드)                                   │
│ • [Fact], [Theory] 특성                                 │
│ • Fixture (IClassFixture, ICollectionFixture)           │
│ • Assert 메서드                                          │
└─────────────────────────────────────────────────────────┘
```

**xUnit의 핵심 개념:**

1. **Test Discovery (테스트 발견)**: xUnit은 Reflection을 사용하여 `[Fact]`와 `[Theory]` 특성이 붙은 public 메서드를 자동으로 찾아냅니다.

2. **Test Collection (테스트 컬렉션)**: 기본적으로 각 테스트 클래스는 독립적인 컬렉션에 속하며 병렬로 실행됩니다. 관련된 테스트들을 그룹화하여 순차 실행이 필요할 때 `[Collection]` 특성을 사용합니다.

3. **Test Lifetime (테스트 생명주기)**: 
   - **Per-Test-Method**: 각 테스트 메서드마다 새 클래스 인스턴스 생성 (기본 동작)
   - **Per-Test-Class**: IClassFixture를 통한 클래스 단위 공유 리소스
   - **Per-Collection**: ICollectionFixture를 통한 컬렉션 단위 공유 리소스

4. **Parallelization (병렬화)**: xUnit은 기본적으로 서로 다른 테스트 클래스를 병렬로 실행하여 전체 테스트 시간을 단축합니다.

### xUnit 프로젝트 생성

**테스트 프로젝트 구조:**

일반적으로 솔루션은 다음과 같은 구조를 가집니다:

```
MySolution/
├── src/
│   └── MyLibrary/          # 실제 코드
│       ├── Calculator.cs
│       └── MyLibrary.csproj
└── tests/
    └── MyLibrary.Tests/    # 테스트 코드
        ├── CalculatorTests.cs
        └── MyLibrary.Tests.csproj
```

**실습: 계산기 라이브러리와 테스트 프로젝트 생성**

터미널에서 다음 명령어를 순서대로 실행합니다:

```bash
# 작업 디렉토리 생성 및 이동
mkdir UnitTestExample
cd UnitTestExample

# 소스 코드 프로젝트 생성 (클래스 라이브러리)
dotnet new classlib -n Calculator
cd Calculator

# 기본 생성된 Class1.cs 삭제
rm Class1.cs
```

`Calculator.cs` 파일을 생성하고 다음 코드를 작성합니다:

```csharp
namespace Calculator;

public class BasicCalculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Subtract(int a, int b)
    {
        return a - b;
    }

    public int Multiply(int a, int b)
    {
        return a * b;
    }

    public double Divide(int a, int b)
    {
        if (b == 0)
        {
            throw new DivideByZeroException("0으로 나눌 수 없습니다.");
        }
        return (double)a / b;
    }
}
```

이제 테스트 프로젝트를 생성합니다:

```bash
# 상위 디렉토리로 이동
cd ..

# xUnit 테스트 프로젝트 생성
dotnet new xunit -n Calculator.Tests
cd Calculator.Tests

# 테스트 프로젝트에서 Calculator 프로젝트 참조 추가
dotnet add reference ../Calculator/Calculator.csproj
```

### 생성된 테스트 프로젝트 구조 이해

`dotnet new xunit` 명령은 다음과 같은 파일들을 자동 생성합니다:

**Calculator.Tests.csproj:**

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <IsPackable>false</IsPackable>
    <IsTestProject>true</IsTestProject>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.11.0" />
    <PackageReference Include="xunit" Version="2.9.0" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.8.2">
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
      <PrivateAssets>all</PrivateAssets>
    </PackageReference>
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="..\Calculator\Calculator.csproj" />
  </ItemGroup>

</Project>
```

**주요 NuGet 패키지:**

- **Microsoft.NET.Test.Sdk**: .NET 테스트 호스트 및 실행기
- **xunit**: xUnit 핵심 프레임워크
- **xunit.runner.visualstudio**: Visual Studio 및 VS Code 통합

### 테스트 실행 확인

기본 생성된 테스트를 실행하여 환경이 올바르게 설정되었는지 확인합니다:

```bash
# 테스트 프로젝트 디렉토리에서 실행
dotnet test

# 출력 예시:
# Determining projects to restore...
# All projects are up-to-date for restore.
# Calculator -> /path/to/Calculator/bin/Debug/net10.0/Calculator.dll
# Calculator.Tests -> /path/to/Calculator.Tests/bin/Debug/net10.0/Calculator.Tests.dll
# Test run for /path/to/Calculator.Tests/bin/Debug/net10.0/Calculator.Tests.dll (.NET 10.0.0)
# Microsoft (R) Test Execution Command Line Tool Version 17.11.0
# Copyright (c) Microsoft Corporation.  All rights reserved.
#
# Starting test execution, please wait...
# A total of 1 test files matched the specified pattern.
#
# Passed!  - Failed:     0, Passed:     1, Skipped:     0, Total:     1, Duration: < 1 ms
```

---

## 24.3 첫 번째 테스트 작성

이제 실제로 Calculator 클래스를 테스트하는 단위 테스트를 작성해봅시다. 이 섹션에서는 xUnit의 기본 문법과 구조를 익히고, 테스트가 실행되는 내부 메커니즘을 이해하며, 효과적인 테스트 작성의 모범 사례를 학습합니다.

**xUnit 테스트 실행 생명주기:**

xUnit이 테스트를 실행할 때의 내부 프로세스를 이해하면 더 효과적인 테스트를 작성할 수 있습니다:

1. **Test Discovery Phase**:
   - xUnit은 어셈블리를 스캔하여 `[Fact]`와 `[Theory]` 특성을 찾습니다
   - Reflection을 사용하여 테스트 메서드의 메타데이터를 수집합니다
   - 테스트 컬렉션을 구성하고 실행 계획을 수립합니다

2. **Test Execution Phase**:
   ```
   각 테스트마다:
   ├── 1. 테스트 클래스의 새 인스턴스 생성
   ├── 2. Constructor 실행 (테스트 초기화)
   ├── 3. 테스트 메서드 실행
   ├── 4. Assert 검증
   ├── 5. IDisposable.Dispose() 호출 (정리)
   └── 6. 인스턴스 폐기
   ```

3. **Result Collection Phase**:
   - 테스트 결과(Pass/Fail/Skip)를 수집합니다
   - 실행 시간, 예외 정보, 스택 트레이스를 기록합니다
   - Test Runner에 결과를 보고합니다

### 기본 테스트 작성

`Calculator.Tests` 프로젝트의 기본 생성된 `UnitTest1.cs` 파일을 삭제하고, `CalculatorTests.cs` 파일을 생성합니다:

```csharp
using Calculator;
using Xunit;

namespace Calculator.Tests;

public class BasicCalculatorTests
{
    [Fact]
    public void Add_TwoPositiveNumbers_ReturnsSum()
    {
        // Arrange: 테스트 준비
        var calculator = new BasicCalculator();
        int a = 5;
        int b = 3;

        // Act: 테스트 실행
        int result = calculator.Add(a, b);

        // Assert: 결과 검증
        Assert.Equal(8, result);
    }

    [Fact]
    public void Subtract_TwoNumbers_ReturnsDifference()
    {
        // Arrange
        var calculator = new BasicCalculator();

        // Act
        int result = calculator.Subtract(10, 4);

        // Assert
        Assert.Equal(6, result);
    }

    [Fact]
    public void Multiply_TwoNumbers_ReturnsProduct()
    {
        // Arrange
        var calculator = new BasicCalculator();

        // Act
        int result = calculator.Multiply(6, 7);

        // Assert
        Assert.Equal(42, result);
    }

    [Fact]
    public void Divide_ValidNumbers_ReturnsQuotient()
    {
        // Arrange
        var calculator = new BasicCalculator();

        // Act
        double result = calculator.Divide(10, 2);

        // Assert
        Assert.Equal(5.0, result);
    }

    [Fact]
    public void Divide_ByZero_ThrowsException()
    {
        // Arrange
        var calculator = new BasicCalculator();

        // Act & Assert: 예외 발생 검증
        Assert.Throws<DivideByZeroException>(() => calculator.Divide(10, 0));
    }
}
```

### xUnit 핵심 요소 이해

**[Fact] 특성(Attribute):**

`[Fact]`는 해당 메서드가 단위 테스트임을 xUnit에 알립니다. 내부적으로 xUnit은 Reflection을 통해 `FactAttribute`를 상속한 모든 특성을 찾아 테스트로 인식합니다.

- 매개변수를 받지 않는 테스트 메서드
- 항상 동일한 방식으로 실행되는 테스트
- 테스트 메서드는 `public void` 또는 `public async Task` 형식이어야 함
- 테스트 클래스는 반드시 public이어야 하며 추상 클래스가 아니어야 함

**[Fact]의 선택적 속성:**

```csharp
[Fact(DisplayName = "사용자 정의 테스트 이름")]
public void Test1() { }

[Fact(Skip = "아직 구현되지 않은 기능")]
public void Test2() { }

[Fact(Timeout = 1000)]  // 1초 내에 완료되어야 함
public void Test3() { }
```

**Assert 클래스 - 포괄적 가이드:**

xUnit의 `Assert` 클래스는 테스트 결과를 검증하는 60개 이상의 메서드를 제공합니다. 주요 카테고리별로 살펴봅시다:

**1. 값 동등성 검증:**

```csharp
// 기본 동등성
Assert.Equal(expected, actual);        // 두 값이 같은지 (Equals 사용)
Assert.NotEqual(expected, actual);     // 두 값이 다른지

// 부동소수점 정밀도 비교
Assert.Equal(3.14159, actual, precision: 3);  // 소수점 3자리까지 비교

// 문자열 대소문자 무시 비교
Assert.Equal("hello", "HELLO", ignoreCase: true);

// 객체 동일성 (참조 비교)
Assert.Same(expected, actual);         // 같은 인스턴스인지
Assert.NotSame(expected, actual);      // 다른 인스턴스인지
```

**2. Boolean 및 조건 검증:**

```csharp
// Boolean 검증
Assert.True(condition);                // 조건이 true인지
Assert.False(condition);               // 조건이 false인지

// Null 검증
Assert.Null(obj);                      // 객체가 null인지
Assert.NotNull(obj);                   // 객체가 null이 아닌지

// 타입 검증
Assert.IsType<ExpectedType>(obj);      // 정확한 타입인지
Assert.IsAssignableFrom<BaseType>(obj); // 할당 가능한 타입인지
```

**3. 예외 검증:**

```csharp
// 특정 예외 타입 검증
var exception = Assert.Throws<ArgumentException>(() => 
{
    throw new ArgumentException("Invalid argument");
});
Assert.Equal("Invalid argument", exception.Message);

// 비동기 예외 검증
await Assert.ThrowsAsync<InvalidOperationException>(async () => 
{
    await SomeAsyncMethod();
});

// 예외의 특정 속성 검증
var ex = Assert.Throws<ArgumentNullException>(() => 
{
    DoSomething(null);
});
Assert.Equal("paramName", ex.ParamName);

// 모든 예외 포괄 검증 (안티패턴 - 권장하지 않음)
Assert.ThrowsAny<Exception>(() => { });
```

**4. 컬렉션 검증:**

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// 항목 포함 여부
Assert.Contains(3, numbers);           // 3이 포함되는지
Assert.DoesNotContain(6, numbers);     // 6이 포함되지 않는지

// 조건으로 검색
Assert.Contains(numbers, n => n > 4);  // 4보다 큰 항목이 있는지

// 빈 컬렉션
Assert.Empty(new List<int>());         // 컬렉션이 비어있는지
Assert.NotEmpty(numbers);              // 컬렉션이 비어있지 않은지

// 단일 항목
Assert.Single(new[] { 42 });           // 정확히 1개의 항목
var item = Assert.Single(new[] { 42 });// 단일 항목 반환
Assert.Equal(42, item);

// 컬렉션 전체 비교
var expected = new[] { 1, 2, 3 };
var actual = new[] { 1, 2, 3 };
Assert.Equal(expected, actual);        // 순서와 값이 모두 같은지

// 모든 항목이 조건을 만족하는지
Assert.All(numbers, n => Assert.True(n > 0));
```

**5. 문자열 검증:**

```csharp
// 문자열 포함 여부
Assert.Contains("world", "Hello world");
Assert.DoesNotContain("foo", "Hello world");

// 시작/끝 검증
Assert.StartsWith("Hello", "Hello world");
Assert.EndsWith("world", "Hello world");

// 정규식 검증
Assert.Matches(@"\d{3}-\d{4}", "123-4567");  // 패턴 매칭
Assert.DoesNotMatch(@"[a-z]", "123");        // 패턴 불일치

// 빈 문자열
Assert.Empty("");
Assert.NotEmpty("text");
```

**6. 숫자 범위 검증:**

```csharp
// 범위 검증
Assert.InRange(5, 1, 10);              // 5가 1~10 사이인지
Assert.NotInRange(15, 1, 10);          // 15가 1~10 밖인지
```

**7. 이벤트 검증:**

```csharp
// 이벤트가 발생했는지 검증
var obj = new MyClass();
Assert.Raises<EventArgs>(
    handler => obj.MyEvent += handler,
    handler => obj.MyEvent -= handler,
    () => obj.TriggerEvent()
);

// PropertyChanged 이벤트 검증
Assert.PropertyChanged(obj, nameof(obj.Property), () => 
{
    obj.Property = "new value";
});
```

**Assert vs FluentAssertions:**

xUnit의 기본 Assert는 강력하지만, 일부 팀은 더 읽기 쉬운 문법을 위해 FluentAssertions 라이브러리를 사용합니다:

```csharp
// xUnit 스타일
Assert.Equal(5, result);
Assert.True(result > 0);
Assert.Contains(item, collection);

// FluentAssertions 스타일 (선택적)
result.Should().Be(5);
result.Should().BePositive();
collection.Should().Contain(item);
```

이 책에서는 xUnit의 표준 Assert를 사용하여 추가 의존성 없이 테스트를 작성합니다.

### 테스트 실행 및 결과 확인

```bash
dotnet test

# 출력:
# Passed!  - Failed:     0, Passed:     5, Skipped:     0, Total:     5, Duration: 15 ms
```

모든 테스트가 통과하면 초록색 체크마크와 함께 "Passed!"가 표시됩니다.

### 테스트 명명 규칙

명확한 테스트 이름은 실패 시 문제를 빠르게 파악하는 데 도움이 됩니다. 권장되는 명명 패턴:

```
[메서드명]_[시나리오]_[예상결과]
```

예시:
- `Add_TwoPositiveNumbers_ReturnsSum`
- `Divide_ByZero_ThrowsException`
- `GetUser_InvalidId_ReturnsNull`

### 실습: 실패하는 테스트 경험하기

테스트가 실패하는 경우를 경험해봅시다. 다음 테스트를 추가합니다:

```csharp
[Fact]
public void Add_AlwaysFails_ForDemonstration()
{
    var calculator = new BasicCalculator();
    int result = calculator.Add(2, 2);
    
    // 의도적으로 잘못된 예상값
    Assert.Equal(5, result);  // 실제는 4인데 5를 기대
}
```

테스트를 실행하면:

```bash
dotnet test

# 출력:
# Failed Add_AlwaysFails_ForDemonstration [< 1 ms]
# Error Message:
#  Assert.Equal() Failure
# Expected: 5
# Actual:   4
# 
# Failed!  - Failed:     1, Passed:     5, Skipped:     0, Total:     6, Duration: 25 ms
```

xUnit은 어떤 테스트가 실패했는지, 예상값과 실제값이 무엇인지 명확하게 보여줍니다. 이 테스트는 삭제하고 계속 진행합니다.

---

## 24.4 Arrange-Act-Assert 패턴

Arrange-Act-Assert(AAA) 패턴은 Bill Wake가 2001년에 제안한 단위 테스트 구조화 방법으로, 현재 업계 표준으로 자리잡았습니다. 이 패턴은 테스트를 세 가지 명확한 단계로 나누어, 가독성과 유지보수성을 크게 향상시킵니다. Kent Beck은 이를 "테스트 코드의 문법(Grammar of Test Code)"이라고 표현했으며, Robert C. Martin은 "Clean Code"에서 테스트 가독성의 핵심 요소로 강조했습니다.

**AAA 패턴의 이론적 기반:**

AAA 패턴은 인간의 인지 과정과 일치하는 자연스러운 구조를 제공합니다:

1. **준비(Setup)**: "어떤 상황에서" - 테스트의 전제 조건
2. **실행(Exercise)**: "무엇을 하면" - 테스트할 동작
3. **검증(Verify)**: "어떤 결과가 나와야 하는가" - 기대 결과

이 구조는 Given-When-Then 패턴(BDD, Behavior-Driven Development에서 사용)과 동일한 개념이며, 테스트를 명세서(specification)처럼 읽을 수 있게 만듭니다.

### AAA 패턴의 세 단계 심층 분석

**1. Arrange (준비) - 테스트 컨텍스트 설정:**

Arrange 단계에서는 테스트에 필요한 모든 사전 조건을 설정합니다:

- **SUT (System Under Test) 생성**: 테스트 대상 객체를 인스턴스화합니다
- **Test Double 준비**: Mock, Stub, Fake 등 의존성 대체 객체를 설정합니다
- **입력 데이터 준비**: 테스트에 사용할 파라미터와 데이터를 준비합니다
- **환경 설정**: 필요한 상태나 컨텍스트를 구성합니다

```csharp
[Fact]
public void ProcessOrder_ValidOrder_UpdatesInventory()
{
    // ============ Arrange ============
    // 1. 의존성 준비 (Stub)
    var mockInventory = new Mock<IInventoryService>();
    mockInventory
        .Setup(i => i.GetStock(It.IsAny<string>()))
        .Returns(100);
    
    // 2. SUT 생성 (의존성 주입)
    var orderProcessor = new OrderProcessor(mockInventory.Object);
    
    // 3. 입력 데이터 준비
    var order = new Order
    {
        ProductId = "PROD-001",
        Quantity = 5,
        CustomerId = "CUST-123"
    };
    
    // 4. 기대값 설정
    var expectedNewStock = 95;  // 100 - 5
    
    // ... Act와 Assert 계속
}
```

**주의사항:**
- Arrange가 너무 복잡하면 테스트 가독성이 떨어집니다
- 복잡한 Setup은 Factory Method나 Test Data Builder 패턴으로 추출 고려
- 과도한 Arrange는 테스트 대상 클래스의 설계 문제를 시사할 수 있습니다

**2. Act (실행) - 단일 동작 수행:**

Act 단계는 일반적으로 **단 한 줄**의 코드로 구성되어야 합니다. 이는 "테스트는 하나의 개념만 검증해야 한다"는 원칙과 연결됩니다.

```csharp
// ============ Act ============
// 테스트할 메서드를 정확히 한 번 호출
var result = orderProcessor.Process(order);
```

**Act 단계의 특징:**
- **원자성(Atomic)**: 하나의 메서드 호출 또는 동작만 수행
- **명확성(Clear)**: 무엇을 테스트하는지 즉시 파악 가능
- **격리성(Isolated)**: 다른 동작과 섞이지 않음

**Act가 여러 줄인 경우는 문제의 징후:**
```csharp
// ❌ 나쁜 예: Act가 여러 동작을 포함
var order = orderProcessor.CreateOrder();
orderProcessor.AddItem(order, item1);
orderProcessor.AddItem(order, item2);
var result = orderProcessor.Submit(order);  // 무엇을 테스트하는가?

// ✅ 좋은 예: 각 동작을 별도 테스트로 분리
[Fact]
public void CreateOrder_ReturnsNewOrder() 
{
    var order = orderProcessor.CreateOrder();
    Assert.NotNull(order);
}

[Fact]
public void AddItem_ToExistingOrder_IncreasesItemCount()
{
    var order = TestDataBuilder.CreateOrder();
    
    orderProcessor.AddItem(order, item);
    
    Assert.Single(order.Items);
}
```

**3. Assert (검증) - 결과 확인:**

Assert 단계에서는 Act의 결과가 예상과 일치하는지 검증합니다. "하나의 논리적 개념"을 검증하기 위해 여러 Assert 문을 사용할 수 있습니다.

```csharp
// ============ Assert ============
// 단일 Assert (가장 간단)
Assert.Equal(expectedValue, actualValue);

// 관련된 여러 속성 검증 (하나의 개념)
Assert.NotNull(result);
Assert.Equal(OrderStatus.Completed, result.Status);
Assert.Equal(95, result.RemainingStock);
Assert.True(result.ProcessedDate > DateTime.Now.AddMinutes(-1));

// Mock 호출 검증
mockInventory.Verify(
    i => i.UpdateStock("PROD-001", -5),
    Times.Once
);
```

**Assert 모범 사례:**

1. **하나의 논리적 개념 검증**: 여러 Assert가 있어도 하나의 동작을 검증해야 합니다
2. **구체적 메시지**: 실패 시 원인을 빠르게 파악할 수 있어야 합니다
3. **과도한 검증 지양**: 테스트가 깨지기 쉬워집니다 (Brittle Test)

```csharp
// ❌ 나쁜 예: 여러 개념을 한 테스트에서 검증
[Fact]
public void ProcessOrder_DoesTooMuch()
{
    var result = processor.Process(order);
    
    Assert.Equal(OrderStatus.Completed, result.Status);  // 주문 처리
    Assert.True(emailSent);                               // 이메일 발송
    Assert.Equal(95, inventory.Stock);                    // 재고 차감
    Assert.NotNull(invoice);                              // 송장 생성
}

// ✅ 좋은 예: 각 개념을 별도 테스트로 분리
[Fact]
public void ProcessOrder_ValidOrder_CompletesOrder()
{
    var result = processor.Process(order);
    Assert.Equal(OrderStatus.Completed, result.Status);
}

[Fact]
public void ProcessOrder_ValidOrder_SendsConfirmationEmail()
{
    processor.Process(order);
    mockEmailService.Verify(e => e.Send(It.IsAny<Email>()), Times.Once);
}
```

### AAA 패턴 적용 예시

```csharp
[Fact]
public void Divide_TwoPositiveNumbers_ReturnsCorrectQuotient()
{
    // ============ Arrange ============
    // 테스트 대상 객체 생성
    var calculator = new BasicCalculator();
    
    // 테스트 데이터 준비
    int dividend = 20;
    int divisor = 4;
    double expectedResult = 5.0;

    // ============ Act ============
    // 테스트할 메서드 실행
    double actualResult = calculator.Divide(dividend, divisor);

    // ============ Assert ============
    // 결과 검증
    Assert.Equal(expectedResult, actualResult);
}
```

### 복잡한 시나리오에서의 AAA 패턴

더 복잡한 클래스를 테스트하는 예제를 살펴봅시다:

```csharp
// 테스트 대상 클래스
public class ShoppingCart
{
    private List<CartItem> _items = new();

    public void AddItem(string productName, decimal price, int quantity)
    {
        _items.Add(new CartItem 
        { 
            ProductName = productName, 
            Price = price, 
            Quantity = quantity 
        });
    }

    public decimal GetTotal()
    {
        return _items.Sum(item => item.Price * item.Quantity);
    }

    public int GetItemCount()
    {
        return _items.Sum(item => item.Quantity);
    }
}

public class CartItem
{
    public string ProductName { get; set; } = string.Empty;
    public decimal Price { get; set; }
    public int Quantity { get; set; }
}
```

**AAA 패턴을 적용한 테스트:**

```csharp
public class ShoppingCartTests
{
    [Fact]
    public void GetTotal_MultipleItems_ReturnsSumOfAllItemPrices()
    {
        // ============ Arrange ============
        var cart = new ShoppingCart();
        
        // 여러 상품 추가
        cart.AddItem("노트북", 1500000m, 1);
        cart.AddItem("마우스", 50000m, 2);
        cart.AddItem("키보드", 120000m, 1);
        
        // 예상 총액: 1,500,000 + (50,000 * 2) + 120,000 = 1,720,000
        decimal expectedTotal = 1720000m;

        // ============ Act ============
        decimal actualTotal = cart.GetTotal();

        // ============ Assert ============
        Assert.Equal(expectedTotal, actualTotal);
    }

    [Fact]
    public void GetItemCount_MultipleItems_ReturnsTotalQuantity()
    {
        // Arrange
        var cart = new ShoppingCart();
        cart.AddItem("상품A", 10000m, 3);
        cart.AddItem("상품B", 20000m, 5);
        cart.AddItem("상품C", 30000m, 2);
        int expectedCount = 10;  // 3 + 5 + 2

        // Act
        int actualCount = cart.GetItemCount();

        // Assert
        Assert.Equal(expectedCount, actualCount);
    }

    [Fact]
    public void GetTotal_EmptyCart_ReturnsZero()
    {
        // Arrange
        var cart = new ShoppingCart();

        // Act
        decimal total = cart.GetTotal();

        // Assert
        Assert.Equal(0m, total);
    }
}
```

### AAA 패턴의 장점

**1. 가독성:**
각 단계가 명확히 구분되어 테스트의 의도를 빠르게 파악할 수 있습니다.

**2. 유지보수성:**
테스트 코드를 수정할 때 어느 부분을 변경해야 하는지 쉽게 알 수 있습니다.

**3. 일관성:**
팀 전체가 동일한 패턴을 사용하면 코드 리뷰와 협업이 원활해집니다.

### 주석 사용 가이드

주석으로 세 단계를 명시적으로 표시하는 것이 권장됩니다:

```csharp
[Fact]
public void ExampleTest()
{
    // Arrange
    // ... 준비 코드
    
    // Act
    // ... 실행 코드
    
    // Assert
    // ... 검증 코드
}
```

일부 개발자는 빈 줄로 구분하는 것을 선호하기도 합니다:

```csharp
[Fact]
public void ExampleTest()
{
    var calculator = new BasicCalculator();
    int a = 10, b = 5;
    
    int result = calculator.Add(a, b);
    
    Assert.Equal(15, result);
}
```

### 실습: AAA 패턴으로 테스트 리팩토링

기존 테스트를 AAA 패턴으로 명확히 구조화해봅시다:

```csharp
// Before: 구조가 불명확한 테스트
[Fact]
public void TestCalculatorDivision()
{
    var calc = new BasicCalculator();
    var result = calc.Divide(20, 5);
    Assert.Equal(4.0, result);
    Assert.NotEqual(0, result);
}

// After: AAA 패턴 적용
[Fact]
public void Divide_ValidInputs_ReturnsCorrectQuotient()
{
    // Arrange
    var calculator = new BasicCalculator();
    int dividend = 20;
    int divisor = 5;
    double expectedQuotient = 4.0;

    // Act
    double actualQuotient = calculator.Divide(dividend, divisor);

    // Assert
    Assert.Equal(expectedQuotient, actualQuotient);
    // 추가 검증: 0이 아님 확인
    Assert.NotEqual(0, actualQuotient);
}
```

---

## 24.5 테스트 데이터와 매개변수화된 테스트

동일한 테스트 로직을 여러 입력값에 대해 반복 실행해야 하는 경우가 많습니다. xUnit의 **Theory**와 데이터 제공 메커니즘을 사용하면 코드 중복 없이 효율적으로 여러 시나리오를 테스트할 수 있습니다. 이는 DRY(Don't Repeat Yourself) 원칙을 테스트 코드에 적용하는 핵심 기법입니다.

**매개변수화된 테스트의 역사와 가치:**

매개변수화된 테스트는 JUnit 4에서 `@Parameters`로 처음 도입되었으며, 이후 NUnit의 `[TestCase]`, xUnit의 `[Theory]`로 발전했습니다. 이 패턴의 핵심 가치는:

- **동등 분할(Equivalence Partitioning)**: 입력 공간을 대표 값으로 나누어 효율적으로 테스트
- **경계값 분석(Boundary Value Analysis)**: 경계 조건을 체계적으로 검증
- **코드 중복 제거**: 동일한 로직을 여러 번 작성하지 않음
- **테스트 가독성**: 입력-출력 관계가 명확하게 표현됨

**xUnit의 데이터 제공 메커니즘:**

xUnit은 세 가지 주요 데이터 제공 방식을 지원합니다:

```
┌──────────────────────────────────────────────────┐
│ [InlineData]                                     │
│ • 간단한 상수 값                                  │
│ • 속성(Attribute) 수준에서 직접 정의              │
│ • 가장 널리 사용됨                                │
└──────────────────────────────────────────────────┘
                    ↓
┌──────────────────────────────────────────────────┐
│ [MemberData]                                     │
│ • 메서드, 프로퍼티에서 데이터 제공                │
│ • 복잡한 객체나 동적 데이터                       │
│ • 재사용 가능한 데이터 소스                       │
└──────────────────────────────────────────────────┘
                    ↓
┌──────────────────────────────────────────────────┐
│ [ClassData]                                      │
│ • 별도 클래스에서 데이터 제공                     │
│ • 여러 테스트에서 공유 가능                       │
│ • 가장 강력하고 유연함                            │
└──────────────────────────────────────────────────┘
```

### Theory와 InlineData - 기본 패턴

**[Theory] 특성의 내부 동작:**

`[Theory]`는 xUnit의 확장 가능한 아키텍처를 보여주는 좋은 예입니다. 내부적으로:

1. `TheoryAttribute`는 `FactAttribute`를 상속합니다
2. 데이터 소스로부터 테스트 케이스를 생성합니다
3. 각 데이터 세트에 대해 별도의 테스트 인스턴스를 실행합니다
4. 실패한 케이스의 파라미터 값을 명확하게 보고합니다

**[InlineData]의 제약사항과 해결책:**

InlineData는 편리하지만 제약이 있습니다:

- ✅ 지원: 상수 값 (문자열, 숫자, bool, null, enum, typeof)
- ❌ 비지원: 복잡한 객체, 계산된 값, 현재 시간

```csharp
// ✅ 가능한 InlineData
[InlineData(42)]
[InlineData("text")]
[InlineData(true)]
[InlineData(null)]
[InlineData(DayOfWeek.Monday)]
[InlineData(typeof(string))]

// ❌ 불가능한 InlineData
[InlineData(new Person { Name = "John" })]  // 복잡한 객체
[InlineData(DateTime.Now)]                   // 계산된 값
[InlineData(GetTestData())]                  // 메서드 호출
```

### 기본 사용 예제와 실행 메커니즘

```csharp
public class CalculatorTheoryTests
{
    [Theory]
    [InlineData(2, 3, 5)]      // 2 + 3 = 5
    [InlineData(0, 0, 0)]      // 0 + 0 = 0
    [InlineData(-1, 1, 0)]     // -1 + 1 = 0
    [InlineData(100, 200, 300)] // 100 + 200 = 300
    [InlineData(-5, -3, -8)]   // -5 + -3 = -8
    public void Add_VariousInputs_ReturnsCorrectSum(int a, int b, int expected)
    {
        // Arrange
        var calculator = new BasicCalculator();

        // Act
        int result = calculator.Add(a, b);

        // Assert
        Assert.Equal(expected, result);
    }
}
```

테스트를 실행하면:

```bash
dotnet test

# 출력:
# Passed Add_VariousInputs_ReturnsCorrectSum(a: 2, b: 3, expected: 5) [< 1 ms]
# Passed Add_VariousInputs_ReturnsCorrectSum(a: 0, b: 0, expected: 0) [< 1 ms]
# Passed Add_VariousInputs_ReturnsCorrectSum(a: -1, b: 1, expected: 0) [< 1 ms]
# Passed Add_VariousInputs_ReturnsCorrectSum(a: 100, b: 200, expected: 300) [< 1 ms]
# Passed Add_VariousInputs_ReturnsCorrectSum(a: -5, b: -3, expected: -8) [< 1 ms]
```

각 `InlineData`에 대해 별도의 테스트가 실행됩니다!

### 엣지 케이스 테스트

매개변수화된 테스트는 경계값(boundary values)과 엣지 케이스를 체계적으로 검증하는 데 이상적입니다:

```csharp
[Theory]
[InlineData(10, 2, 5.0)]           // 정상적인 나눗셈
[InlineData(7, 2, 3.5)]            // 소수점 결과
[InlineData(0, 5, 0.0)]            // 0을 나누기
[InlineData(-10, 2, -5.0)]         // 음수 나눗셈
[InlineData(10, -2, -5.0)]         // 음수 제수
[InlineData(int.MaxValue, 1, (double)int.MaxValue)] // 최대값
public void Divide_VariousScenarios_ReturnsCorrectQuotient(
    int dividend, 
    int divisor, 
    double expected)
{
    // Arrange
    var calculator = new BasicCalculator();

    // Act
    double result = calculator.Divide(dividend, divisor);

    // Assert
    Assert.Equal(expected, result, precision: 10);  // 부동소수점 비교 시 정밀도 지정
}
```

### 복잡한 데이터 타입 사용

문자열이나 다른 타입도 매개변수로 전달할 수 있습니다:

```csharp
public class StringValidator
{
    public bool IsValidEmail(string email)
    {
        if (string.IsNullOrWhiteSpace(email))
            return false;
        
        return email.Contains("@") && email.Contains(".");
    }
}

public class StringValidatorTests
{
    [Theory]
    [InlineData("test@example.com", true)]
    [InlineData("user@domain.co.kr", true)]
    [InlineData("invalid.email.com", false)]  // @ 없음
    [InlineData("@example.com", false)]       // 앞 부분 없음
    [InlineData("test@", false)]              // . 없음
    [InlineData("", false)]                   // 빈 문자열
    [InlineData(null, false)]                 // null
    public void IsValidEmail_VariousInputs_ReturnsExpectedResult(
        string email, 
        bool expected)
    {
        // Arrange
        var validator = new StringValidator();

        // Act
        bool result = validator.IsValidEmail(email);

        // Assert
        Assert.Equal(expected, result);
    }
}
```

### MemberData를 사용한 고급 데이터 제공

더 복잡한 테스트 데이터는 `[MemberData]`로 제공할 수 있습니다:

```csharp
public class AdvancedCalculatorTests
{
    public static IEnumerable<object[]> TestData()
    {
        yield return new object[] { 2, 3, 5 };
        yield return new object[] { 10, 20, 30 };
        yield return new object[] { -5, 5, 0 };
        yield return new object[] { 0, 0, 0 };
    }

    [Theory]
    [MemberData(nameof(TestData))]
    public void Add_MemberDataSource_ReturnsCorrectSum(int a, int b, int expected)
    {
        var calculator = new BasicCalculator();
        int result = calculator.Add(a, b);
        Assert.Equal(expected, result);
    }
}
```

### ClassData를 사용한 재사용 가능한 테스트 데이터

테스트 데이터를 별도 클래스로 분리하여 여러 테스트에서 재사용할 수 있습니다:

```csharp
public class CalculatorTestData : IEnumerable<object[]>
{
    public IEnumerator<object[]> GetEnumerator()
    {
        yield return new object[] { 1, 1, 2 };
        yield return new object[] { 5, 5, 10 };
        yield return new object[] { -1, 1, 0 };
    }

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

public class ClassDataExampleTests
{
    [Theory]
    [ClassData(typeof(CalculatorTestData))]
    public void Add_ClassDataSource_ReturnsCorrectSum(int a, int b, int expected)
    {
        var calculator = new BasicCalculator();
        int result = calculator.Add(a, b);
        Assert.Equal(expected, result);
    }
}
```

### 매개변수화된 테스트의 장점

**1. 코드 중복 제거:**
동일한 로직을 반복 작성하지 않고 여러 시나리오를 테스트합니다.

**2. 포괄적인 테스트:**
다양한 입력값을 쉽게 추가하여 테스트 커버리지를 높일 수 있습니다.

**3. 유지보수 용이:**
테스트 로직을 한 곳에서 관리하므로, 수정 시 한 번만 변경하면 됩니다.

---

## 24.6 Moq를 이용한 Mocking

실제 애플리케이션에서는 테스트하려는 코드가 데이터베이스, 외부 API, 파일 시스템 등 외부 의존성을 가지는 경우가 많습니다. **Moq**(발음: "Mock-you" 또는 "Mock")는 이러한 의존성을 가짜 객체(Mock)로 대체하여 단위 테스트를 독립적으로 실행할 수 있게 해주는 .NET의 가장 인기 있는 Mocking 프레임워크입니다.

**Mocking과 Test Double의 이론:**

Gerard Meszaros의 "xUnit Test Patterns"에서 정의한 Test Double의 다섯 가지 유형을 이해하는 것이 중요합니다:

1. **Dummy**: 파라미터를 채우기 위해 전달되지만 실제로 사용되지 않는 객체
2. **Stub**: 테스트에서 호출하는 메서드에 대해 미리 정의된 응답을 제공하는 객체
3. **Spy**: Stub의 일종으로, 호출 정보를 기록하는 객체
4. **Mock**: 기대하는 호출이 발생했는지 검증하는 객체 (Moq의 주요 용도)
5. **Fake**: 실제 동작하지만 프로덕션에는 적합하지 않은 간단한 구현 (예: InMemoryDatabase)

Moq는 주로 **Mock**과 **Stub** 역할을 수행하며, 동적 프록시 생성을 통해 런타임에 인터페이스 구현을 생성합니다.

**Moq의 내부 동작 원리:**

Moq는 Castle.Core의 DynamicProxy를 사용하여 다음과 같이 동작합니다:

```
┌─────────────────────────────────────────────────┐
│ 1. Mock<T> 생성                                  │
│    var mock = new Mock<IUserRepository>();     │
└─────────────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────────────┐
│ 2. DynamicProxy로 런타임 타입 생성               │
│    • IUserRepository 구현체를 IL로 생성          │
│    • 모든 메서드 호출을 Interceptor로 리디렉션    │
└─────────────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────────────┐
│ 3. Setup으로 동작 정의                           │
│    mock.Setup(m => m.GetUser(1))                │
│        .Returns(new User());                    │
└─────────────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────────────┐
│ 4. 테스트에서 mock.Object 사용                   │
│    var service = new Service(mock.Object);      │
└─────────────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────────────┐
│ 5. Verify로 호출 검증                            │
│    mock.Verify(m => m.GetUser(1), Times.Once); │
└─────────────────────────────────────────────────┘
```

**Moq vs 수동 Mock 작성:**

Moq 이전에는 다음과 같이 수동으로 Mock을 작성했습니다:

```csharp
// ❌ Moq 없이: 수동 Mock 작성 (번거로움)
public class FakeUserRepository : IUserRepository
{
    private User? _userToReturn;
    public int GetUserByIdCallCount { get; private set; }
    
    public void SetupGetUserById(User user)
    {
        _userToReturn = user;
    }
    
    public User? GetUserById(int id)
    {
        GetUserByIdCallCount++;
        return _userToReturn;
    }
    
    public void SaveUser(User user) { }
}

// 사용
var fake = new FakeUserRepository();
fake.SetupGetUserById(new User { Id = 1, FirstName = "John" });
var service = new UserService(fake);

// ✅ Moq 사용: 간결하고 표현력 있음
var mock = new Mock<IUserRepository>();
mock.Setup(r => r.GetUserById(1))
    .Returns(new User { Id = 1, FirstName = "John" });
var service = new UserService(mock.Object);
```

**의존성 주입과 테스트 가능한 설계:**

Mocking을 효과적으로 사용하려면 SOLID 원칙, 특히 의존성 역전 원칙(DIP)을 따라야 합니다:

```csharp
// ❌ 테스트하기 어려운 설계
public class OrderService
{
    public void Process(Order order)
    {
        // 구체 클래스에 직접 의존
        var repository = new SqlOrderRepository();
        repository.Save(order);
    }
}

// ✅ 테스트하기 쉬운 설계 (의존성 주입)
public class OrderService
{
    private readonly IOrderRepository _repository;
    
    // 생성자 주입
    public OrderService(IOrderRepository repository)
    {
        _repository = repository;
    }
    
    public void Process(Order order)
    {
        _repository.Save(order);
    }
}

// 테스트에서는 Mock 주입
var mock = new Mock<IOrderRepository>();
var service = new OrderService(mock.Object);
```

### Mocking의 필요성

**문제 상황:**

다음과 같은 서비스를 테스트한다고 가정해봅시다:

```csharp
public interface IUserRepository
{
    User? GetUserById(int id);
    void SaveUser(User user);
}

public class UserService
{
    private readonly IUserRepository _repository;

    public UserService(IUserRepository repository)
    {
        _repository = repository;
    }

    public string GetUserDisplayName(int userId)
    {
        var user = _repository.GetUserById(userId);
        
        if (user == null)
            return "Unknown User";
        
        return $"{user.FirstName} {user.LastName}";
    }
}

public class User
{
    public int Id { get; set; }
    public string FirstName { get; set; } = string.Empty;
    public string LastName { get; set; } = string.Empty;
}
```

`UserService`를 테스트하려면 `IUserRepository` 구현이 필요합니다. 하지만:
- 실제 데이터베이스에 연결하면 테스트가 느려집니다
- 테스트 데이터 설정이 복잡합니다
- 데이터베이스 상태에 따라 테스트 결과가 달라질 수 있습니다

**해결책: Mocking**

Mock 객체를 사용하면 실제 데이터베이스 없이도 테스트할 수 있습니다!

### Moq 설치

```bash
cd Calculator.Tests
dotnet add package Moq
```

### 기본 Mocking 예제

```csharp
using Moq;
using Xunit;

public class UserServiceTests
{
    [Fact]
    public void GetUserDisplayName_ExistingUser_ReturnsFullName()
    {
        // Arrange
        // Mock 객체 생성
        var mockRepository = new Mock<IUserRepository>();
        
        // Mock의 동작 설정: GetUserById(1) 호출 시 반환값 지정
        mockRepository
            .Setup(repo => repo.GetUserById(1))
            .Returns(new User 
            { 
                Id = 1, 
                FirstName = "홍", 
                LastName = "길동" 
            });
        
        // Mock 객체를 주입하여 서비스 생성
        var service = new UserService(mockRepository.Object);

        // Act
        string displayName = service.GetUserDisplayName(1);

        // Assert
        Assert.Equal("홍 길동", displayName);
    }

    [Fact]
    public void GetUserDisplayName_NonExistingUser_ReturnsUnknown()
    {
        // Arrange
        var mockRepository = new Mock<IUserRepository>();
        
        // GetUserById가 null을 반환하도록 설정
        mockRepository
            .Setup(repo => repo.GetUserById(999))
            .Returns((User?)null);
        
        var service = new UserService(mockRepository.Object);

        // Act
        string displayName = service.GetUserDisplayName(999);

        // Assert
        Assert.Equal("Unknown User", displayName);
    }
}
```

### Moq의 핵심 개념

**1. Setup - Mock 동작 정의:**

```csharp
// 특정 인자에 대해 반환값 지정
mock.Setup(m => m.GetData(42)).Returns("result");

// 모든 인자에 대해 동일한 동작
mock.Setup(m => m.GetData(It.IsAny<int>())).Returns("default");

// 조건부 반환
mock.Setup(m => m.GetUser(It.Is<int>(id => id > 0)))
    .Returns(new User());
```

**2. Verify - 메서드 호출 검증:**

```csharp
// 메서드가 정확히 한 번 호출되었는지 검증
mock.Verify(m => m.SaveData(It.IsAny<string>()), Times.Once);

// 메서드가 호출되지 않았는지 검증
mock.Verify(m => m.DeleteData(It.IsAny<int>()), Times.Never);

// 메서드가 특정 횟수만큼 호출되었는지
mock.Verify(m => m.Log(It.IsAny<string>()), Times.Exactly(3));
```

### 실전 예제: 이메일 서비스 테스트

```csharp
public interface IEmailSender
{
    void SendEmail(string to, string subject, string body);
}

public class OrderService
{
    private readonly IEmailSender _emailSender;

    public OrderService(IEmailSender emailSender)
    {
        _emailSender = emailSender;
    }

    public void PlaceOrder(string customerEmail, string productName)
    {
        // 주문 처리 로직...
        
        // 주문 확인 이메일 발송
        _emailSender.SendEmail(
            customerEmail,
            "주문 확인",
            $"{productName} 주문이 완료되었습니다."
        );
    }
}

public class OrderServiceTests
{
    [Fact]
    public void PlaceOrder_ValidOrder_SendsConfirmationEmail()
    {
        // Arrange
        var mockEmailSender = new Mock<IEmailSender>();
        var orderService = new OrderService(mockEmailSender.Object);
        
        string customerEmail = "customer@example.com";
        string productName = "노트북";

        // Act
        orderService.PlaceOrder(customerEmail, productName);

        // Assert
        // SendEmail이 정확한 인자로 호출되었는지 검증
        mockEmailSender.Verify(
            sender => sender.SendEmail(
                customerEmail,
                "주문 확인",
                "노트북 주문이 완료되었습니다."
            ),
            Times.Once
        );
    }
}
```

### 예외 발생 시뮬레이션

Mock을 사용하여 예외 상황도 테스트할 수 있습니다:

```csharp
public class DataService
{
    private readonly IUserRepository _repository;

    public DataService(IUserRepository repository)
    {
        _repository = repository;
    }

    public bool TrySaveUser(User user)
    {
        try
        {
            _repository.SaveUser(user);
            return true;
        }
        catch (Exception)
        {
            return false;
        }
    }
}

public class DataServiceTests
{
    [Fact]
    public void TrySaveUser_RepositoryThrowsException_ReturnsFalse()
    {
        // Arrange
        var mockRepository = new Mock<IUserRepository>();
        
        // SaveUser 호출 시 예외 발생하도록 설정
        mockRepository
            .Setup(repo => repo.SaveUser(It.IsAny<User>()))
            .Throws<InvalidOperationException>();
        
        var service = new DataService(mockRepository.Object);
        var user = new User { Id = 1, FirstName = "Test", LastName = "User" };

        // Act
        bool result = service.TrySaveUser(user);

        // Assert
        Assert.False(result);
    }
}
```

### It.IsAny<T> - 유연한 매개변수 매칭

특정 값이 아닌 모든 값에 대해 동작을 지정할 때 사용합니다:

```csharp
// 어떤 int 값이든 동일한 결과 반환
mock.Setup(m => m.GetData(It.IsAny<int>())).Returns("data");

// 어떤 문자열이든 저장 허용
mock.Setup(m => m.Save(It.IsAny<string>())).Returns(true);
```

### 속성 Mocking

인터페이스의 속성도 Mock할 수 있습니다:

```csharp
public interface IConfiguration
{
    string DatabaseConnection { get; }
    int MaxRetries { get; }
}

[Fact]
public void UseConfiguration_Test()
{
    // Arrange
    var mockConfig = new Mock<IConfiguration>();
    mockConfig.Setup(c => c.DatabaseConnection).Returns("test-connection");
    mockConfig.Setup(c => c.MaxRetries).Returns(3);

    // 테스트에서 mockConfig.Object 사용
}
```

### Mocking 모범 사례

**1. 인터페이스 기반 설계:**
의존성은 항상 인터페이스로 정의하여 Mock 가능하게 만듭니다.

**2. 하나의 Mock, 하나의 테스트:**
각 테스트에서 새로운 Mock 인스턴스를 생성하여 테스트 간 독립성을 유지합니다.

**3. Verify를 통한 동작 검증:**
중요한 메서드 호출은 반드시 Verify로 확인합니다.

**4. 과도한 Mocking 주의:**
너무 많은 의존성을 Mock해야 한다면, 설계를 재검토해야 할 신호일 수 있습니다.

---

## 24장 정리 및 요약

이 장에서는 .NET 애플리케이션의 품질과 신뢰성을 보장하는 단위 테스트의 이론적 기반부터 실무 적용까지 포괄적으로 학습했습니다. Kent Beck의 SUnit에서 시작된 xUnit 계열 프레임워크의 역사, 테스트 주도 개발(TDD)의 철학, 그리고 현대 소프트웨어 공학에서 단위 테스트가 차지하는 핵심적 위치를 깊이 있게 탐구했습니다.

### 핵심 개념의 재조명

**1. 단위 테스트의 경제적 가치와 철학:**

IBM과 NIST의 연구가 증명하듯이, 소프트웨어 결함의 수정 비용은 발견 시점에 따라 기하급수적으로 증가합니다. 개발 단계에서 발견된 버그는 운영 환경에서 발견된 버그보다 100배 이상 적은 비용으로 수정할 수 있습니다. Microsoft의 Visual Studio 팀과 Google의 사례는 단위 테스트가 프로덕션 버그를 60-90% 감소시킬 수 있음을 보여줍니다.

단위 테스트는 다음과 같은 다층적 가치를 제공합니다:

- **코드 정확성 자동 검증**: 매번 수동으로 확인할 필요 없이 자동화된 검증
- **리팩토링 안전망**: 코드 구조 개선 시 기능 보존 확인
- **살아있는 문서**: 항상 코드와 동기화되는 실행 가능한 예제
- **설계 품질 피드백**: 테스트하기 어려운 코드는 설계 개선이 필요함을 시사
- **조기 버그 발견**: 개발 단계에서 문제를 발견하여 비용 최소화

**2. FIRST 원칙 - 좋은 테스트의 특징:**

Robert C. Martin과 Tim Ottinger가 정리한 FIRST 원칙은 효과적인 단위 테스트의 지표입니다:

- **Fast (빠른 실행)**: 밀리초 단위로 실행되어 개발자가 자주 실행
- **Independent (독립적)**: 테스트 간 의존성 없이 독립 실행
- **Repeatable (반복 가능)**: 동일 입력에 항상 동일 결과
- **Self-validating (자체 검증)**: 수동 확인 불필요한 명확한 결과
- **Timely (적시성)**: 프로덕션 코드와 함께 즉시 작성

**3. 테스트 피라미드와 균형잡힌 전략:**

Mike Cohn의 테스트 피라미드 모델은 효과적인 테스트 전략의 기준을 제시합니다:

- **단위 테스트 (70%)**: 빠르고 안정적이며 유지보수 비용이 낮음
- **통합 테스트 (20%)**: 컴포넌트 간 상호작용 검증
- **E2E 테스트 (10%)**: 사용자 시나리오 전체 검증

역삼각형(Ice Cream Cone) 구조는 테스트가 느리고 불안정하며 유지보수 비용이 높은 안티패턴입니다.

**4. xUnit.net의 아키텍처와 설계 철학:**

Brad Wilson과 Jim Newkirk이 2007년 시작한 xUnit.net은 다음 원칙으로 설계되었습니다:

- **테스트 격리**: 각 테스트마다 클래스의 새 인스턴스 생성
- **확장 가능한 아키텍처**: ITestFramework, ITestCaseOrderer 등 확장 포인트
- **병렬 실행 최적화**: TestCollection을 통한 병렬/순차 실행 제어
- **특성 기반 확장**: [Fact], [Theory] 외 사용자 정의 특성 지원

xUnit은 ASP.NET Core, Entity Framework Core 등 Microsoft의 주요 프로젝트에서 표준으로 채택되었습니다.

**5. AAA 패턴 - 테스트 코드의 문법:**

Bill Wake가 제안한 Arrange-Act-Assert 패턴은 테스트를 명세서처럼 읽을 수 있게 만듭니다:

- **Arrange (준비)**: 테스트 컨텍스트와 사전 조건 설정
- **Act (실행)**: 테스트할 단일 동작 수행 (한 줄)
- **Assert (검증)**: 결과가 예상과 일치하는지 확인

이 패턴은 BDD의 Given-When-Then과 동일한 개념이며, 인간의 인지 과정과 일치합니다.

**6. 매개변수화된 테스트와 데이터 주도 테스트:**

xUnit의 Theory와 데이터 제공 메커니즘은 DRY 원칙을 테스트에 적용합니다:

- **[InlineData]**: 간단한 상수 값으로 여러 시나리오 테스트
- **[MemberData]**: 복잡한 객체나 동적 데이터 제공
- **[ClassData]**: 여러 테스트에서 재사용 가능한 데이터 소스

동등 분할(Equivalence Partitioning)과 경계값 분석(Boundary Value Analysis)을 체계적으로 적용할 수 있습니다.

**7. Moq와 Test Double 패턴:**

Moq는 Castle.Core의 DynamicProxy를 사용하여 런타임에 인터페이스 구현을 생성합니다. Gerard Meszaros의 Test Double 분류에서:

- **Mock**: 기대하는 호출이 발생했는지 검증
- **Stub**: 미리 정의된 응답 제공
- **Spy**: 호출 정보 기록
- **Fake**: 간단한 실제 구현 (InMemoryDatabase)

의존성 역전 원칙(DIP)을 따르면 테스트 가능한 설계가 자연스럽게 달성됩니다.

### 프레임워크와 도구 비교

**xUnit vs NUnit vs MSTest:**

| 측면 | xUnit.net | NUnit | MSTest |
|------|-----------|-------|--------|
| 릴리스 | 2007년 | 2002년 | 2005년 |
| 테스트 격리 | 매번 새 인스턴스 | [SetUp]/[TearDown] | [TestInitialize]/[TestCleanup] |
| 병렬 실행 | 기본 지원 | 기본 지원 | 제한적 |
| Microsoft 채택 | ASP.NET Core, EF Core | - | - |
| 확장성 | 매우 높음 | 높음 | 제한적 |

xUnit이 .NET Core/10의 표준으로 자리잡은 이유는 현대적 설계와 Microsoft 공식 권장 때문입니다.

**Moq vs NSubstitute vs FakeItEasy:**

Moq는 .NET에서 가장 널리 사용되는 Mocking 프레임워크(GitHub 10k+ 스타)로, 간결한 문법과 강력한 기능을 제공합니다.

### 실무 모범 사례와 안티패턴

**모범 사례:**

1. **하나의 테스트, 하나의 개념**: 테스트는 한 가지만 검증
2. **명확한 테스트 이름**: `[Method]_[Scenario]_[ExpectedResult]` 패턴
3. **AAA 패턴 일관성**: 주석으로 세 단계 명시
4. **테스트 독립성**: 테스트 간 상태 공유 금지
5. **의미 있는 Assertion**: 실패 시 원인을 빠르게 파악 가능

**피해야 할 안티패턴:**

1. **Flaky Tests**: 랜덤 값, 현재 시간 의존으로 불안정한 테스트
2. **과도한 Mocking**: 테스트보다 Mock 설정이 더 복잡
3. **Private 메서드 테스트**: Public API를 통해 간접 검증
4. **테스트 상호 의존성**: 실행 순서에 의존하는 테스트
5. **의미 없는 테스트**: 검증 없이 예외만 발생하지 않는지 확인

### 실습 체크리스트

다음 항목을 직접 해보며 학습 내용을 확인하세요:

- [ ] xUnit 테스트 프로젝트 생성 및 `dotnet test` 실행
- [ ] `[Fact]`를 사용한 기본 테스트 작성 및 Assert 메서드 활용
- [ ] AAA 패턴을 적용한 명확한 테스트 구조화
- [ ] `[Theory]`와 `[InlineData]`로 경계값 테스트 작성
- [ ] `[MemberData]`로 복잡한 테스트 데이터 제공
- [ ] Moq를 사용하여 인터페이스 Mocking
- [ ] `Setup()`으로 동작 정의, `Verify()`로 호출 검증
- [ ] TDD Red-Green-Refactor 사이클 실습

### 다음 단계와 심화 학습

**고급 주제:**

- **통합 테스트**: WebApplicationFactory, TestServer를 활용한 ASP.NET Core 통합 테스트
- **테스트 커버리지**: Coverlet, ReportGenerator로 코드 커버리지 측정
- **TDD(Test-Driven Development)**: 테스트 먼저 작성하는 개발 방법론 실천
- **BDD(Behavior-Driven Development)**: SpecFlow를 활용한 행위 주도 개발
- **CI/CD 통합**: GitHub Actions, Azure Pipelines에서 자동 테스트 실행
- **성능 테스트**: BenchmarkDotNet을 활용한 마이크로 벤치마킹
- **변이 테스트(Mutation Testing)**: Stryker.NET으로 테스트 품질 검증

**권장 리소스:**

- **도서**:
  - "Test-Driven Development: By Example" by Kent Beck
  - "xUnit Test Patterns" by Gerard Meszaros
  - "The Art of Unit Testing" by Roy Osherove
  - "Clean Code" by Robert C. Martin (테스트 장)

- **공식 문서**:
  - xUnit.net: https://xunit.net/
  - Moq: https://github.com/moq/moq4
  - .NET 테스트: https://docs.microsoft.com/dotnet/core/testing/

- **커뮤니티**:
  - xUnit GitHub: https://github.com/xunit/xunit
  - Stack Overflow [xunit] 태그
  - .NET Foundation 프로젝트

**실습 과제:**

**초급:**
1. Calculator 클래스에 나머지 연산 기능 추가 및 테스트 작성
2. 문자열 처리 유틸리티 클래스 작성 및 엣지 케이스 테스트
3. Theory로 여러 날짜 형식을 파싱하는 테스트 작성

**중급:**
4. ShoppingCart 클래스에 할인 로직 추가 및 다양한 시나리오 테스트
5. Mock을 사용하여 이메일 발송 서비스 테스트
6. TDD로 FizzBuzz 또는 Prime Number 기능 구현

**고급:**
7. Repository 패턴 구현 및 Moq로 데이터 계층 격리 테스트
8. ASP.NET Core API 엔드포인트의 통합 테스트 작성
9. 테스트 커버리지 80% 이상 달성하고 리포트 생성

### 마무리

단위 테스트는 현대 소프트웨어 공학의 필수 역량입니다. 이 장에서 학습한 내용은:

✅ **이론적 기반**: 테스트의 역사, FIRST 원칙, 테스트 피라미드, 경제적 가치
✅ **실무 기술**: xUnit 프레임워크, AAA 패턴, 매개변수화된 테스트, Moq
✅ **설계 원칙**: 의존성 주입, SOLID 원칙, 테스트 가능한 코드
✅ **모범 사례**: 테스트 명명 규칙, 격리, 독립성, 반복 가능성

25장에서는 Serilog를 활용한 구조화된 로깅과 효과적인 디버깅 기법을 학습하여, 테스트와 함께 소프트웨어 품질을 보장하는 종합적인 역량을 갖추게 됩니다. 단위 테스트, 로깅, 디버깅은 전문 개발자의 3대 핵심 도구입니다!
