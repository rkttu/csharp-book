# 10장. 상속과 다형성

객체지향 프로그래밍(Object-Oriented Programming, OOP)의 세계에서 상속(Inheritance)과 다형성(Polymorphism)은 단순한 프로그래밍 기법을 넘어, 소프트웨어 설계의 근본적인 철학을 구현하는 핵심 메커니즘입니다. 이 두 개념은 1960년대 중반 Simula 67 언어에서 처음 도입된 이후, 현대 소프트웨어 공학의 근간이 되었으며, 코드 재사용성(Reusability), 확장성(Extensibility), 유지보수성(Maintainability)을 획기적으로 향상시켰습니다.

상속은 자연계의 분류학(Taxonomy)에서 영감을 받은 개념으로, 생물학적 분류 체계처럼 소프트웨어 엔티티 간의 "is-a" 관계를 표현합니다. 예를 들어, "개는 동물이다(Dog is-a Animal)"라는 관계를 코드로 직접 표현할 수 있게 해줍니다. 다형성은 그리스어로 "여러 형태(poly = many, morph = form)"를 의미하며, 동일한 인터페이스를 통해 서로 다른 타입의 객체를 일관되게 다룰 수 있게 하는 강력한 추상화 메커니즘입니다.

**객체지향 프로그래밍의 역사적 맥락:**

객체지향 프로그래밍의 개념은 1960년대 노르웨이의 Ole-Johan Dahl과 Kristen Nygaard가 개발한 Simula 67에서 시작되었습니다. 이들은 시뮬레이션 프로그램을 작성하면서 현실 세계의 객체들을 프로그래밍으로 모델링할 필요성을 느꼈고, 클래스(Class)와 상속(Inheritance)이라는 혁신적인 개념을 도입했습니다. 1970년대 Xerox PARC의 Alan Kay는 Smalltalk 언어를 통해 객체지향 개념을 더욱 발전시켰으며, "메시지 전달(Message Passing)"이라는 다형성의 핵심 아이디어를 정립했습니다.

1980년대 Bjarne Stroustrup이 개발한 C++은 객체지향 프로그래밍을 주류로 만들었으며, 1990년대 Java의 등장으로 "순수한" 객체지향 언어의 시대가 열렸습니다. C#은 2000년대 초반 Microsoft가 .NET 플랫폼을 위해 설계한 언어로, Java의 장점을 흡수하면서도 더욱 현대적이고 실용적인 객체지향 기능을 제공합니다.

**SOLID 원칙과의 관계:**

상속과 다형성은 Robert C. Martin이 제시한 SOLID 원칙과 밀접한 관계가 있습니다:

1. **단일 책임 원칙(Single Responsibility Principle, SRP)**: 클래스는 하나의 책임만 가져야 하며, 상속을 통해 관심사를 분리할 수 있습니다
2. **개방-폐쇄 원칙(Open-Closed Principle, OCP)**: 소프트웨어 엔터티는 확장에는 열려 있고 수정에는 닫혀 있어야 하며, 상속과 다형성이 이를 가능하게 합니다
3. **리스코프 치환 원칙(Liskov Substitution Principle, LSP)**: 파생 클래스는 기본 클래스를 대체할 수 있어야 하며, 이는 올바른 상속 설계의 핵심입니다
4. **인터페이스 분리 원칙(Interface Segregation Principle, ISP)**: 클라이언트는 사용하지 않는 인터페이스에 의존하지 않아야 합니다
5. **의존성 역전 원칙(Dependency Inversion Principle, DIP)**: 구체적인 것이 아닌 추상에 의존해야 하며, 인터페이스와 추상 클래스가 이를 실현합니다

이러한 원칙들은 단순히 이론적인 개념이 아니라, 실제 대규모 소프트웨어 프로젝트에서 검증된 모범 사례(Best Practices)입니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 객체지향 프로그래밍의 핵심 메커니즘을 체계적으로 학습하게 됩니다:

- **상속의 이론과 실제**: 기본 클래스와 파생 클래스의 관계, base 키워드의 활용, 메서드 오버라이딩의 메커니즘, 그리고 상속의 올바른 사용법과 주의사항을 깊이 있게 이해합니다. "is-a" 관계의 의미와 상속 계층 구조 설계의 원칙, 그리고 Fragile Base Class Problem 같은 상속의 함정을 배웁니다.

- **추상 클래스의 설계 패턴**: 인스턴스화할 수 없는 추상 클래스의 철학적 배경과 실용적 활용법, 템플릿 메서드 패턴(Template Method Pattern)의 구현, 그리고 추상 클래스와 인터페이스의 선택 기준을 학습합니다.

- **인터페이스 기반 설계**: 계약(Contract) 지향 프로그래밍의 핵심인 인터페이스의 정의와 구현, C#의 다중 인터페이스 구현 메커니즘, 그리고 C# 8.0의 혁신적인 기본 인터페이스 메서드(Default Interface Methods)를 깊이 있게 다룹니다.

- **다형성의 원리와 응용**: 컴파일 타임 다형성(메서드 오버로딩)과 런타임 다형성(메서드 오버라이딩)의 차이, 가상 메서드 테이블(Virtual Method Table, VMT)의 동작 원리, 그리고 다형성을 활용한 디자인 패턴을 실무 관점에서 학습합니다.

**학습 목표:**
- 상속 계층 구조의 올바른 설계와 구현 능력 배양
- 추상화(Abstraction)와 캡슐화(Encapsulation)의 실제 적용
- 인터페이스 기반의 유연한 아키텍처 설계
- 다형성을 활용한 확장 가능한 시스템 구축
- SOLID 원칙을 코드로 구현하는 실무 능력

---

## 10.1 상속 (Inheritance)

상속은 기존 클래스의 특성과 동작을 물려받아 새로운 클래스를 정의하는 객체지향 프로그래밍의 핵심 메커니즘입니다. 이는 단순히 코드를 복사-붙여넣기하는 것이 아니라, 클래스 간의 의미론적(Semantic) 관계를 명시적으로 표현하는 강력한 도구입니다. 상속을 통해 코드 재사용성(Code Reusability)을 극대화하고, 클래스 간의 계층 구조(Class Hierarchy)를 자연스럽게 표현하며, 다형성(Polymorphism)의 기반을 마련할 수 있습니다.

**상속의 이론적 배경:**

상속은 생물학의 분류학(Taxonomy)에서 영감을 받은 개념입니다. 린네(Carl Linnaeus)의 생물 분류 체계가 생물을 계(Kingdom), 문(Phylum), 강(Class), 목(Order), 과(Family), 속(Genus), 종(Species)으로 계층화했듯이, 객체지향 프로그래밍도 클래스를 계층적으로 조직화합니다. 예를 들어, "포유류는 동물의 일종이고, 개는 포유류의 일종이다"라는 생물학적 관계를 "Dog is-a Mammal, Mammal is-a Animal"이라는 상속 관계로 표현할 수 있습니다.

**"is-a" vs "has-a" 관계:**

상속을 올바르게 사용하기 위해서는 "is-a"(~은 ~이다)와 "has-a"(~은 ~을 가지고 있다) 관계를 명확히 구분해야 합니다:

- **is-a 관계 (상속 사용)**: "개는 동물이다(Dog is-a Animal)" - 상속으로 구현
- **has-a 관계 (컴포지션 사용)**: "자동차는 엔진을 가지고 있다(Car has-a Engine)" - 멤버 변수로 구현

이 구분은 단순한 문법적 차이가 아니라, 소프트웨어 설계의 근본적인 철학적 차이를 반영합니다. Gang of Four의 디자인 패턴 책에서는 "상속보다 컴포지션을 선호하라(Favor Composition Over Inheritance)"는 원칙을 강조하는데, 이는 상속의 남용이 코드의 결합도(Coupling)를 높이고 유연성을 떨어뜨릴 수 있기 때문입니다.

**C#의 상속 모델:**

C#은 **단일 상속(Single Inheritance)** 모델을 채택했습니다. 이는 하나의 클래스가 오직 하나의 기본 클래스만 상속할 수 있다는 의미입니다. 이러한 제약은 C++의 다중 상속이 야기하는 "다이아몬드 문제(Diamond Problem)"를 근본적으로 방지합니다. 대신 C#은 **다중 인터페이스 구현(Multiple Interface Implementation)**을 통해 다중 상속의 이점을 취하면서도 복잡성을 줄였습니다.

```
다이아몬드 문제 (C++에서 발생):
        A
       / \
      B   C
       \ /
        D
D가 B와 C를 모두 상속하면, A의 멤버가 중복되는 문제 발생
C#은 단일 상속으로 이 문제를 원천적으로 차단
```

**상속의 메모리 레이아웃:**

상속된 클래스의 인스턴스는 메모리에서 기본 클래스의 필드를 먼저 배치하고, 그 다음 파생 클래스의 필드를 배치하는 순차적 구조를 가집니다:

```
[기본 클래스 필드들] [파생 클래스 필드들] [가상 메서드 테이블 포인터]
```

이러한 레이아웃 덕분에 파생 클래스의 인스턴스를 기본 클래스 타입으로 참조할 때, 추가적인 변환 없이 기본 클래스의 멤버에 직접 접근할 수 있습니다.

### 10.1.1 기본 클래스와 파생 클래스

기본 클래스(Base Class, 또는 부모 클래스, 슈퍼 클래스)는 상속을 제공하는 클래스이고, 파생 클래스(Derived Class, 또는 자식 클래스, 서브 클래스)는 상속을 받는 클래스입니다. 이러한 용어는 객체지향 언어마다 다르게 사용되지만, C#에서는 주로 "기본 클래스"와 "파생 클래스"라는 용어를 사용합니다.

**상속 구문의 이해:**

C#에서 상속은 콜론(`:`) 기호를 사용하여 표현합니다. 이는 Pascal 계열 언어의 영향을 받은 문법으로, Java의 `extends` 키워드보다 간결하면서도 명확합니다:

```csharp
class 파생클래스명 : 기본클래스명
{
    // 파생 클래스의 추가 멤버
}
```

**기본적인 상속 예제:**

```csharp
// 기본 클래스 - 동물의 공통 특성 정의
class Animal
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public void Eat()
    {
        Console.WriteLine($"{Name}이(가) 먹이를 먹습니다.");
    }
    
    public void Sleep()
    {
        Console.WriteLine($"{Name}이(가) 잠을 잡니다.");
    }
}

// 파생 클래스 - Dog는 Animal의 모든 특성을 상속받고 추가 기능을 정의
class Dog : Animal
{
    public string Breed { get; set; }  // Dog만의 속성
    
    // Dog만의 메서드
    public void Bark()
    {
        Console.WriteLine($"{Name}이(가) 짖습니다: 멍멍!");
    }
    
    public void WagTail()
    {
        Console.WriteLine($"{Name}이(가) 꼬리를 흔듭니다.");
    }
}

// 또 다른 파생 클래스
class Cat : Animal
{
    public string FurColor { get; set; }
    
    public void Meow()
    {
        Console.WriteLine($"{Name}이(가) 웁니다: 야옹~");
    }
    
    public void Purr()
    {
        Console.WriteLine($"{Name}이(가) 가르랑거립니다.");
    }
}

// 사용 예제
Dog myDog = new Dog();
myDog.Name = "바둑이";
myDog.Age = 3;
myDog.Breed = "진돗개";
myDog.Eat();      // Animal로부터 상속받은 메서드
myDog.Sleep();    // Animal로부터 상속받은 메서드
myDog.Bark();     // Dog 클래스 고유의 메서드
myDog.WagTail();  // Dog 클래스 고유의 메서드

Cat myCat = new Cat();
myCat.Name = "나비";
myCat.Age = 2;
myCat.FurColor = "하얀색";
myCat.Eat();      // Animal로부터 상속받은 메서드
myCat.Meow();     // Cat 클래스 고유의 메서드

// 출력:
// 바둑이이(가) 먹이를 먹습니다.
// 바둑이이(가) 잠을 잡니다.
// 바둑이이(가) 짖습니다: 멍멍!
// 바둑이이(가) 꼬리를 흔듭니다.
// 나비이(가) 먹이를 먹습니다.
// 나비이(가) 웁니다: 야옹~
```

**멤버 접근성과 상속:**

상속에서 접근 제한자(Access Modifier)는 파생 클래스가 기본 클래스의 어떤 멤버에 접근할 수 있는지 결정합니다:

| 접근 제한자 | 동일 클래스 | 파생 클래스 | 동일 어셈블리 | 외부 어셈블리 |
|------------|-----------|------------|-------------|-------------|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ❌ | ❌ |
| `internal` | ✅ | ✅* | ✅ | ❌ |
| `protected internal` | ✅ | ✅ | ✅ | ✅ (파생 클래스만) |
| `private protected` | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

*internal: 동일 어셈블리 내에서만

```csharp
class Animal
{
    public string Name { get; set; }         // 모든 곳에서 접근 가능
    protected int age;                        // 파생 클래스에서 접근 가능
    private string secretIdentity;            // Animal 클래스 내부에서만 접근 가능
    
    public void PublicMethod() { }
    protected void ProtectedMethod() { }      // 파생 클래스에서 호출 가능
    private void PrivateMethod() { }          // 파생 클래스에서 호출 불가
}

class Dog : Animal
{
    public void AccessMembers()
    {
        Console.WriteLine(Name);              // ✅ OK: public
        Console.WriteLine(age);               // ✅ OK: protected
        // Console.WriteLine(secretIdentity); // ❌ Error: private
        
        PublicMethod();                       // ✅ OK
        ProtectedMethod();                    // ✅ OK: protected
        // PrivateMethod();                   // ❌ Error: private
    }
}
```

**object 클래스: 모든 타입의 조상**

C#의 모든 타입은 명시적이든 암묵적이든 `System.Object` 클래스(별칭 `object`)를 상속받습니다. 이는 .NET의 통합 타입 시스템(Unified Type System)의 핵심입니다:

```csharp
// 명시적 object 상속 (일반적으로 생략)
class MyClass : object
{
}

// 암묵적 object 상속 (일반적인 방식)
class MyClass
{
    // 실제로는 object를 상속받고 있음
}
```

`object` 클래스는 다음과 같은 기본 메서드를 제공하며, 모든 클래스가 이를 상속받습니다:

```csharp
class Object
{
    public virtual bool Equals(object obj);          // 객체 동등성 비교
    public virtual int GetHashCode();                // 해시 코드 생성
    public virtual string ToString();                // 문자열 표현 반환
    public Type GetType();                           // 런타임 타입 정보
    protected virtual void Finalize();               // 소멸자 (거의 사용 안 함)
    protected object MemberwiseClone();              // 얕은 복사
}
```

**실용적인 object 메서드 오버라이딩:**

```csharp
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    // ToString 오버라이딩 - 객체의 문자열 표현 정의
    public override string ToString()
    {
        return $"Person: {Name}, Age: {Age}";
    }
    
    // Equals 오버라이딩 - 동등성 비교 논리 정의
    public override bool Equals(object obj)
    {
        if (obj == null || GetType() != obj.GetType())
            return false;
        
        Person other = (Person)obj;
        return Name == other.Name && Age == other.Age;
    }
    
    // GetHashCode 오버라이딩 - Equals와 일관성 유지 필요
    public override int GetHashCode()
    {
        return HashCode.Combine(Name, Age);
    }
}

Person p1 = new Person { Name = "김철수", Age = 30 };
Person p2 = new Person { Name = "김철수", Age = 30 };

Console.WriteLine(p1.ToString());        // Person: 김철수, Age: 30
Console.WriteLine(p1.Equals(p2));        // True (내용이 같음)
Console.WriteLine(p1 == p2);             // False (참조가 다름)
```

**상속의 주요 특징과 제약사항:**

1. **단일 상속 제약**: C#은 클래스의 다중 상속을 지원하지 않습니다. 하나의 클래스는 오직 하나의 기본 클래스만 상속할 수 있습니다.

```csharp
// ❌ 오류: 다중 상속 불가
class FlyingFish : Fish, Bird  // Compile Error!
{
}

// ✅ 올바른 방법: 인터페이스로 해결
class FlyingFish : Fish, IFlyable
{
    public void Fly() { /* 구현 */ }
}
```

2. **sealed 클래스**: `sealed` 키워드로 표시된 클래스는 상속할 수 없습니다. 이는 클래스 설계자가 의도한 대로만 사용되도록 보장합니다.

```csharp
sealed class FinalClass
{
    // 이 클래스는 더 이상 상속될 수 없음
}

// ❌ 오류
class DerivedClass : FinalClass  // Compile Error!
{
}
```

C# BCL(Base Class Library)의 많은 클래스가 sealed로 선언되어 있습니다:
- `System.String`
- `System.Int32`, `System.Double` 등 모든 값 타입의 래퍼 클래스
- `System.ValueType`

3. **암묵적 object 상속**: 명시적으로 기본 클래스를 지정하지 않으면 자동으로 `object`를 상속합니다.

4. **생성자 체이닝**: 파생 클래스의 인스턴스가 생성될 때, 기본 클래스의 생성자가 먼저 실행됩니다. 이는 다음 섹션에서 자세히 다룹니다.

### 10.1.2 base 키워드

`base` 키워드는 파생 클래스에서 기본 클래스의 멤버에 명시적으로 접근할 때 사용하는 특별한 참조입니다. 이는 Java의 `super` 키워드와 유사한 역할을 하며, 주로 두 가지 주요 상황에서 사용됩니다: 기본 클래스의 생성자 호출과 기본 클래스의 멤버(메서드, 속성) 접근입니다.

**생성자 체이닝과 초기화 순서:**

객체지향 프로그래밍에서 파생 클래스의 객체가 생성될 때, 기본 클래스의 생성자가 먼저 실행된 후 파생 클래스의 생성자가 실행됩니다. 이를 **생성자 체이닝(Constructor Chaining)**이라고 합니다. 이는 논리적으로 당연한 순서인데, 파생 클래스가 기본 클래스의 기능에 의존하므로 기본 클래스가 먼저 완전히 초기화되어야 하기 때문입니다.

**생성자 호출 순서의 이해:**

```
[기본 클래스 생성자] → [파생 클래스 생성자]

더 깊은 상속 계층에서는:
[최상위 기본 클래스] → [중간 클래스] → [최종 파생 클래스]
```

**base를 사용한 생성자 호출:**

```csharp
class Animal
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    // 기본 생성자
    public Animal()
    {
        Console.WriteLine("Animal 기본 생성자 실행");
    }
    
    // 매개변수가 있는 생성자
    public Animal(string name, int age)
    {
        Name = name;
        Age = age;
        Console.WriteLine($"Animal 생성자: {name}, {age}세");
    }
    
    public virtual void MakeSound()
    {
        Console.WriteLine($"{Name}: 동물 소리");
    }
}

class Dog : Animal
{
    public string Breed { get; set; }
    
    // 기본 생성자 - 명시적으로 기본 클래스 생성자 호출
    public Dog() : base()
    {
        Console.WriteLine("Dog 기본 생성자 실행");
    }
    
    // 매개변수 생성자 - base를 사용하여 기본 클래스 생성자에 값 전달
    public Dog(string name, int age, string breed) : base(name, age)
    {
        Breed = breed;
        Console.WriteLine($"Dog 생성자: 품종 {breed}");
    }
    
    public override void MakeSound()
    {
        // base를 사용하여 기본 클래스 메서드 호출
        base.MakeSound();
        Console.WriteLine($"{Name}: 멍멍!");
    }
}

// 사용 예제 1: 기본 생성자
Console.WriteLine("=== 기본 생성자 사용 ===");
Dog dog1 = new Dog();
// 출력:
// Animal 기본 생성자 실행
// Dog 기본 생성자 실행

Console.WriteLine("\n=== 매개변수 생성자 사용 ===");
Dog dog2 = new Dog("바둑이", 3, "진돗개");
// 출력:
// Animal 생성자: 바둑이, 3세
// Dog 생성자: 품종 진돗개

Console.WriteLine("\n=== 메서드 호출 ===");
dog2.MakeSound();
// 출력:
// 바둑이: 동물 소리
// 바둑이: 멍멍!
```

**base의 필요성과 활용 시나리오:**

1. **기본 클래스 생성자 명시적 호출**: 기본 클래스에 매개변수가 있는 생성자만 있고 기본 생성자가 없는 경우, 파생 클래스는 반드시 `base(...)`를 사용해야 합니다.

```csharp
class Vehicle
{
    public string Model { get; set; }
    
    // 기본 생성자가 없음 - 오직 매개변수 생성자만 존재
    public Vehicle(string model)
    {
        Model = model;
        Console.WriteLine($"Vehicle 생성: {model}");
    }
}

class Car : Vehicle
{
    public int Seats { get; set; }
    
    // ❌ 오류: 기본 클래스에 매개변수 없는 생성자가 없음
    // public Car() { }
    
    // ✅ 올바른 방법: base를 사용하여 기본 클래스 생성자 호출
    public Car(string model, int seats) : base(model)
    {
        Seats = seats;
        Console.WriteLine($"Car 생성: {seats}인승");
    }
}
```

2. **기본 클래스 메서드 확장**: 기본 클래스의 메서드 동작을 완전히 대체하지 않고, 추가 기능을 덧붙일 때 사용합니다.

```csharp
class Logger
{
    protected string prefix = "[LOG]";
    
    public virtual void Log(string message)
    {
        Console.WriteLine($"{prefix} {DateTime.Now:HH:mm:ss} - {message}");
    }
}

class DetailedLogger : Logger
{
    public override void Log(string message)
    {
        // 기본 클래스의 Log 메서드 호출 (기본 동작 유지)
        base.Log(message);
        
        // 추가 상세 정보 출력
        Console.WriteLine($"    Thread: {Thread.CurrentThread.ManagedThreadId}");
        Console.WriteLine($"    Source: {System.Reflection.MethodBase.GetCurrentMethod()?.DeclaringType?.Name}");
    }
}

DetailedLogger logger = new DetailedLogger();
logger.Log("시스템 초기화 완료");
// 출력:
// [LOG] 14:30:45 - 시스템 초기화 완료
//     Thread: 1
//     Source: DetailedLogger
```

3. **속성 접근**: 기본 클래스와 파생 클래스에 같은 이름의 멤버가 있을 때, `base`로 기본 클래스 멤버에 명확히 접근할 수 있습니다.

```csharp
class BaseClass
{
    public string Message { get; set; } = "기본 클래스 메시지";
    
    public void ShowMessage()
    {
        Console.WriteLine($"BaseClass: {Message}");
    }
}

class DerivedClass : BaseClass
{
    // new 키워드로 기본 클래스 멤버를 숨김 (hiding)
    public new string Message { get; set; } = "파생 클래스 메시지";
    
    public void ShowBothMessages()
    {
        Console.WriteLine($"파생 클래스 Message: {Message}");
        Console.WriteLine($"기본 클래스 Message: {base.Message}");
        
        // 기본 클래스의 메서드 호출
        base.ShowMessage();
    }
}

DerivedClass obj = new DerivedClass();
obj.ShowBothMessages();
// 출력:
// 파생 클래스 Message: 파생 클래스 메시지
// 기본 클래스 Message: 기본 클래스 메시지
// BaseClass: 기본 클래스 메시지
```

**복잡한 상속 계층에서의 base:**

```csharp
class Organism
{
    public string Species { get; set; }
    
    public Organism(string species)
    {
        Species = species;
        Console.WriteLine($"[1] Organism 생성: {species}");
    }
    
    public virtual void Live()
    {
        Console.WriteLine($"{Species}가(이) 생명 활동을 합니다.");
    }
}

class Animal : Organism
{
    public int LegCount { get; set; }
    
    public Animal(string species, int legCount) : base(species)
    {
        LegCount = legCount;
        Console.WriteLine($"[2] Animal 생성: {legCount}개의 다리");
    }
    
    public override void Live()
    {
        base.Live();
        Console.WriteLine($"{Species}가(이) 움직입니다.");
    }
}

class Mammal : Animal
{
    public bool HasFur { get; set; }
    
    public Mammal(string species, int legCount, bool hasFur) : base(species, legCount)
    {
        HasFur = hasFur;
        Console.WriteLine($"[3] Mammal 생성: 털 {(hasFur ? "있음" : "없음")}");
    }
    
    public override void Live()
    {
        base.Live();
        Console.WriteLine($"{Species}가(이) 체온을 유지합니다.");
    }
}

Console.WriteLine("=== 객체 생성 ===");
Mammal dog = new Mammal("개", 4, true);
// 출력:
// [1] Organism 생성: 개
// [2] Animal 생성: 4개의 다리
// [3] Mammal 생성: 털 있음

Console.WriteLine("\n=== Live 메서드 호출 ===");
dog.Live();
// 출력:
// 개가(이) 생명 활동을 합니다.
// 개가(이) 움직입니다.
// 개가(이) 체온을 유지합니다.
```

**base 키워드 사용 시 주의사항:**

1. **생성자에서만 사용 가능한 base(...)**: 생성자 체이닝에서 `base(...)`는 생성자 본문 전에, 초기화자 위치에서만 사용할 수 있습니다.

```csharp
// ✅ 올바른 사용
public Dog(string name) : base(name)
{
}

// ❌ 오류: 생성자 본문 내에서는 base 생성자를 호출할 수 없음
public Dog(string name)
{
    base(name);  // Compile Error!
}
```

2. **정적 멤버 접근 불가**: `base` 키워드는 인스턴스 멤버에만 접근할 수 있고, 정적(static) 멤버에는 사용할 수 없습니다.

```csharp
class BaseClass
{
    public static void StaticMethod() { }
    public void InstanceMethod() { }
}

class DerivedClass : BaseClass
{
    public void TestAccess()
    {
        base.InstanceMethod();     // ✅ OK
        // base.StaticMethod();    // ❌ Error: 정적 멤버는 base로 접근 불가
        BaseClass.StaticMethod();  // ✅ OK: 클래스 이름으로 접근
    }
}
```

3. **순환 참조 방지**: 기본 클래스와 파생 클래스의 메서드가 서로를 호출하면 무한 재귀가 발생할 수 있습니다.

```csharp
// ⚠️ 위험: 무한 재귀 가능성
class BaseClass
{
    public virtual void Method()
    {
        // 파생 클래스의 오버라이드된 메서드가 호출됨 (다형성)
    }
}

class DerivedClass : BaseClass
{
    public override void Method()
    {
        base.Method();  // 기본 클래스 호출
        // 추가 로직...
    }
}
```

### 10.1.3 메서드 오버라이딩

메서드 오버라이딩(Method Overriding)은 파생 클래스가 기본 클래스의 가상 메서드(Virtual Method)나 추상 메서드(Abstract Method)를 재정의하여, 같은 시그니처(이름, 매개변수, 반환 타입)를 가지면서도 다른 구현을 제공하는 다형성의 핵심 메커니즘입니다. 이는 컴파일 타임이 아닌 **런타임(Runtime)**에 실제로 호출될 메서드가 결정되는 **동적 바인딩(Dynamic Binding)** 또는 **후기 바인딩(Late Binding)**의 대표적인 예입니다.

**메서드 오버라이딩 vs 메서드 오버로딩:**

이 두 개념은 이름이 비슷하지만 완전히 다른 메커니즘입니다:

| 특성 | 오버라이딩 (Overriding) | 오버로딩 (Overloading) |
|------|----------------------|---------------------|
| 정의 | 기본 클래스 메서드 재정의 | 같은 이름의 다른 매개변수 메서드 |
| 시그니처 | 동일해야 함 | 달라야 함 |
| 키워드 | `virtual`, `override` | 불필요 |
| 바인딩 | 런타임 (동적 바인딩) | 컴파일 타임 (정적 바인딩) |
| 다형성 | 런타임 다형성 | 컴파일 타임 다형성 |
| 상속 관계 | 필요 | 불필요 (같은 클래스 내 가능) |

**가상 메서드 테이블(Virtual Method Table, VMT)의 동작 원리:**

C#은 메서드 오버라이딩을 구현하기 위해 각 클래스마다 **가상 메서드 테이블(VMT)** 또는 **vtable**을 생성합니다. 이는 C++의 구현 방식과 유사하며, .NET CLR(Common Language Runtime)의 핵심 메커니즘입니다:

```
[객체 인스턴스]
    ├─ [타입 정보 포인터] → [타입 메타데이터]
    ├─ [동기화 블록 인덱스]          ├─ Method Table
    └─ [인스턴스 필드들]              │   ├─ Method 1 → 실제 구현 주소
                                      │   ├─ Method 2 → 실제 구현 주소
                                      │   └─ Virtual Method → 오버라이드된 구현
```

런타임에 가상 메서드가 호출되면:
1. 객체의 실제 타입 확인 (런타임 타입)
2. 해당 타입의 Method Table 조회
3. 메서드 엔트리에서 실제 구현 주소 획득
4. 해당 주소의 코드 실행

이러한 간접 참조(Indirection) 때문에 가상 메서드 호출은 일반 메서드 호출보다 약간 느리지만, 현대 CPU의 분기 예측(Branch Prediction) 덕분에 실무에서는 성능 차이가 미미합니다.

**virtual, override, new 키워드의 이해:**

```csharp
class Shape
{
    // virtual: 이 메서드는 파생 클래스에서 오버라이드될 수 있음
    public virtual double GetArea()
    {
        return 0;
    }
    
    public virtual void Draw()
    {
        Console.WriteLine("도형을 그립니다.");
    }
    
    // virtual 없는 일반 메서드 - 오버라이드 불가
    public void Info()
    {
        Console.WriteLine("Shape 클래스");
    }
}

class Circle : Shape
{
    public double Radius { get; set; }
    
    public Circle(double radius)
    {
        Radius = radius;
    }
    
    // override: 기본 클래스의 virtual 메서드를 재정의
    public override double GetArea()
    {
        return Math.PI * Radius * Radius;
    }
    
    public override void Draw()
    {
        Console.WriteLine($"반지름 {Radius}인 원을 그립니다.");
    }
}

class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    public Rectangle(double width, double height)
    {
        Width = width;
        Height = height;
    }
    
    public override double GetArea()
    {
        return Width * Height;
    }
    
    public override void Draw()
    {
        Console.WriteLine($"{Width}x{Height} 사각형을 그립니다.");
    }
}

// 다형성 시연
Shape[] shapes = new Shape[]
{
    new Circle(5),
    new Rectangle(4, 6),
    new Circle(3)
};

Console.WriteLine("=== 모든 도형 그리기 ===");
foreach (Shape shape in shapes)
{
    shape.Draw();  // 각 객체의 실제 타입에 따라 다른 메서드 호출
    Console.WriteLine($"넓이: {shape.GetArea():F2}\n");
}

// 출력:
// === 모든 도형 그리기 ===
// 반지름 5인 원을 그립니다.
// 넓이: 78.54
//
// 4x6 사각형을 그립니다.
// 넓이: 24.00
//
// 반지름 3인 원을 그립니다.
// 넓이: 28.27
```

**메서드 숨김(Method Hiding) vs 오버라이딩:**

`new` 키워드를 사용하면 기본 클래스의 메서드를 숨길(hide) 수 있지만, 이는 오버라이딩과 다릅니다:

```csharp
class BaseClass
{
    public virtual void Method1()
    {
        Console.WriteLine("BaseClass.Method1");
    }
    
    public void Method2()
    {
        Console.WriteLine("BaseClass.Method2");
    }
}

class DerivedOverride : BaseClass
{
    // 오버라이딩: 다형성 동작
    public override void Method1()
    {
        Console.WriteLine("DerivedOverride.Method1");
    }
}

class DerivedHiding : BaseClass
{
    // 숨김: 다형성 동작하지 않음
    public new void Method2()
    {
        Console.WriteLine("DerivedHiding.Method2");
    }
}

// 테스트
Console.WriteLine("=== 오버라이딩 (다형성 O) ===");
BaseClass obj1 = new DerivedOverride();
obj1.Method1();  // 출력: DerivedOverride.Method1

Console.WriteLine("\n=== 숨김 (다형성 X) ===");
BaseClass obj2 = new DerivedHiding();
obj2.Method2();  // 출력: BaseClass.Method2 (파생 클래스 버전 호출 안됨!)

DerivedHiding obj3 = new DerivedHiding();
obj3.Method2();  // 출력: DerivedHiding.Method2
```

**sealed 키워드로 오버라이딩 방지:**

`sealed override`를 사용하면 더 이상 오버라이드되지 않도록 "봉인"할 수 있습니다:

```csharp
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("동물 소리");
    }
}

class Dog : Animal
{
    // sealed override: 이후 파생 클래스에서 더 이상 오버라이드 불가
    public sealed override void MakeSound()
    {
        Console.WriteLine("멍멍!");
    }
}

class Puppy : Dog
{
    // ❌ 오류: sealed 메서드는 오버라이드 불가
    // public override void MakeSound() { }
}
```

**리스코프 치환 원칙(Liskov Substitution Principle, LSP)과 오버라이딩:**

Barbara Liskov가 1987년에 제시한 이 원칙은 "파생 클래스의 객체는 기본 클래스의 객체를 대체할 수 있어야 한다"는 의미입니다. 올바른 오버라이딩은 LSP를 준수해야 합니다:

```csharp
// ✅ LSP 준수: 파생 클래스가 기본 클래스의 계약을 지킴
class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
    
    public virtual int GetArea()
    {
        return Width * Height;
    }
}

// ❌ LSP 위반 예제 (유명한 직사각형-정사각형 문제)
class Square : Rectangle
{
    // 정사각형은 너비와 높이가 같아야 하므로...
    public override int Width
    {
        get => base.Width;
        set { base.Width = value; base.Height = value; }
    }
    
    public override int Height
    {
        get => base.Height;
        set { base.Width = value; base.Height = value; }
    }
}

// 문제 상황
void TestRectangle(Rectangle rect)
{
    rect.Width = 5;
    rect.Height = 10;
    Console.WriteLine($"예상 넓이: 50, 실제 넓이: {rect.GetArea()}");
    // Square를 전달하면: 예상 50, 실제 100 - LSP 위반!
}
```

**오버라이딩 규칙과 제약사항:**

1. **시그니처 동일성**: 메서드 이름, 매개변수 목록, 반환 타입이 완전히 일치해야 합니다.

```csharp
class Base
{
    public virtual int Calculate(int a, int b)
    {
        return a + b;
    }
}

class Derived : Base
{
    // ✅ 올바른 오버라이드
    public override int Calculate(int a, int b)
    {
        return a * b;
    }
    
    // ❌ 오류: 반환 타입이 다름
    // public override double Calculate(int a, int b) { }
    
    // ❌ 오류: 매개변수가 다름
    // public override int Calculate(int a) { }
}
```

2. **접근 제한자 규칙**: 오버라이드된 메서드의 접근성은 기본 클래스와 같거나 더 넓어야 합니다.

```csharp
class Base
{
    protected virtual void Method() { }
}

class Derived : Base
{
    // ✅ OK: protected → public (더 넓어짐)
    public override void Method() { }
    
    // ❌ Error: protected → private (좁아짐)
    // private override void Method() { }
}
```

3. **static 메서드 오버라이딩 불가**: 정적 메서드는 타입에 속하지 인스턴스에 속하지 않으므로 오버라이드할 수 없습니다.

```csharp
class Base
{
    public static void StaticMethod() { }
    // ❌ Error: static 메서드는 virtual일 수 없음
    // public static virtual void Method() { }
}
```

**실용적인 오버라이딩 패턴:**

```csharp
// 템플릿 메서드 패턴 (Template Method Pattern)
abstract class DataProcessor
{
    // 템플릿 메서드 - 전체 알고리즘 구조 정의
    public void Process()
    {
        LoadData();
        ProcessData();
        SaveData();
    }
    
    protected abstract void LoadData();    // 파생 클래스가 구현
    protected abstract void ProcessData(); // 파생 클래스가 구현
    protected abstract void SaveData();    // 파생 클래스가 구현
}

class CSVProcessor : DataProcessor
{
    protected override void LoadData()
    {
        Console.WriteLine("CSV 파일에서 데이터 로드");
    }
    
    protected override void ProcessData()
    {
        Console.WriteLine("CSV 데이터 처리");
    }
    
    protected override void SaveData()
    {
        Console.WriteLine("처리된 데이터를 CSV로 저장");
    }
}

class XMLProcessor : DataProcessor
{
    protected override void LoadData()
    {
        Console.WriteLine("XML 파일에서 데이터 로드");
    }
    
    protected override void ProcessData()
    {
        Console.WriteLine("XML 데이터 처리");
    }
    
    protected override void SaveData()
    {
        Console.WriteLine("처리된 데이터를 XML로 저장");
    }
}

// 사용
DataProcessor processor = new CSVProcessor();
processor.Process();
// 출력:
// CSV 파일에서 데이터 로드
// CSV 데이터 처리
// 처리된 데이터를 CSV로 저장
```

---

## 10.2 추상 클래스 (Abstract Class)

추상 클래스(Abstract Class)는 인스턴스를 직접 생성할 수 없으며, 오직 상속을 통해서만 사용할 수 있는 특별한 형태의 클래스입니다. 이는 "불완전한" 클래스로서, 파생 클래스가 반드시 구현해야 할 "계약"을 정의하는 동시에, 공통 기능의 구현을 제공할 수 있는 강력한 추상화 도구입니다. 추상 클래스는 Erich Gamma 등이 저술한 "Design Patterns" 책에서 소개된 **템플릿 메서드 패턴(Template Method Pattern)**의 핵심 구현 메커니즘입니다.

**추상 클래스의 철학적 배경:**

추상 클래스는 플라톤의 이데아론(Theory of Forms)과 유사한 개념입니다. "완벽한 원"의 이데아는 실제로 존재하지 않지만, 모든 구체적인 원의 본질을 정의합니다. 마찬가지로 추상 클래스는 실체화될 수 없지만, 파생 클래스들의 공통 본질과 행위를 정의합니다.

프로그래밍 관점에서, 추상 클래스는 "무엇을(What)" 해야 하는지 정의하면서도 "어떻게(How)" 할지는 파생 클래스에 위임합니다. 이는 **개방-폐쇄 원칙(Open-Closed Principle)**의 실천이며, 프레임워크 설계의 핵심 기법입니다.

**추상 클래스 vs 구체 클래스 vs 인터페이스:**

| 특성 | 추상 클래스 | 구체 클래스 | 인터페이스 |
|------|-----------|-----------|---------|
| 인스턴스 생성 | ❌ | ✅ | ❌ |
| 추상 메서드 | ✅ | ❌ | ✅ (구현 없음) |
| 구현된 메서드 | ✅ | ✅ | ✅ (C# 8.0+) |
| 필드 | ✅ | ✅ | ❌ |
| 생성자 | ✅ | ✅ | ❌ |
| 접근 제한자 | ✅ (다양) | ✅ (다양) | ✅ (public만, C# 8.0+ 변화) |
| 다중 상속/구현 | ❌ (단일) | ❌ (단일) | ✅ (다중) |
| 상태 유지 | ✅ | ✅ | ❌ |

**추상 클래스의 선언과 구조:**

```csharp
// abstract 키워드로 추상 클래스 선언
abstract class Vehicle
{
    // 인스턴스 필드 - 추상 클래스도 상태를 가질 수 있음
    private string _model;
    protected int productionYear;
    
    // 속성
    public string Model 
    { 
        get => _model;
        set => _model = value ?? throw new ArgumentNullException(nameof(value));
    }
    
    // 생성자 - 추상 클래스도 생성자를 가질 수 있음 (파생 클래스에서 호출)
    protected Vehicle(string model)
    {
        Model = model;
        productionYear = DateTime.Now.Year;
        Console.WriteLine($"Vehicle 기본 구성 완료: {model}");
    }
    
    // 구현된 일반 메서드 - 모든 파생 클래스가 공통으로 사용
    public void DisplayInfo()
    {
        Console.WriteLine($"모델: {Model}");
        Console.WriteLine($"제작연도: {productionYear}");
        Console.WriteLine($"타입: {this.GetType().Name}");
    }
    
    // 가상 메서드 - 기본 구현 제공, 오버라이드 선택적
    public virtual void Maintain()
    {
        Console.WriteLine($"{Model} 정기 점검 수행");
    }
    
    // 추상 메서드 - 구현 없음, 파생 클래스에서 반드시 구현
    public abstract void Start();
    public abstract void Stop();
    public abstract double GetFuelEfficiency();
    
    // 추상 속성
    public abstract int MaxSpeed { get; }
}

class Car : Vehicle
{
    public int Seats { get; set; }
    
    public Car(string model, int seats) : base(model)
    {
        Seats = seats;
    }
    
    // 추상 메서드 구현 - override 키워드 필수
    public override void Start()
    {
        Console.WriteLine($"{Model} 자동차 시동: 엔진 가동");
    }
    
    public override void Stop()
    {
        Console.WriteLine($"{Model} 자동차 정지: 엔진 off");
    }
    
    public override double GetFuelEfficiency()
    {
        return 15.5;  // km/L
    }
    
    public override int MaxSpeed => 180;  // km/h
    
    // 가상 메서드 오버라이드 - 선택적
    public override void Maintain()
    {
        base.Maintain();  // 기본 동작 수행
        Console.WriteLine("타이어 공기압 확인");
        Console.WriteLine("엔진 오일 점검");
    }
}

class Motorcycle : Vehicle
{
    public bool HasSidecar { get; set; }
    
    public Motorcycle(string model, bool hasSidecar) : base(model)
    {
        HasSidecar = hasSidecar;
    }
    
    public override void Start()
    {
        Console.WriteLine($"{Model} 오토바이 시동: 킥스타트");
    }
    
    public override void Stop()
    {
        Console.WriteLine($"{Model} 오토바이 정지: 시동 off");
    }
    
    public override double GetFuelEfficiency()
    {
        return 30.0;  // km/L - 오토바이가 더 효율적
    }
    
    public override int MaxSpeed => 140;  // km/h
}

class ElectricCar : Vehicle
{
    public double BatteryCapacity { get; set; }  // kWh
    
    public ElectricCar(string model, double batteryCapacity) : base(model)
    {
        BatteryCapacity = batteryCapacity;
    }
    
    public override void Start()
    {
        Console.WriteLine($"{Model} 전기차 시동: 시스템 활성화");
    }
    
    public override void Stop()
    {
        Console.WriteLine($"{Model} 전기차 정지: 시스템 대기");
    }
    
    public override double GetFuelEfficiency()
    {
        return BatteryCapacity / 100.0 * 500;  // 주행거리 (km)
    }
    
    public override int MaxSpeed => 200;  // km/h
    
    public override void Maintain()
    {
        base.Maintain();
        Console.WriteLine("배터리 상태 점검");
        Console.WriteLine("전기 모터 점검");
    }
}

// 사용 예제
Console.WriteLine("=== 차량 생성 ===");
// Vehicle vehicle = new Vehicle("테스트");  // ❌ 오류! 추상 클래스는 인스턴스화 불가

Car car = new Car("소나타", 5);
Motorcycle motorcycle = new Motorcycle("혼다", false);
ElectricCar tesla = new ElectricCar("Model 3", 75.0);

Console.WriteLine("\n=== 다형성을 통한 처리 ===");
Vehicle[] vehicles = { car, motorcycle, tesla };

foreach (Vehicle v in vehicles)
{
    Console.WriteLine($"\n--- {v.Model} ---");
    v.DisplayInfo();
    v.Start();
    Console.WriteLine($"최고속도: {v.MaxSpeed} km/h");
    Console.WriteLine($"연비/효율: {v.GetFuelEfficiency():F1}");
    v.Maintain();
    v.Stop();
}

// 출력:
// === 차량 생성 ===
// Vehicle 기본 구성 완료: 소나타
// Vehicle 기본 구성 완료: 혼다
// Vehicle 기본 구성 완료: Model 3
//
// === 다형성을 통한 처리 ===
//
// --- 소나타 ---
// 모델: 소나타
// 제작연도: 2025
// 타입: Car
// 소나타 자동차 시동: 엔진 가동
// 최고속도: 180 km/h
// 연비/효율: 15.5
// 소나타 정기 점검 수행
// 타이어 공기압 확인
// 엔진 오일 점검
// 소나타 자동차 정지: 엔진 off
// ...
```

**템플릿 메서드 패턴(Template Method Pattern):**

추상 클래스의 가장 강력한 활용은 템플릿 메서드 패턴입니다. 이는 Gang of Four의 23가지 디자인 패턴 중 하나로, 알고리즘의 골격을 정의하면서 세부 단계는 서브클래스에 위임합니다:

```csharp
abstract class DataImporter
{
    // 템플릿 메서드 - 전체 알고리즘의 골격 정의
    public void ImportData(string filePath)
    {
        Console.WriteLine("=== 데이터 가져오기 프로세스 시작 ===\n");
        
        // 1. 파일 열기
        if (!OpenFile(filePath))
        {
            Console.WriteLine("파일 열기 실패");
            return;
        }
        
        // 2. 데이터 검증
        if (!ValidateData())
        {
            Console.WriteLine("데이터 검증 실패");
            CloseFile();
            return;
        }
        
        // 3. 데이터 변환 (파생 클래스가 구현)
        TransformData();
        
        // 4. 데이터 저장 (파생 클래스가 구현)
        SaveToDatabase();
        
        // 5. 정리
        CloseFile();
        
        // 6. 후처리 (선택적 - 훅 메서드)
        PostProcess();
        
        Console.WriteLine("\n=== 데이터 가져오기 완료 ===");
    }
    
    // 구현된 메서드
    private bool OpenFile(string filePath)
    {
        Console.WriteLine($"파일 열기: {filePath}");
        return true;
    }
    
    private void CloseFile()
    {
        Console.WriteLine("파일 닫기");
    }
    
    protected virtual bool ValidateData()
    {
        Console.WriteLine("기본 검증 수행");
        return true;
    }
    
    // 추상 메서드 - 파생 클래스가 반드시 구현
    protected abstract void TransformData();
    protected abstract void SaveToDatabase();
    
    // 훅 메서드 (Hook Method) - 선택적 오버라이드
    protected virtual void PostProcess()
    {
        // 기본적으로 아무것도 하지 않음
    }
}

class CSVImporter : DataImporter
{
    protected override void TransformData()
    {
        Console.WriteLine("CSV 데이터를 파싱하고 변환");
    }
    
    protected override void SaveToDatabase()
    {
        Console.WriteLine("변환된 데이터를 데이터베이스에 저장");
    }
    
    protected override void PostProcess()
    {
        Console.WriteLine("CSV 가져오기 로그 기록");
    }
}

class XMLImporter : DataImporter
{
    protected override bool ValidateData()
    {
        Console.WriteLine("XML 스키마 검증");
        return base.ValidateData();
    }
    
    protected override void TransformData()
    {
        Console.WriteLine("XML을 파싱하고 객체로 변환");
    }
    
    protected override void SaveToDatabase()
    {
        Console.WriteLine("XML 데이터를 데이터베이스에 일괄 저장");
    }
}

// 사용
DataImporter csvImporter = new CSVImporter();
csvImporter.ImportData("data.csv");

Console.WriteLine();

DataImporter xmlImporter = new XMLImporter();
xmlImporter.ImportData("data.xml");
```

**추상 클래스의 실용적 활용 시나리오:**

1. **프레임워크 기반 클래스**: ASP.NET의 `Controller`, `DbContext` 등은 추상 클래스를 통해 확장 포인트를 제공합니다.

2. **공통 로직 제공**: 여러 클래스가 공유하는 로직(로깅, 검증 등)을 추상 클래스에 구현하고, 특화된 부분만 파생 클래스에서 구현합니다.

3. **버전 관리**: 추상 클래스에 새 메서드를 추가하면 모든 파생 클래스가 컴파일 오류로 알림을 받아, 누락 없이 업데이트할 수 있습니다.

**추상 클래스 사용 시 주의사항:**

1. **인스턴스화 불가**: 추상 클래스는 직접 인스턴스를 생성할 수 없지만, 변수 타입으로는 사용할 수 있습니다.

```csharp
Vehicle v1 = new Car("소나타", 5);  // ✅ OK
// Vehicle v2 = new Vehicle("테스트");  // ❌ Error
```

2. **생성자 호출 순서**: 파생 클래스 생성 시 추상 클래스의 생성자가 먼저 실행됩니다.

3. **모든 추상 멤버 구현 필수**: 파생 클래스가 추상 클래스가 아니라면, 모든 추상 멤버를 구현해야 합니다.

```csharp
// ❌ Error: GetFuelEfficiency를 구현하지 않음
class IncompleteCar : Vehicle
{
    public IncompleteCar(string model) : base(model) { }
    public override void Start() { }
    public override void Stop() { }
    // public override double GetFuelEfficiency() { }  // 누락!
    public override int MaxSpeed => 100;
}

// ✅ OK: 추상 클래스로 선언하면 일부만 구현 가능
abstract class PartialCar : Vehicle
{
    protected PartialCar(string model) : base(model) { }
    public override void Start() { }
    public override void Stop() { }
    // GetFuelEfficiency와 MaxSpeed는 하위 클래스가 구현
}
```

---

## 10.3 인터페이스 (Interface)

인터페이스(Interface)는 클래스나 구조체가 구현해야 하는 멤버들의 "계약(Contract)"을 정의하는 참조 타입입니다. 이는 "무엇을(What)" 할 수 있는지를 정의하지만, "어떻게(How)" 구현할지는 각 구현 클래스에 맡기는 강력한 추상화 메커니즘입니다. 인터페이스는 1990년대 Java 언어에서 다중 상속의 문제를 해결하기 위한 대안으로 도입되었으며, 현대 객체지향 프로그래밍의 핵심 도구가 되었습니다.

**인터페이스의 철학적 기반:**

인터페이스는 계약법(Contract Law)의 개념에서 영감을 받았습니다. 계약서는 당사자들이 수행해야 할 의무를 명시하지만, 그 의무를 "어떻게" 이행할지는 각 당사자의 자유입니다. 마찬가지로 인터페이스는 클래스가 제공해야 할 기능을 명시하지만, 구현 방법은 각 클래스가 자유롭게 결정할 수 있습니다.

Martin Fowler는 "Program to an interface, not an implementation"(구현이 아닌 인터페이스에 프로그래밍하라)라는 원칙을 강조했습니다. 이는 의존성 역전 원칙(Dependency Inversion Principle)의 핵심이며, 테스트 가능한 코드(Testable Code)와 느슨한 결합(Loose Coupling)의 기반입니다.

**인터페이스 vs 추상 클래스 선택 가이드:**

| 상황 | 인터페이스 | 추상 클래스 |
|------|-----------|------------|
| 다중 상속 필요 | ✅ 추천 | ❌ 불가능 |
| 공통 구현 제공 | ⚠️ C# 8.0+ 가능 | ✅ 추천 |
| 상태(필드) 필요 | ❌ 불가능 | ✅ 가능 |
| 생성자 필요 | ❌ 불가능 | ✅ 가능 |
| is-a 관계 | ❌ | ✅ |
| can-do 관계 | ✅ | ❌ |
| 버전 관리 | ⚠️ 어려움 | ✅ 쉬움 |
| 플러그인 아키텍처 | ✅ 추천 | △ |

### 10.1 인터페이스 정의와 구현

C#에서 인터페이스는 `interface` 키워드로 선언하며, 관례적으로 이름은 대문자 'I'로 시작합니다. 이는 헝가리안 표기법(Hungarian Notation)의 일종으로, 인터페이스임을 명확히 나타냅니다.

**인터페이스의 구성 요소 (C# 8.0 이전):**

```csharp
// 전통적인 인터페이스 (C# 8.0 이전)
interface IShape
{
    // 메서드 시그니처 (구현 없음, 암묵적으로 public abstract)
    double GetArea();
    double GetPerimeter();
    
    // 속성 (get, set 접근자)
    string Name { get; set; }
    string Color { get; }  // 읽기 전용 속성
    
    // 인덱서
    int this[int index] { get; set; }
    
    // 이벤트
    event EventHandler ShapeChanged;
}
```

**기본적인 인터페이스 구현:**

```csharp
// 그리기 기능을 정의하는 인터페이스
interface IDrawable
{
    void Draw();
    void Erase();
    string GetDescription();
}

// 크기 조절 기능을 정의하는 인터페이스
interface IResizable
{
    void Resize(double scaleFactor);
    double GetScale();
}

// 인터페이스 구현 - 명시적 public 접근자 필요
class Square : IDrawable, IResizable
{
    public double Size { get; set; }
    private double _scale = 1.0;
    
    public Square(double size)
    {
        Size = size;
    }
    
    // IDrawable 구현
    public void Draw()
    {
        Console.WriteLine($"크기 {Size * _scale}인 정사각형을 그립니다.");
        Console.WriteLine("┌─────┐");
        Console.WriteLine("│     │");
        Console.WriteLine("└─────┘");
    }
    
    public void Erase()
    {
        Console.WriteLine("정사각형을 지웁니다.");
    }
    
    public string GetDescription()
    {
        return $"정사각형 (한 변: {Size}, 스케일: {_scale})";
    }
    
    // IResizable 구현
    public void Resize(double scaleFactor)
    {
        _scale *= scaleFactor;
        Console.WriteLine($"크기를 {scaleFactor}배로 조정했습니다. 현재 스케일: {_scale}");
    }
    
    public double GetScale()
    {
        return _scale;
    }
}

// 다른 구현 예제
class Circle : IDrawable, IResizable
{
    public double Radius { get; set; }
    private double _scale = 1.0;
    
    public Circle(double radius)
    {
        Radius = radius;
    }
    
    public void Draw()
    {
        Console.WriteLine($"반지름 {Radius * _scale}인 원을 그립니다.");
        Console.WriteLine("  ●");
    }
    
    public void Erase()
    {
        Console.WriteLine("원을 지웁니다.");
    }
    
    public string GetDescription()
    {
        return $"원 (반지름: {Radius}, 스케일: {_scale})";
    }
    
    public void Resize(double scaleFactor)
    {
        _scale *= scaleFactor;
        Console.WriteLine($"반지름을 {scaleFactor}배로 조정했습니다.");
    }
    
    public double GetScale()
    {
        return _scale;
    }
}

// 사용 예제 - 다형성
List<IDrawable> shapes = new List<IDrawable>
{
    new Square(10),
    new Circle(5),
    new Square(15)
};

Console.WriteLine("=== 모든 도형 그리기 ===");
foreach (IDrawable shape in shapes)
{
    Console.WriteLine(shape.GetDescription());
    shape.Draw();
    Console.WriteLine();
}

// IResizable로도 참조 가능
Console.WriteLine("=== 크기 조절 ===");
foreach (var shape in shapes)
{
    if (shape is IResizable resizable)
    {
        resizable.Resize(1.5);
    }
}
```

**명시적 인터페이스 구현(Explicit Interface Implementation):**

같은 이름의 메서드를 가진 여러 인터페이스를 구현하거나, 인터페이스 멤버를 숨기고 싶을 때 사용합니다:

```csharp
interface IEnglishSpeaker
{
    void Speak();
}

interface IKoreanSpeaker
{
    void Speak();
}

class BilingualPerson : IEnglishSpeaker, IKoreanSpeaker
{
    // 명시적 구현 - 인터페이스 타입을 통해서만 호출 가능
    void IEnglishSpeaker.Speak()
    {
        Console.WriteLine("Hello!");
    }
    
    void IKoreanSpeaker.Speak()
    {
        Console.WriteLine("안녕하세요!");
    }
    
    // 일반 public 메서드
    public void SpeakBoth()
    {
        ((IEnglishSpeaker)this).Speak();
        ((IKoreanSpeaker)this).Speak();
    }
}

// 사용
BilingualPerson person = new BilingualPerson();
// person.Speak();  // ❌ Error: 어떤 Speak인지 모호함

IEnglishSpeaker english = person;
english.Speak();  // 출력: Hello!

IKoreanSpeaker korean = person;
korean.Speak();  // 출력: 안녕하세요!

person.SpeakBoth();  // 둘 다 호출
```

### 10.3.2 다중 인터페이스 구현

C#의 가장 강력한 기능 중 하나는 한 클래스가 여러 인터페이스를 동시에 구현할 수 있다는 것입니다. 이는 C++의 다중 상속 문제를 피하면서도 다중 상속의 이점을 누릴 수 있게 해줍니다.

**역할 기반 설계(Role-Based Design):**

```csharp
// 이동 가능한 객체
interface IMovable
{
    void Move(int deltaX, int deltaY);
    (int X, int Y) GetPosition();
}

// 회전 가능한 객체
interface IRotatable
{
    void Rotate(double degrees);
    double GetRotation();
}

// 크기 조절 가능한 객체
interface IScalable
{
    void Scale(double factor);
    double GetSize();
}

// 충돌 감지 가능한 객체
interface ICollid able
{
    bool CheckCollision(ICollidable other);
    (int X, int Y, int Width, int Height) GetBounds();
}

// 게임 오브젝트 - 여러 역할(role)을 가짐
class GameObject : IMovable, IRotatable, IScalable, ICollidable
{
    private int _x, _y;
    private double _rotation;
    private double _size;
    private int _width, _height;
    
    public string Name { get; set; }
    
    public GameObject(string name, int x, int y, int width, int height)
    {
        Name = name;
        _x = x;
        _y = y;
        _width = width;
        _height = height;
        _size = 1.0;
        _rotation = 0;
    }
    
    // IMovable 구현
    public void Move(int deltaX, int deltaY)
    {
        _x += deltaX;
        _y += deltaY;
        Console.WriteLine($"{Name} 이동: ({_x}, {_y})");
    }
    
    public (int X, int Y) GetPosition() => (_x, _y);
    
    // IRotatable 구현
    public void Rotate(double degrees)
    {
        _rotation = (_rotation + degrees) % 360;
        Console.WriteLine($"{Name} 회전: {_rotation}도");
    }
    
    public double GetRotation() => _rotation;
    
    // IScalable 구현
    public void Scale(double factor)
    {
        _size *= factor;
        Console.WriteLine($"{Name} 크기: {_size}배");
    }
    
    public double GetSize() => _size;
    
    // ICollidable 구현
    public bool CheckCollision(ICollidable other)
    {
        var (x1, y1, w1, h1) = GetBounds();
        var (x2, y2, w2, h2) = other.GetBounds();
        
        return x1 < x2 + w2 && x1 + w1 > x2 &&
               y1 < y2 + h2 && y1 + h1 > y2;
    }
    
    public (int X, int Y, int Width, int Height) GetBounds()
    {
        int scaledWidth = (int)(_width * _size);
        int scaledHeight = (int)(_height * _size);
        return (_x, _y, scaledWidth, scaledHeight);
    }
}

// 플레이어는 모든 기능이 필요
class Player : GameObject
{
    public int Health { get; set; }
    
    public Player(string name, int x, int y) 
        : base(name, x, y, 32, 32)
    {
        Health = 100;
    }
}

// 벽은 이동이나 회전이 불필요 - 선택적 구현
class Wall : ICollidable
{
    private int _x, _y, _width, _height;
    
    public Wall(int x, int y, int width, int height)
    {
        _x = x;
        _y = y;
        _width = width;
        _height = height;
    }
    
    public bool CheckCollision(ICollidable other)
    {
        var (x1, y1, w1, h1) = GetBounds();
        var (x2, y2, w2, h2) = other.GetBounds();
        
        return x1 < x2 + w2 && x1 + w1 > x2 &&
               y1 < y2 + h2 && y1 + h1 > y2;
    }
    
    public (int X, int Y, int Width, int Height) GetBounds()
    {
        return (_x, _y, _width, _height);
    }
}

// 사용 예제
Player player = new Player("Hero", 0, 0);
Wall wall = new Wall(100, 0, 50, 100);

// 인터페이스별로 다르게 처리
IMovable movable = player;
movable.Move(50, 25);

IRotatable rotatable = player;
rotatable.Rotate(45);

// 충돌 체크
if (player.CheckCollision(wall))
{
    Console.WriteLine("벽과 충돌!");
}
else
{
    Console.WriteLine("안전");
}
```

**인터페이스 상속:**

인터페이스도 다른 인터페이스를 상속할 수 있습니다:

```csharp
interface IEntity
{
    string GetName();
    void Update(double deltaTime);
}

interface IRenderable
{
    void Render();
}

// 인터페이스 상속 - IGameObject는 IEntity와 IRenderable의 모든 멤버 포함
interface IGameObject : IEntity, IRenderable
{
    void Initialize();
    void Destroy();
}

class Sprite : IGameObject
{
    public string Name { get; set; }
    
    // IEntity 구현
    public string GetName() => Name;
    
    public void Update(double deltaTime)
    {
        Console.WriteLine($"{Name} 업데이트: {deltaTime}ms");
    }
    
    // IRenderable 구현
    public void Render()
    {
        Console.WriteLine($"{Name} 렌더링");
    }
    
    // IGameObject 구현
    public void Initialize()
    {
        Console.WriteLine($"{Name} 초기화");
    }
    
    public void Destroy()
    {
        Console.WriteLine($"{Name} 파괴");
    }
}
```

### 10.3.3 기본 인터페이스 메서드 (Default Interface Methods)

C# 8.0(.NET Core 3.0, .NET Standard 2.1)부터 인터페이스에 기본 구현을 제공할 수 있게 되었습니다. 이는 Java 8의 default 메서드에서 영감을 받은 기능으로, 인터페이스의 버전 관리 문제를 해결하는 혁신적인 방법입니다.

**버전 관리 문제와 해결책:**

전통적으로 인터페이스에 새 메서드를 추가하면, 해당 인터페이스를 구현한 모든 클래스가 깨집니다(Breaking Change). 기본 인터페이스 메서드는 이 문제를 해결합니다:

```csharp
// C# 8.0+ 기본 인터페이스 메서드
interface ILogger
{
    // 추상 메서드 - 반드시 구현 필요
    void Log(string message);
    
    // 기본 구현이 있는 메서드 - 구현 선택적
    void LogError(string error)
    {
        Log($"[ERROR] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - {error}");
    }
    
    void LogWarning(string warning)
    {
        Log($"[WARNING] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - {warning}");
    }
    
    void LogInfo(string info)
    {
        Log($"[INFO] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - {info}");
    }
    
    void LogDebug(string debug)
    {
        #if DEBUG
        Log($"[DEBUG] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - {debug}");
        #endif
    }
}

// 최소 구현 - Log 메서드만 구현하면 됨
class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
    
    // LogError, LogWarning 등은 자동으로 기본 구현 사용
}

// 선택적 오버라이드 - 일부만 커스터마이즈
class ColoredConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
    
    // LogError만 커스터마이즈
    public void LogError(string error)
    {
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine($"!!! 오류: {error} !!!");
        Console.ResetColor();
    }
    
    // LogWarning, LogInfo, LogDebug는 기본 구현 사용
}

// 완전 커스터마이즈
class FileLogger : ILogger
{
    private string _filePath;
    
    public FileLogger(string filePath)
    {
        _filePath = filePath;
    }
    
    public void Log(string message)
    {
        File.AppendAllText(_filePath, message + Environment.NewLine);
    }
    
    // 모든 메서드를 파일 기반으로 재구현
    public void LogError(string error)
    {
        Log($"[CRITICAL ERROR] {error}");
        // 추가로 이메일 알림 등...
    }
}

// 사용 예제
ILogger logger1 = new ConsoleLogger();
logger1.Log("일반 메시지");
logger1.LogError("오류 발생");
logger1.LogWarning("주의사항");
logger1.LogInfo("정보");

Console.WriteLine();

ILogger logger2 = new ColoredConsoleLogger();
logger2.LogError("이 오류는 빨간색으로 표시됩니다");
logger2.LogWarning("이 경고는 기본 형식입니다");
```

**기본 인터페이스 메서드의 고급 기능:**

C# 8.0+에서는 인터페이스에 더 많은 기능이 추가되었습니다:

```csharp
interface IAdvancedLogger
{
    // 정적 필드 (C# 8.0+)
    static int LogCount = 0;
    
    // 정적 메서드
    static void ResetCount()
    {
        LogCount = 0;
    }
    
    // 추상 메서드
    void Log(string message);
    
    // 기본 구현 메서드
    void LogWithCount(string message)
    {
        LogCount++;
        Log($"[{LogCount}] {message}");
    }
    
    // 보호 멤버 (C# 8.0+)
    protected void InternalLog(string message)
    {
        Console.WriteLine($"[Internal] {message}");
    }
    
    // 프로퍼티 (C# 8.0+)
    string LogPrefix => "[LOG]";
    
    void LogWithPrefix(string message)
    {
        Log($"{LogPrefix} {message}");
    }
}
```

**주의사항:**

1. **접근 방법**: 기본 구현 메서드는 인터페이스 타입을 통해서만 호출할 수 있습니다:

```csharp
ConsoleLogger logger = new ConsoleLogger();
// logger.LogError("test");  // ❌ Error: ConsoleLogger에 LogError 메서드 없음

ILogger ilogger = logger;
ilogger.LogError("test");  // ✅ OK: 인터페이스를 통해 접근
```

2. **다이아몬드 문제**: 여러 인터페이스에 같은 기본 메서드가 있으면 명시적으로 선택해야 합니다.

3. **.NET Framework 호환성**: 기본 인터페이스 메서드는 .NET Core 3.0+ 또는 .NET 5+에서만 지원됩니다.

---

## 10.4 다형성 (Polymorphism)

다형성(Polymorphism)은 그리스어로 "여러(poly) 형태(morph)"를 의미하며, 객체지향 프로그래밍의 가장 강력하고 핵심적인 개념입니다. 다형성은 하나의 인터페이스를 통해 서로 다른 타입의 객체를 일관되게 다룰 수 있게 하는 능력으로, 코드의 유연성, 확장성, 재사용성을 극대화합니다.

**다형성의 유형:**

1. **Ad-hoc 다형성 (Ad-hoc Polymorphism)**:
   - **메서드 오버로딩(Method Overloading)**: 같은 이름의 다른 시그니처 메서드
   - **연산자 오버로딩(Operator Overloading)**: `+`, `-` 등의 연산자를 사용자 정의 타입에 재정의
   - 컴파일 타임에 결정 (정적 바인딩)

2. **파라메트릭 다형성 (Parametric Polymorphism)**:
   - **제네릭(Generics)**: `List<T>`, `Dictionary<TKey, TValue>` 등
   - 타입을 매개변수화하여 재사용성 극대화

3. **서브타입 다형성 (Subtype Polymorphism)**: ⭐ 이 장의 주제
   - **메서드 오버라이딩(Method Overriding)**: 상속과 가상 메서드
   - **인터페이스 구현**: 인터페이스 기반 다형성
   - 런타임에 결정 (동적 바인딩)

**서브타입 다형성의 실제 응용:**

```csharp
// 추상 기본 클래스
abstract class Employee
{
    public string Name { get; set; }
    public string EmployeeId { get; set; }
    public DateTime HireDate { get; set; }
    
    protected Employee(string name, string employeeId)
    {
        Name = name;
        EmployeeId = employeeId;
        HireDate = DateTime.Now;
    }
    
    // 추상 메서드 - 각 직원 유형마다 다르게 계산
    public abstract double CalculateSalary();
    public abstract double CalculateBonus();
    
    // 구현된 메서드 - 모든 직원에게 공통
    public void DisplayInfo()
    {
        Console.WriteLine($"=== {Name} ({EmployeeId}) ===");
        Console.WriteLine($"고용일: {HireDate:yyyy-MM-dd}");
        Console.WriteLine($"근무기간: {(DateTime.Now - HireDate).Days}일");
        Console.WriteLine($"급여: {CalculateSalary():C}");
        Console.WriteLine($"보너스: {CalculateBonus():C}");
        Console.WriteLine($"총 수령액: {(CalculateSalary() + CalculateBonus()):C}");
    }
    
    // 가상 메서드 - 기본 구현 제공, 선택적 오버라이드
    public virtual string GetEmployeeType()
    {
        return "일반 직원";
    }
}

class FullTimeEmployee : Employee
{
    public double MonthlySalary { get; set; }
    public double YearsOfService { get; set; }
    
    public FullTimeEmployee(string name, string id, double salary, double years)
        : base(name, id)
    {
        MonthlySalary = salary;
        YearsOfService = years;
    }
    
    public override double CalculateSalary()
    {
        return MonthlySalary;
    }
    
    public override double CalculateBonus()
    {
        // 근속년수에 따른 보너스 (근속 1년당 월급의 5%)
        return MonthlySalary * YearsOfService * 0.05;
    }
    
    public override string GetEmployeeType()
    {
        return "정규직";
    }
}

class PartTimeEmployee : Employee
{
    public double HourlyRate { get; set; }
    public int HoursWorked { get; set; }
    
    public PartTimeEmployee(string name, string id, double rate, int hours)
        : base(name, id)
    {
        HourlyRate = rate;
        HoursWorked = hours;
    }
    
    public override double CalculateSalary()
    {
        double basePay = HourlyRate * HoursWorked;
        // 주 40시간 초과 시 1.5배 수당
        if (HoursWorked > 40)
        {
            double overtimeHours = HoursWorked - 40;
            basePay += (overtimeHours * HourlyRate * 0.5);
        }
        return basePay;
    }
    
    public override double CalculateBonus()
    {
        // 시간제는 보너스 없음
        return 0;
    }
    
    public override string GetEmployeeType()
    {
        return "시간제";
    }
}

class Contractor : Employee
{
    public double ProjectFee { get; set; }
    public double CompletionPercentage { get; set; }
    
    public Contractor(string name, string id, double fee, double completion)
        : base(name, id)
    {
        ProjectFee = fee;
        CompletionPercentage = completion;
    }
    
    public override double CalculateSalary()
    {
        return ProjectFee * (CompletionPercentage / 100.0);
    }
    
    public override double CalculateBonus()
    {
        // 프로젝트 100% 완료 시에만 보너스
        return CompletionPercentage >= 100 ? ProjectFee * 0.1 : 0;
    }
    
    public override string GetEmployeeType()
    {
        return $"계약직 ({CompletionPercentage:F1}% 완료)";
    }
}

// 다형성을 활용한 급여 시스템
class PayrollSystem
{
    private List<Employee> employees = new List<Employee>();
    
    public void AddEmployee(Employee employee)
    {
        employees.Add(employee);
        Console.WriteLine($"{employee.Name} ({employee.GetEmployeeType()}) 등록됨");
    }
    
    public void ProcessPayroll()
    {
        Console.WriteLine("\n========================================");
        Console.WriteLine("       급여 처리 시스템");
        Console.WriteLine("========================================\n");
        
        double totalSalary = 0;
        double totalBonus = 0;
        
        foreach (Employee emp in employees)
        {
            emp.DisplayInfo();
            Console.WriteLine();
            
            totalSalary += emp.CalculateSalary();
            totalBonus += emp.CalculateBonus();
        }
        
        Console.WriteLine("========================================");
        Console.WriteLine($"총 급여: {totalSalary:C}");
        Console.WriteLine($"총 보너스: {totalBonus:C}");
        Console.WriteLine($"전체 합계: {(totalSalary + totalBonus):C}");
        Console.WriteLine($"직원 수: {employees.Count}명");
        Console.WriteLine("========================================");
    }
    
    public void ShowEmployeeTypes()
    {
        Console.WriteLine("\n=== 직원 유형별 통계 ===");
        var typeGroups = employees.GroupBy(e => e.GetEmployeeType());
        foreach (var group in typeGroups)
        {
            Console.WriteLine($"{group.Key}: {group.Count()}명");
        }
    }
}

// 사용 예제
PayrollSystem payroll = new PayrollSystem();

payroll.AddEmployee(new FullTimeEmployee("김철수", "E001", 4500000, 5));
payroll.AddEmployee(new FullTimeEmployee("이영희", "E002", 5200000, 8));
payroll.AddEmployee(new PartTimeEmployee("박민수", "P001", 15000, 45));
payroll.AddEmployee(new PartTimeEmployee("정수진", "P002", 12000, 35));
payroll.AddEmployee(new Contractor("최대현", "C001", 10000000, 75));
payroll.AddEmployee(new Contractor("강미래", "C002", 8000000, 100));

payroll.ShowEmployeeTypes();
payroll.ProcessPayroll();
```

**인터페이스 기반 다형성:**

상속보다 더 유연한 다형성을 제공합니다:

```csharp
// 지불 가능한 엔티티의 계약
interface IPayable
{
    double GetPaymentAmount();
    string GetPaymentDescription();
    DateTime GetPaymentDue();
}

// 송장 클래스
class Invoice : IPayable
{
    public string InvoiceNumber { get; set; }
    public string PartNumber { get; set; }
    public string PartDescription { get; set; }
    public int Quantity { get; set; }
    public double PricePerItem { get; set; }
    public DateTime DueDate { get; set; }
    
    public double GetPaymentAmount()
    {
        return Quantity * PricePerItem;
    }
    
    public string GetPaymentDescription()
    {
        return $"송장 #{InvoiceNumber}: {PartDescription} x {Quantity}";
    }
    
    public DateTime GetPaymentDue()
    {
        return DueDate;
    }
}

// 급여 직원 클래스
class SalariedEmployee : IPayable
{
    public string Name { get; set; }
    public string EmployeeId { get; set; }
    public double WeeklySalary { get; set; }
    public DateTime PayDay { get; set; }
    
    public double GetPaymentAmount()
    {
        return WeeklySalary;
    }
    
    public string GetPaymentDescription()
    {
        return $"급여: {Name} (ID: {EmployeeId})";
    }
    
    public DateTime GetPaymentDue()
    {
        return PayDay;
    }
}

// 세금 납부 클래스
class TaxPayment : IPayable
{
    public string TaxType { get; set; }
    public double Amount { get; set; }
    public DateTime DueDate { get; set; }
    
    public double GetPaymentAmount()
    {
        return Amount;
    }
    
    public string GetPaymentDescription()
    {
        return $"세금 납부: {TaxType}";
    }
    
    public DateTime GetPaymentDue()
    {
        return DueDate;
    }
}

// 지불 처리 시스템 - 완전히 다른 타입을 동일하게 처리
class PaymentProcessor
{
    public void ProcessPayments(List<IPayable> payables)
    {
        Console.WriteLine("========================================");
        Console.WriteLine("         지불 처리 시스템");
        Console.WriteLine("========================================\n");
        
        // 마감일순 정렬
        var sorted = payables.OrderBy(p => p.GetPaymentDue());
        
        double totalAmount = 0;
        int itemCount = 0;
        
        foreach (IPayable payable in sorted)
        {
            itemCount++;
            double amount = payable.GetPaymentAmount();
            totalAmount += amount;
            
            Console.WriteLine($"[{itemCount}] {payable.GetPaymentDescription()}");
            Console.WriteLine($"    금액: {amount:C}");
            Console.WriteLine($"    마감: {payable.GetPaymentDue():yyyy-MM-dd}");
            
            // 마감일 임박 경고
            TimeSpan timeUntilDue = payable.GetPaymentDue() - DateTime.Now;
            if (timeUntilDue.TotalDays < 7 && timeUntilDue.TotalDays > 0)
            {
                Console.WriteLine($"    ⚠️  {timeUntilDue.Days}일 후 마감!");
            }
            else if (timeUntilDue.TotalDays < 0)
            {
                Console.WriteLine($"    ❌ {Math.Abs(timeUntilDue.Days)}일 연체!");
            }
            
            Console.WriteLine();
        }
        
        Console.WriteLine("========================================");
        Console.WriteLine($"총 지불 항목: {itemCount}개");
        Console.WriteLine($"총 지불 금액: {totalAmount:C}");
        Console.WriteLine("========================================");
    }
}

// 사용
var payables = new List<IPayable>
{
    new Invoice 
    { 
        InvoiceNumber = "INV-001",
        PartNumber = "A101", 
        PartDescription = "고급 부품", 
        Quantity = 10, 
        PricePerItem = 50000,
        DueDate = DateTime.Now.AddDays(5)
    },
    new SalariedEmployee 
    { 
        Name = "최수정", 
        EmployeeId = "E100",
        WeeklySalary = 800000,
        PayDay = DateTime.Now.AddDays(2)
    },
    new TaxPayment
    {
        TaxType = "법인세",
        Amount = 5000000,
        DueDate = DateTime.Now.AddDays(15)
    },
    new Invoice
    {
        InvoiceNumber = "INV-002",
        PartNumber = "B202",
        PartDescription = "일반 부품",
        Quantity = 50,
        PricePerItem = 10000,
        DueDate = DateTime.Now.AddDays(-3)  // 연체
    }
};

PaymentProcessor processor = new PaymentProcessor();
processor.ProcessPayments(payables);
```

**다형성의 실무 패턴:**

```csharp
// 전략 패턴 (Strategy Pattern)
interface ICompressionStrategy
{
    byte[] Compress(byte[] data);
    byte[] Decompress(byte[] data);
    string GetAlgorithmName();
}

class ZipCompression : ICompressionStrategy
{
    public byte[] Compress(byte[] data)
    {
        Console.WriteLine("ZIP 압축 수행");
        // 실제 ZIP 압축 로직
        return data;
    }
    
    public byte[] Decompress(byte[] data)
    {
        Console.WriteLine("ZIP 압축 해제");
        return data;
    }
    
    public string GetAlgorithmName() => "ZIP";
}

class GzipCompression : ICompressionStrategy
{
    public byte[] Compress(byte[] data)
    {
        Console.WriteLine("GZIP 압축 수행");
        return data;
    }
    
    public byte[] Decompress(byte[] data)
    {
        Console.WriteLine("GZIP 압축 해제");
        return data;
    }
    
    public string GetAlgorithmName() => "GZIP";
}

class FileCompressor
{
    private ICompressionStrategy _strategy;
    
    public void SetStrategy(ICompressionStrategy strategy)
    {
        _strategy = strategy;
        Console.WriteLine($"압축 알고리즘을 {strategy.GetAlgorithmName()}로 변경");
    }
    
    public void CompressFile(string filePath)
    {
        Console.WriteLine($"파일 압축: {filePath}");
        // byte[] data = File.ReadAllBytes(filePath);
        byte[] data = new byte[100];  // 시뮬레이션
        byte[] compressed = _strategy.Compress(data);
        Console.WriteLine("압축 완료\n");
    }
}

// 사용 - 런타임에 알고리즘 변경
FileCompressor compressor = new FileCompressor();

compressor.SetStrategy(new ZipCompression());
compressor.CompressFile("document.txt");

compressor.SetStrategy(new GzipCompression());
compressor.CompressFile("data.bin");
```

**다형성의 장점과 SOLID 원칙:**

1. **개방-폐쇄 원칙(OCP)**: 새로운 타입 추가 시 기존 코드 수정 불필요
2. **의존성 역전 원칙(DIP)**: 구체적인 구현이 아닌 추상에 의존
3. **리스코프 치환 원칙(LSP)**: 파생 타입으로 기본 타입을 대체 가능
4. **인터페이스 분리 원칙(ISP)**: 인터페이스를 작고 특화되게 유지

---

## 마무리

이 장에서는 객체지향 프로그래밍의 가장 핵심적인 두 가지 개념인 상속과 다형성을 깊이 있게 학습했습니다. 이러한 개념들은 단순한 프로그래밍 기법을 넘어, 소프트웨어 설계의 철학과 원칙을 구현하는 강력한 도구입니다.

**핵심 학습 내용:**

✅ **상속 (Inheritance)**: 
- is-a 관계를 통한 코드 재사용과 계층 구조 표현
- base 키워드를 통한 생성자 체이닝과 기본 클래스 멤버 접근
- virtual/override를 통한 메서드 오버라이딩과 동적 바인딩
- 가상 메서드 테이블(VMT)의 동작 원리와 성능 특성

✅ **추상 클래스 (Abstract Class)**:
- 불완전한 클래스를 통한 계약 정의와 공통 구현 제공
- 템플릿 메서드 패턴(Template Method Pattern)의 구현
- 추상 클래스 vs 인터페이스의 선택 기준과 트레이드오프

✅ **인터페이스 (Interface)**:
- 계약 지향 프로그래밍(Contract-based Programming)의 핵심
- 다중 인터페이스 구현을 통한 유연한 설계
- C# 8.0+의 기본 인터페이스 메서드와 버전 관리 전략

✅ **다형성 (Polymorphism)**:
- 런타임 다형성을 통한 확장 가능한 시스템 구축
- 인터페이스 기반 설계와 의존성 주입(Dependency Injection)
- 전략 패턴(Strategy Pattern) 등 다형성 기반 디자인 패턴

**SOLID 원칙과의 연결:**

이 장에서 학습한 개념들은 Robert C. Martin의 SOLID 원칙과 직접적으로 연결됩니다:

1. **단일 책임 원칙(SRP)**: 상속을 통해 관심사를 분리
2. **개방-폐쇄 원칙(OCP)**: 상속과 다형성으로 확장에 개방, 수정에 폐쇄
3. **리스코프 치환 원칙(LSP)**: 올바른 상속 계층 구조 설계
4. **인터페이스 분리 원칙(ISP)**: 작고 특화된 인터페이스 설계
5. **의존성 역전 원칙(DIP)**: 인터페이스를 통한 느슨한 결합

**실무 적용 가이드:**

1. **상속 사용 시기**:
   - 명확한 is-a 관계가 있을 때
   - 공통 동작과 상태를 공유해야 할 때
   - 다형성이 필요할 때

2. **인터페이스 사용 시기**:
   - can-do 관계를 표현할 때
   - 다중 상속이 필요할 때
   - 느슨한 결합이 중요할 때
   - 테스트 가능성이 중요할 때

3. **조합 vs 상속**:
   - Gang of Four: "상속보다 조합을 선호하라(Favor Composition Over Inheritance)"
   - 유연성이 필요한 경우 인터페이스와 조합을 우선 고려
   - 상속은 신중하게, 깊은 상속 계층(>3 레벨)은 피하기

**디자인 패턴 프리뷰:**

이 장에서 배운 개념들은 다음과 같은 디자인 패턴의 기초가 됩니다:

- **템플릿 메서드 패턴**: 추상 클래스로 알고리즘 골격 정의
- **전략 패턴**: 인터페이스로 교환 가능한 알고리즘 구현
- **팩토리 메서드 패턴**: 상속을 통한 객체 생성 위임
- **어댑터 패턴**: 인터페이스를 통한 호환성 제공
- **데코레이터 패턴**: 상속 대신 조합으로 기능 확장

**다음 단계:**

11장에서는 sealed 클래스, record 타입, 구조체(struct), 열거형(enum) 등 C#의 고급 객체지향 개념을 다룹니다. 이들은 상속과 다형성의 개념을 보완하고, 더욱 정교한 타입 시스템 설계를 가능하게 합니다.

**실습 과제:**

1. **도형 계층 구조 구현**: Shape 추상 클래스를 기반으로 Circle, Rectangle, Triangle 클래스를 구현하고, 넓이/둘레 계산 메서드를 오버라이드하세요.

2. **플러그인 시스템 설계**: IPlugin 인터페이스를 정의하고, 여러 플러그인을 로드하여 실행하는 시스템을 구현하세요.

3. **정렬 전략 구현**: IComparer<T> 인터페이스를 구현하여 다양한 정렬 알고리즘(버블, 퀵, 병합)을 전략 패턴으로 구현하세요.

4. **직원 관리 시스템**: 이 장의 예제를 확장하여, 승진, 이직, 퇴사 등의 기능을 추가한 완전한 HR 시스템을 구현하세요.

**권장 학습 자료:**

- "Design Patterns" by Gang of Four - 디자인 패턴의 바이블
- "Clean Code" by Robert C. Martin - 객체지향 설계 원칙
- "Head First Design Patterns" - 디자인 패턴 입문서
- Microsoft Docs: C# Inheritance - 공식 문서
- "Refactoring" by Martin Fowler - 리팩토링 기법

상속과 다형성은 객체지향 프로그래밍의 핵심이지만, 과용하면 오히려 복잡도를 증가시킬 수 있습니다. 항상 문제의 본질을 이해하고, 가장 단순하면서도 효과적인 해결책을 선택하는 것이 중요합니다. "단순함은 궁극의 정교함이다(Simplicity is the ultimate sophistication)" - 레오나르도 다 빈치의 말처럼, 복잡함보다는 명확함을, 영리함보다는 이해 가능함을 추구하시기 바랍니다.
