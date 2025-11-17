# 20장. 할 일 관리 앱

할 일 관리 애플리케이션은 **정보 관리 시스템(Information Management System)**의 본질을 가장 단순하고 명확하게 보여주는 고전적 프로젝트입니다. 1960년대 초 Ivan Sutherland의 Sketchpad부터 시작하여, 1980년대 David Allen의 GTD(Getting Things Done) 방법론, 그리고 현대의 Todoist, Trello, Asana에 이르기까지, 할 일 관리는 **생산성 소프트웨어(Productivity Software)**의 핵심 패러다임으로 자리잡았습니다.

**할 일 관리 앱의 컴퓨터 과학적 의미:**

이 프로젝트는 단순한 목록 관리를 넘어서, 현대 소프트웨어 개발의 핵심 개념들을 통합합니다:

1. **데이터베이스 이론(Database Theory)**: E.F. Codd의 관계형 모델(Relational Model)과 ACID 속성(Atomicity, Consistency, Isolation, Durability)의 기초를 학습합니다. 우리의 할 일 목록은 간단한 인메모리 데이터베이스이며, CRUD 작업은 SQL의 INSERT, SELECT, UPDATE, DELETE와 정확히 대응됩니다.

2. **객체지향 설계(Object-Oriented Design)**: Grady Booch의 객체지향 분석 및 설계(OOAD) 원칙을 적용하여, 현실 세계의 개념(할 일)을 소프트웨어 객체로 매핑합니다. 이는 **도메인 주도 설계(Domain-Driven Design, DDD)**의 실천이며, Eric Evans가 제시한 유비쿼터스 언어(Ubiquitous Language)를 코드로 구현합니다.

3. **데이터 영속성(Data Persistence)**: Martin Fowler의 "Patterns of Enterprise Application Architecture"에서 제시한 **저장소 패턴(Repository Pattern)**과 **데이터 매퍼(Data Mapper)**를 학습합니다. JSON 직렬화는 객체-관계 임피던스 불일치(Object-Relational Impedance Mismatch) 문제의 단순화된 해결책입니다.

4. **쿼리 언어와 LINQ**: Edgar F. Codd의 관계 대수(Relational Algebra)에서 시작된 쿼리 이론을 C#의 LINQ(Language Integrated Query)로 경험합니다. LINQ는 Haskell의 리스트 컴프리헨션(List Comprehension)과 SQL의 선언적 쿼리를 C#에 통합한 혁신적 기능입니다.

5. **상태 관리와 불변성(State Management and Immutability)**: 할 일의 생명주기(생성 → 진행 중 → 완료)는 **유한 상태 기계(Finite State Machine)**로 모델링할 수 있습니다. 이는 React의 상태 관리, Redux의 불변 상태, 그리고 함수형 프로그래밍의 핵심 개념과 연결됩니다.

**소프트웨어 아키텍처 패턴의 실습:**

이 프로젝트를 통해 다음의 아키텍처 패턴을 경험합니다:

- **계층 아키텍처(Layered Architecture)**: 프레젠테이션(UI), 비즈니스 로직, 데이터 접근 계층의 분리
- **MVC 패턴의 변형**: 모델(TodoItem), 뷰(Console), 컨트롤러(메뉴 시스템)
- **저장소 패턴(Repository Pattern)**: 데이터 접근의 추상화
- **서비스 레이어(Service Layer)**: 비즈니스 로직의 캡슐화

**생산성 시스템의 심리학과 인지 과학:**

David Allen의 GTD 방법론은 인지 심리학의 연구 결과를 바탕으로 합니다:

- **외부 기억(External Memory)**: Daniel Schacter의 기억 연구에 따르면, 인간의 작업 기억(Working Memory)은 제한적입니다. 할 일을 외부 시스템에 저장하면 인지 부하(Cognitive Load)가 감소하여 창의적 사고에 집중할 수 있습니다.

- **자이가르닉 효과(Zeigarnik Effect)**: 미완성 과제가 완료된 과제보다 더 잘 기억된다는 심리학 원리를 활용하여, 미완료 항목을 명시적으로 추적합니다.

- **우선순위 매트릭스(Priority Matrix)**: Eisenhower의 긴급-중요 매트릭스를 디지털화하여, 시간 관리(Time Management)의 체계적 접근을 가능하게 합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 실무 애플리케이션 개발의 핵심 역량을 습득하게 됩니다:

- **도메인 모델링(Domain Modeling)**: Eric Evans의 DDD 원칙에 따라 현실 세계의 개념을 코드로 변환하는 방법을 학습합니다. 엔티티(Entity), 값 객체(Value Object), 집합 루트(Aggregate Root)의 개념을 이해하고, 유비쿼터스 언어를 통해 도메인 전문가와 개발자 간의 의사소통을 촉진합니다.

- **CRUD 패턴의 완전한 구현**: Roy Fielding의 REST(Representational State Transfer) 아키텍처의 기초가 되는 CRUD 작업을 체계적으로 구현합니다. 각 작업의 전제조건(Precondition)과 후조건(Postcondition), 그리고 불변조건(Invariant) 유지 방법을 배웁니다.

- **직렬화와 역직렬화(Serialization and Deserialization)**: 객체를 바이트 스트림으로 변환하는 직렬화의 이론적 기반을 이해합니다. JSON, XML, Protocol Buffers 등 다양한 직렬화 형식의 장단점을 비교하고, System.Text.Json의 성능 최적화 기법을 학습합니다.

- **선언적 프로그래밍과 LINQ**: 명령형(Imperative) 프로그래밍에서 선언적(Declarative) 프로그래밍으로의 패러다임 전환을 경험합니다. SQL의 관계 대수, Haskell의 모나드(Monad), C#의 LINQ가 어떻게 연결되는지 이해하며, 지연 실행(Deferred Execution)과 즉시 실행(Immediate Execution)의 차이를 파악합니다.

- **시간 기반 데이터 관리**: 마감일, 우선순위, 상태 전이를 통해 **시간적 데이터베이스(Temporal Database)**의 기초 개념을 학습합니다. 유효 시간(Valid Time)과 트랜잭션 시간(Transaction Time)의 구분, 그리고 SCD(Slowly Changing Dimension) 패턴을 이해합니다.

**프로젝트 기반 학습의 교육적 가치:**

Seymour Papert의 구성주의(Constructionism)와 Jerome Bruner의 발견 학습(Discovery Learning) 이론에 따라, 이 프로젝트는:

1. **구체적 경험(Concrete Experience)**: 실제 사용 가능한 애플리케이션 구축
2. **반성적 관찰(Reflective Observation)**: 코드의 동작 원리 이해
3. **추상적 개념화(Abstract Conceptualization)**: 디자인 패턴과 원칙 추출
4. **능동적 실험(Active Experimentation)**: 확장 기능 구현

이러한 Kolb의 경험 학습 사이클(Experiential Learning Cycle)을 통해 깊은 이해와 장기 기억을 촉진합니다.

---

## 20.1 데이터 모델 설계

데이터 모델 설계는 소프트웨어 개발에서 가장 중요한 결정 중 하나입니다. Peter Chen의 개체-관계 모델(Entity-Relationship Model, 1976)과 James Martin의 정보 공학(Information Engineering) 방법론은 데이터 모델링의 이론적 기반을 제공합니다.

**데이터 모델링의 철학적 기반:**

Plato의 이데아론(Theory of Forms)에서처럼, 우리는 현실 세계의 "할 일"이라는 추상적 개념을 소프트웨어 객체로 구체화합니다. 이는 **존재론(Ontology)**의 문제이며, 다음 질문에 답해야 합니다:

- **정체성(Identity)**: 무엇이 할 일을 고유하게 만드는가? → `Id` 속성
- **속성(Attributes)**: 할 일의 본질적 특성은 무엇인가? → `Title`, `Description`
- **상태(State)**: 할 일이 가질 수 있는 상태는? → `IsCompleted`
- **시간성(Temporality)**: 시간과 어떻게 관련되는가? → `CreatedAt`, `DueDate`
- **우선순위(Priority)**: 상대적 중요도를 어떻게 표현하는가? → `Priority`

**도메인 주도 설계(DDD)의 적용:**

Eric Evans의 DDD 패턴을 우리의 할 일 관리 시스템에 적용합니다:

1. **엔티티(Entity)**: `TodoItem`은 고유 식별자(`Id`)를 가지며, 속성 값이 변해도 동일성(Identity)을 유지합니다. 이는 값 객체(Value Object)와 대비됩니다.

2. **유비쿼터스 언어(Ubiquitous Language)**: 도메인 전문가(사용자)와 개발자가 공유하는 용어를 코드에 직접 반영합니다. `Title`, `DueDate`, `Priority` 등은 일상 언어를 그대로 사용합니다.

3. **집합 루트(Aggregate Root)**: `TodoItem`은 자신의 생명주기를 관리하는 집합의 루트입니다. 외부에서는 이 루트를 통해서만 내부 상태에 접근할 수 있어야 합니다(캡슐화).

4. **경계 컨텍스트(Bounded Context)**: 할 일 관리 도메인의 경계를 명확히 하여, 다른 도메인(예: 사용자 관리, 알림 시스템)과의 관계를 정의합니다.

**관계형 모델과 객체 모델의 임피던스 불일치:**

Ted Codd의 관계형 모델은 수학적 집합 이론에 기반하지만, 객체지향 모델은 클래스와 상속에 기반합니다. 이 두 패러다임의 차이를 **객체-관계 임피던스 불일치(Object-Relational Impedance Mismatch)**라고 하며, ORM(Object-Relational Mapping) 도구가 이를 해결합니다. 우리의 경우 JSON 직렬화가 단순화된 매핑 메커니즘입니다.

### TodoItem 클래스 정의

**완전한 데이터 모델:**

```csharp
class TodoItem
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Description { get; set; }
    public bool IsCompleted { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime? DueDate { get; set; }
    public int Priority { get; set; } // 1: 낮음, 2: 보통, 3: 높음
}
```

**속성 설계의 이론적 근거:**

**1. Id (식별자, Primary Key):**
- **이론**: E.F. Codd의 관계형 모델에서 각 튜플(행)은 고유하게 식별되어야 합니다. `Id`는 **대리 키(Surrogate Key)**로, 비즈니스 로직과 무관한 시스템 생성 값입니다.
- **대안 고려**: UUID(Guid)는 분산 시스템에서 충돌 없이 고유성을 보장하지만, 순차적 정수는 인간이 읽기 쉽고 메모리 효율적입니다.
- **타입 선택**: `int`는 20억 개의 할 일을 지원하며, 대부분의 개인 사용에 충분합니다. 엔터프라이즈 시스템에서는 `long` 또는 `Guid`를 고려합니다.

**2. Title (제목, 필수 필드):**
- **이론**: 정보 검색(Information Retrieval)에서 제목은 문서의 핵심 요약입니다. Salton의 VSM(Vector Space Model)에서 제목은 높은 가중치를 갖습니다.
- **제약 조건**: `[Required]` 속성(Data Annotations)이나 생성자 검증으로 null 불허를 강제할 수 있습니다.
- **길이 제한**: UX 연구에 따르면 화면에 표시 가능한 제목은 50-60자가 적절합니다.

**3. Description (설명, 선택적):**
- **이론**: 상세 설명은 제목의 보충 정보로, 검색 시 전체 텍스트 검색(Full-Text Search)의 대상이 됩니다.
- **NULL vs 빈 문자열**: NULL은 "알 수 없음(Unknown)"을, 빈 문자열은 "없음(Absent)"을 의미합니다. 여기서는 NULL을 사용하여 메모리를 절약합니다.

**4. IsCompleted (완료 여부, 상태 플래그):**
- **이론**: Boolean 플래그는 **유한 상태 기계(FSM)**의 가장 단순한 형태입니다. 2개 상태(미완료/완료)만 가집니다.
- **확장 가능성**: 더 복잡한 워크플로우(예: 대기→진행중→검토→완료)는 `enum Status { Pending, InProgress, Review, Completed }`로 확장 가능합니다.
- **소프트 삭제(Soft Delete)**: `IsDeleted` 플래그를 추가하여 데이터를 물리적으로 삭제하지 않고 논리적으로만 제거하는 패턴도 흔히 사용됩니다.

**5. CreatedAt (생성 일시, 감사 로그):**
- **이론**: 시간적 데이터베이스(Temporal Database) 이론에서 **트랜잭션 시간(Transaction Time)**을 기록합니다. Chris Date의 시간 데이터베이스 연구를 참고합니다.
- **불변성(Immutability)**: 생성 시간은 변경되지 않아야 하므로, 실무에서는 `{ get; init; }` 또는 `{ get; private set; }`를 사용합니다.
- **타임존(Timezone)**: `DateTime.Now`는 로컬 시간, `DateTime.UtcNow`는 UTC입니다. 국제화 앱에서는 UTC 저장 후 표시 시 로컬 변환이 표준입니다.

**6. DueDate (마감일, Nullable DateTime):**
- **이론**: Tony Hoare가 "Billion Dollar Mistake"라고 부른 NULL 참조의 적절한 사용 사례입니다. 모든 할 일에 마감일이 있는 것은 아니므로, `DateTime?`로 "없을 수 있음"을 명시합니다.
- **비교 연산**: `DateTime.Today`와 비교하여 오늘, 내일, 다음 주 등으로 분류할 수 있습니다.
- **알림 시스템**: 마감일 기반으로 `DueDate - TimeSpan.FromDays(1)` 같은 알림 로직을 구현할 수 있습니다.

**7. Priority (우선순위, 1-3 스케일):**
- **이론**: Eisenhower Matrix의 "중요도" 축을 단순화한 것입니다. 심리학 연구에 따르면, 인간은 7±2개 항목을 동시에 처리할 수 있으므로(Miller's Law), 3단계는 인지적으로 적절합니다.
- **확장성**: 실무에서는 `enum Priority { Low = 1, Medium = 2, High = 3, Critical = 4 }`로 명시적 열거형을 사용합니다.
- **정렬 알고리즘**: 우선순위는 정렬의 주요 키(Primary Key)로, `OrderByDescending(t => t.Priority)`로 높은 우선순위를 먼저 표시합니다.

**데이터 정규화와 반정규화:**

E.F. Codd의 정규형(Normal Forms) 이론에 따르면:

- **1NF(First Normal Form)**: 모든 속성은 원자 값(Atomic Value)입니다. ✓ 우리 모델은 1NF를 만족합니다.
- **2NF**: 부분 함수 종속성이 없습니다. ✓ 단일 테이블이므로 자동 만족합니다.
- **3NF**: 이행적 종속성이 없습니다. ✓ 모든 속성이 Id에만 종속됩니다.

실무에서는 성능을 위해 의도적으로 **반정규화(Denormalization)**하기도 하지만, 우리의 단순한 모델에서는 불필요합니다.

**메모리 효율성 분석:**

```
struct 크기 계산 (64비트 시스템):
- int Id: 4 bytes
- string Title: 8 bytes (참조) + 문자열 객체
- string Description: 8 bytes (참조) + 문자열 객체 또는 NULL
- bool IsCompleted: 1 byte (4 bytes로 패딩)
- DateTime CreatedAt: 8 bytes
- DateTime? DueDate: 9 bytes (1 byte hasValue + 8 bytes value)
- int Priority: 4 bytes
총 헤더 제외 최소: ~46 bytes + 문자열 데이터
```

1000개 할 일 항목도 약 50KB에 불과하므로, 메모리 최적화는 수만 개 규모에서 고려합니다.

### 초기 데이터 리스트와 메모리 관리

**컬렉션 선택의 이론적 기반:**

```csharp
List<TodoItem> todos = new List<TodoItem>();
int nextId = 1;
```

**List<T>의 내부 구조와 성능 특성:**

.NET의 `List<T>`는 **동적 배열(Dynamic Array)**로 구현되어 있으며, Donald Knuth의 "The Art of Computer Programming"에서 설명한 순차 메모리 할당 구조입니다:

**시간 복잡도(Time Complexity):**
- **추가(Add)**: O(1) amortized - 용량이 충분하면 O(1), 확장이 필요하면 O(n) 복사 발생
- **인덱스 접근**: O(1) - 배열의 기본 특성
- **검색(Find)**: O(n) - 선형 탐색
- **삽입/삭제**: O(n) - 요소 이동 필요

**공간 복잡도(Space Complexity):**
- O(n) - 항목 수에 비례
- **용량 확장 전략**: 현재 용량의 2배로 증가(Doubling Strategy), 이는 amortized O(1) 추가를 보장합니다.

**대안 컬렉션 비교:**

| 컬렉션 타입 | 추가 | 검색 | 삽입 | 메모리 | 사용 사례 |
|-----------|------|------|------|--------|---------|
| `List<T>` | O(1)* | O(n) | O(n) | 연속 | 일반적 목록, 인덱스 접근 |
| `LinkedList<T>` | O(1) | O(n) | O(1) | 분산 | 빈번한 삽입/삭제 |
| `HashSet<T>` | O(1) | O(1) | - | 해시 | 고유 항목, 빠른 검색 |
| `Dictionary<K,V>` | O(1) | O(1) | - | 해시 | 키-값 쌍, Id 기반 접근 |
| `SortedList<K,V>` | O(n) | O(log n) | O(n) | 연속 | 정렬 유지, 범위 쿼리 |

우리는 `List<T>`를 선택합니다:
1. **순차 접근이 주**: 대부분의 작업이 전체 목록 순회
2. **메모리 효율성**: 연속 메모리 할당으로 캐시 친화적
3. **단순성**: LINQ와의 완벽한 통합

**Id 생성 전략의 선택:**

```csharp
int nextId = 1;  // 단순 증가 카운터
```

**다양한 Id 생성 전략:**

1. **순차적 정수(Sequential Integer)**: 
   - 장점: 단순, 정렬 가능, 디버깅 용이
   - 단점: 분산 환경에서 충돌 가능, 보안 취약(예측 가능)
   
2. **GUID/UUID**:
   ```csharp
   Guid id = Guid.NewGuid();  // 128비트 전역 고유 식별자
   ```
   - 장점: 충돌 없음, 분산 생성 가능
   - 단점: 16바이트로 메모리 비효율, 인덱스 성능 저하

3. **Snowflake Algorithm** (Twitter):
   - 타임스탬프 + 머신 ID + 시퀀스 번호
   - 시간 순서 보장, 분산 환경 지원

4. **ULID (Universally Unique Lexicographically Sortable Identifier)**:
   - 시간 기반 + 랜덤
   - 정렬 가능 + 고유성

우리는 단일 사용자 앱이므로 순차적 정수가 최적입니다.

**스레드 안전성 고려:**

현재 구현은 **스레드 안전하지 않습니다(Not Thread-Safe)**. 멀티스레드 환경에서는:

```csharp
// Interlocked를 사용한 원자적 증가
int nextId = 0;
int GetNextId() => Interlocked.Increment(ref nextId);

// 또는 ConcurrentBag<T> 사용
ConcurrentBag<TodoItem> todos = new ConcurrentBag<TodoItem>();
```

하지만 콘솔 앱은 단일 스레드이므로 불필요한 복잡성입니다.

---

## 20.2 할 일 추가, 조회, 수정, 삭제

CRUD(Create, Read, Update, Delete) 패턴은 데이터베이스 시스템의 근간이며, James Martin의 "Strategic Data Planning"(1982)과 Roy Fielding의 REST 아키텍처(2000)를 통해 표준화되었습니다. 이 네 가지 기본 작업은 SQL의 INSERT, SELECT, UPDATE, DELETE와 정확히 대응되며, HTTP의 POST, GET, PUT/PATCH, DELETE 메서드로도 매핑됩니다.

**CRUD의 이론적 기반:**

1. **트랜잭션 ACID 속성**: Jim Gray와 Andreas Reuter의 트랜잭션 처리 이론에서:
   - **Atomicity(원자성)**: 작업은 전부 성공하거나 전부 실패
   - **Consistency(일관성)**: 데이터는 항상 유효한 상태 유지
   - **Isolation(격리성)**: 동시 작업이 서로 간섭하지 않음
   - **Durability(지속성)**: 완료된 작업은 영구적으로 저장

2. **명령-쿼리 분리(CQS, Command-Query Separation)**: Bertrand Meyer의 원칙에 따라:
   - **명령(Command)**: Create, Update, Delete - 상태 변경, 반환값 없음
   - **쿼리(Query)**: Read - 상태 변경 없음, 값 반환
   
3. **멱등성(Idempotence)**: REST 원칙에서 중요한 개념:
   - **멱등**: GET, PUT, DELETE - 여러 번 실행해도 결과 동일
   - **비멱등**: POST - 실행할 때마다 새 리소스 생성

**소프트웨어 패턴의 적용:**

- **저장소 패턴(Repository Pattern)**: Martin Fowler의 P of EAA에서 제시한 데이터 접근 추상화
- **서비스 레이어**: 비즈니스 로직의 캡슐화와 트랜잭션 경계 정의
- **DTO(Data Transfer Object)**: 계층 간 데이터 전송을 위한 순수 데이터 객체

### 할 일 추가 (Create Operation)

**생성 작업의 설계 원칙:**

Create 작업은 **팩토리 패턴(Factory Pattern)**의 단순화된 형태입니다. GoF의 디자인 패턴에서 팩토리는 객체 생성 로직을 캡슐화하여, 클라이언트 코드를 생성 세부사항으로부터 분리합니다.

```csharp
void AddTodo()
{
    Console.WriteLine("\n[ 할 일 추가 ]");
    
    Console.Write("제목: ");
    string title = Console.ReadLine();
    
    if (string.IsNullOrWhiteSpace(title))
    {
        Console.WriteLine("❌ 제목은 필수입니다.");
        return;
    }
    
    Console.Write("설명 (선택): ");
    string description = Console.ReadLine();
    
    Console.Write("우선순위 (1:낮음, 2:보통, 3:높음): ");
    if (!int.TryParse(Console.ReadLine(), out int priority) || priority < 1 || priority > 3)
    {
        priority = 2; // 기본값: 보통
    }
    
    var todo = new TodoItem
    {
        Id = nextId++,
        Title = title,
        Description = description,
        IsCompleted = false,
        CreatedAt = DateTime.Now,
        Priority = priority
    };
    
    todos.Add(todo);
    Console.WriteLine($"✓ 할 일이 추가되었습니다. (ID: {todo.Id})");
}
```

**코드 분석 - 계층별 책임:**

**1. 입력 검증 레이어(Input Validation Layer):**

```csharp
if (string.IsNullOrWhiteSpace(title))
```

이는 **방어적 프로그래밍(Defensive Programming)**의 핵심입니다. Barbara Liskov의 "Design by Contract"에 따르면:
- **전제조건(Precondition)**: Title은 null이나 공백이 아니어야 함
- **후조건(Postcondition)**: 유효한 TodoItem이 생성됨
- **불변조건(Invariant)**: 리스트의 모든 항목은 유효한 Title을 가짐

`TryParse` 패턴은 .NET의 **Try-Parse 관용구(Try-Parse Idiom)**로, 예외 대신 불린 반환으로 성능을 개선합니다:
- **예외 기반**: `Parse()` - 실패 시 `FormatException`, 느림
- **TryParse**: 실패 시 `false` 반환, 약 100배 빠름 (예외 처리 오버헤드 없음)

**2. 비즈니스 로직 레이어(Business Logic Layer):**

```csharp
var todo = new TodoItem { /* ... */ };
```

**객체 초기화 구문(Object Initializer Syntax)**은 C# 3.0의 기능으로, 다음과 동등합니다:

```csharp
// 전통적 방식 (C# 1.0)
TodoItem todo = new TodoItem();
todo.Id = nextId++;
todo.Title = title;
// ...

// 현대적 방식 (C# 3.0+)
var todo = new TodoItem { Id = nextId++, Title = title, /* ... */ };
```

이는 **빌더 패턴(Builder Pattern)**의 언어 수준 지원으로 볼 수 있습니다.

**3. 데이터 접근 레이어(Data Access Layer):**

```csharp
todos.Add(todo);
```

`List<T>.Add()`의 내부 동작:
1. 용량 확인: `count < capacity`?
2. 충분하면: `items[count++] = todo;` - O(1)
3. 부족하면: 
   - 새 배열 할당: `newCapacity = capacity * 2`
   - 기존 요소 복사: `Array.Copy()` - O(n)
   - 새 항목 추가 - O(1)

Amortized 분석(Mark Allen Weiss)에 따르면, n개 추가의 총 비용은 O(n)이므로, 평균 O(1)입니다.

**Id 생성의 동시성 문제:**

```csharp
Id = nextId++
```

이는 **Read-Modify-Write** 패턴으로, 3개의 원자적 작업:
1. Read: `temp = nextId`
2. Modify: `temp = temp + 1`
3. Write: `nextId = temp`

멀티스레드 환경에서는 **경쟁 조건(Race Condition)**이 발생할 수 있습니다:

```
Thread A: Read(1) → Modify(2) → ...
Thread B: Read(1) → ... → Write(2)  // 동일한 Id!
Thread A: ... → Write(2)             // 덮어쓰기
```

해결책: `Interlocked.Increment(ref nextId)`로 원자적 증가 보장.

**시간 정확도와 타임존:**

```csharp
CreatedAt = DateTime.Now
```

- `DateTime.Now`: 로컬 시간 (DST 영향 받음)
- `DateTime.UtcNow`: UTC 시간 (권장, 국제화 앱)
- `DateTimeOffset.Now`: 타임존 오프셋 포함 (최고 정확도)

John Skeet의 "Noda Time" 라이브러리는 `DateTime`의 함정을 피하기 위한 대안입니다.

**기본값 전략(Default Value Strategy):**

```csharp
priority = 2;  // 기본값: 보통
```

이는 **Convention over Configuration** 원칙의 적용입니다. David Heinemeier Hansson의 Ruby on Rails에서 유명해진 이 원칙은 합리적 기본값을 제공하여 사용자 부담을 줄입니다.

### 할 일 목록 조회 (Read Operation)

**조회 작업의 패턴:**

Read 작업은 **반복자 패턴(Iterator Pattern)**과 **템플릿 메서드 패턴(Template Method Pattern)**을 활용합니다. GoF 패턴에서 반복자는 컬렉션의 내부 구조를 노출하지 않고 요소에 순차 접근합니다.

```csharp
void ListTodos()
{
    Console.WriteLine("\n[ 할 일 목록 ]");
    
    if (todos.Count == 0)
    {
        Console.WriteLine("할 일이 없습니다.");
        return;
    }
    
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━");
    foreach (var todo in todos)
    {
        string status = todo.IsCompleted ? "✓" : " ";
        string priority = GetPriorityString(todo.Priority);
        Console.WriteLine($"[{status}] {todo.Id}. {todo.Title} {priority}");
        
        if (!string.IsNullOrEmpty(todo.Description))
        {
            Console.WriteLine($"    📝 {todo.Description}");
        }
        
        if (todo.DueDate.HasValue)
        {
            Console.WriteLine($"    📅 마감: {todo.DueDate.Value:yyyy-MM-dd}");
        }
    }
    Console.WriteLine("━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━");
}

string GetPriorityString(int priority)
{
    return priority switch
    {
        1 => "[낮음]",
        2 => "[보통]",
        3 => "[높음]",
        _ => ""
    };
}
```

### 할 일 완료 표시

```csharp
void CompleteTodo()
{
    Console.WriteLine("\n[ 할 일 완료 ]");
    
    Console.Write("완료할 할 일 ID: ");
    if (!int.TryParse(Console.ReadLine(), out int id))
    {
        Console.WriteLine("❌ 올바른 ID를 입력하세요.");
        return;
    }
    
    var todo = todos.FirstOrDefault(t => t.Id == id);
    if (todo == null)
    {
        Console.WriteLine("❌ 해당 ID의 할 일을 찾을 수 없습니다.");
        return;
    }
    
    todo.IsCompleted = true;
    Console.WriteLine($"✓ '{todo.Title}'이(가) 완료되었습니다.");
}
```

### 할 일 삭제

```csharp
void DeleteTodo()
{
    Console.WriteLine("\n[ 할 일 삭제 ]");
    
    Console.Write("삭제할 할 일 ID: ");
    if (!int.TryParse(Console.ReadLine(), out int id))
    {
        Console.WriteLine("❌ 올바른 ID를 입력하세요.");
        return;
    }
    
    var todo = todos.FirstOrDefault(t => t.Id == id);
    if (todo == null)
    {
        Console.WriteLine("❌ 해당 ID의 할 일을 찾을 수 없습니다.");
        return;
    }
    
    Console.Write($"정말로 '{todo.Title}'을(를) 삭제하시겠습니까? (y/n): ");
    if (Console.ReadLine()?.ToLower() == "y")
    {
        todos.Remove(todo);
        Console.WriteLine("✓ 삭제되었습니다.");
    }
    else
    {
        Console.WriteLine("취소되었습니다.");
    }
}
```

---

## 20.3 파일로 데이터 저장 및 불러오기

데이터 영속성(Data Persistence)은 소프트웨어 시스템의 근본적 요구사항입니다. Martin Fowler의 "Patterns of Enterprise Application Architecture"와 Michael Stonebraker의 데이터베이스 연구는 영속성의 이론적 기반을 제공합니다.

**영속성 계층의 진화:**

1. **파일 시스템(1960s)**: 순차/랜덤 접근 파일, ISAM(Indexed Sequential Access Method)
2. **계층형 데이터베이스(1970s)**: IBM의 IMS(Information Management System)
3. **관계형 데이터베이스(1980s)**: Oracle, SQL Server, PostgreSQL
4. **NoSQL(2000s)**: MongoDB, Cassandra, Redis
5. **NewSQL(2010s)**: CockroachDB, Google Spanner

우리는 가장 단순한 파일 기반 영속성을 사용하지만, 이는 **저장소 패턴(Repository Pattern)**의 구현으로, 나중에 데이터베이스로 쉽게 전환할 수 있습니다.

**직렬화의 이론적 기반:**

**직렬화(Serialization)**는 메모리의 객체 그래프를 바이트 스트림으로 변환하는 과정입니다. 이는 다음과 같은 컴퓨터 과학 문제를 해결합니다:

1. **그래프 순회(Graph Traversal)**: DFS(Depth-First Search)로 객체 참조 추적
2. **순환 참조(Circular Reference)**: 무한 루프 방지를 위한 방문 표시
3. **타입 정보 보존**: 역직렬화 시 올바른 타입 재구성
4. **버전 관리(Versioning)**: 스키마 변경 시 하위 호환성

**JSON vs 다른 직렬화 형식:**

| 형식 | 크기 | 속도 | 인간 가독성 | 타입 안전성 | 사용 사례 |
|------|------|------|------------|-----------|---------|
| **JSON** | 보통 | 보통 | 높음 | 낮음 | REST API, 설정 파일, 간단한 데이터 교환 |
| **XML** | 큼 | 느림 | 높음 | 중간 | 레거시 시스템, SOAP, 문서 표준 |
| **Protocol Buffers** | 작음 | 빠름 | 낮음 | 높음 | gRPC, 고성능 마이크로서비스 |
| **MessagePack** | 작음 | 빠름 | 낮음 | 중간 | 바이너리 데이터, 네트워크 전송 |
| **Binary Serialization** | 작음 | 빠름 | 없음 | 높음 | .NET 전용, 원격 호출 |

JSON을 선택한 이유:
1. **인간 가독성**: 디버깅과 수동 편집 용이
2. **플랫폼 독립성**: 모든 언어와 플랫폼 지원
3. **간단함**: 복잡한 스키마 정의 불필요
4. **웹 표준**: JavaScript와 완벽 통합

**System.Text.Json vs Newtonsoft.Json:**

.NET Core 3.0부터 도입된 `System.Text.Json`은:
- **성능**: Newtonsoft보다 2-3배 빠름, 메모리 할당 적음
- **보안**: 기본적으로 안전한 설정, DoS 공격 방어
- **표준**: .NET 팀의 공식 지원, 장기 유지보수 보장

하지만 Newtonsoft는 더 많은 기능을 제공합니다 (복잡한 타입 변환, 커스터마이징).

### JSON 직렬화 및 저장

**저장 작업의 아키텍처:**

```csharp
using System.Text.Json;

void SaveToFile()
{
    try
    {
        string fileName = "todos.json";
        string json = JsonSerializer.Serialize(todos, new JsonSerializerOptions 
        { 
            WriteIndented = true 
        });
        
        File.WriteAllText(fileName, json);
        Console.WriteLine($"✓ {fileName}에 저장되었습니다.");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"❌ 저장 실패: {ex.Message}");
    }
}
```

### JSON 역직렬화 및 불러오기

```csharp
void LoadFromFile()
{
    try
    {
        string fileName = "todos.json";
        
        if (!File.Exists(fileName))
        {
            Console.WriteLine("저장된 파일이 없습니다.");
            return;
        }
        
        string json = File.ReadAllText(fileName);
        var loadedTodos = JsonSerializer.Deserialize<List<TodoItem>>(json);
        
        if (loadedTodos != null && loadedTodos.Count > 0)
        {
            todos = loadedTodos;
            nextId = todos.Max(t => t.Id) + 1;
            Console.WriteLine($"✓ {loadedTodos.Count}개의 할 일을 불러왔습니다.");
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine($"❌ 불러오기 실패: {ex.Message}");
    }
}
```

**JSON 파일 예시:**
```json
[
  {
    "Id": 1,
    "Title": "C# 프로젝트 완료하기",
    "Description": "20장 할 일 관리 앱 구현",
    "IsCompleted": false,
    "CreatedAt": "2024-01-15T10:30:00",
    "DueDate": "2024-01-20T00:00:00",
    "Priority": 3
  }
]
```

---

## 20.4 LINQ를 활용한 검색 및 필터링

LINQ(Language-Integrated Query)는 C# 3.0(2007)에서 도입된 혁명적 기능으로, Erik Meijer와 Microsoft Research 팀의 작품입니다. LINQ는 SQL의 선언적 쿼리, Haskell의 리스트 컴프리헨션, 그리고 람다 계산(Lambda Calculus)을 C#에 통합했습니다.

**LINQ의 이론적 기반:**

1. **관계 대수(Relational Algebra)**: E.F. Codd의 관계형 데이터베이스 이론
   - Selection (σ): `Where`
   - Projection (π): `Select`
   - Cartesian Product (×): `SelectMany`
   - Join (⋈): `Join`
   - Union (∪), Intersection (∩), Difference (−): `Union`, `Intersect`, `Except`

2. **모나드(Monad)**: Haskell의 함수형 프로그래밍 개념
   - LINQ는 "리스트 모나드(List Monad)"의 C# 구현
   - `SelectMany`는 `bind` (>>=) 연산자
   - `Select`는 `fmap` (functor map)

3. **지연 실행(Deferred Execution)**: 
   - **게으른 평가(Lazy Evaluation)**: Haskell의 핵심 특성
   - **pull 모델**: 필요할 때만 계산 (대조: push 모델의 eager evaluation)
   - **메모리 효율성**: 전체 결과를 메모리에 로드하지 않음

**LINQ 쿼리의 두 가지 구문:**

```csharp
// 쿼리 구문 (Query Syntax) - SQL 유사
var results = from todo in todos
              where !todo.IsCompleted
              orderby todo.Priority descending
              select todo;

// 메서드 구문 (Method Syntax) - 함수형
var results = todos
    .Where(todo => !todo.IsCompleted)
    .OrderByDescending(todo => todo.Priority);
```

두 구문은 동일한 IL 코드로 컴파일됩니다. 메서드 구문이 더 강력하고 유연합니다.

**쿼리 실행 모델:**

```csharp
var query = todos.Where(t => t.Priority == 3);  // 아직 실행 안 됨!
// ... 더 많은 연산 추가 가능
var results = query.ToList();  // 여기서 실행됨!
```

이는 **빌더 패턴(Builder Pattern)**과 유사하며, SQL의 쿼리 최적화기(Query Optimizer)처럼 작동합니다.

**표준 쿼리 연산자(Standard Query Operators)의 복잡도:**

| 연산자 | 시간 복잡도 | 공간 복잡도 | 지연 실행 | 설명 |
|--------|-----------|-----------|---------|------|
| `Where` | O(n) | O(1) | ✓ | 조건 필터링 |
| `Select` | O(n) | O(1) | ✓ | 투영(projection) |
| `OrderBy` | O(n log n) | O(n) | ✓ | 퀵소트 기반 정렬 |
| `GroupBy` | O(n) | O(n) | ✓ | 해시 기반 그룹화 |
| `Join` | O(n+m) | O(n+m) | ✓ | 해시 조인 |
| `ToList/ToArray` | O(n) | O(n) | ✗ | 즉시 실행, 메모리 로드 |
| `Count` | O(n) 또는 O(1) | O(1) | ✗ | 컬렉션에 따라 다름 |
| `First` | O(1) | O(1) | ✗ | 첫 요소만 |
| `Any` | O(1) - O(n) | O(1) | ✗ | 조기 종료 가능 |

**함수형 프로그래밍의 원칙:**

LINQ는 다음 FP 원칙을 따릅니다:

1. **불변성(Immutability)**: 원본 컬렉션을 변경하지 않고 새 컬렉션 반환
2. **순수 함수(Pure Functions)**: 람다 표현식은 부작용 없어야 함
3. **고차 함수(Higher-Order Functions)**: 함수를 매개변수로 받음 (`Where`, `Select` 등)
4. **합성(Composition)**: 메서드 체이닝으로 복잡한 쿼리 구축

### 제목으로 검색 - 전체 텍스트 검색의 기초

**정보 검색(Information Retrieval)의 원리:**

```csharp
void SearchTodos()
{
    Console.WriteLine("\n[ 할 일 검색 ]");
    Console.Write("검색어: ");
    string keyword = Console.ReadLine()?.ToLower();
    
    if (string.IsNullOrWhiteSpace(keyword))
    {
        Console.WriteLine("검색어를 입력하세요.");
        return;
    }
    
    var results = todos.Where(t => 
        t.Title.ToLower().Contains(keyword) || 
        (t.Description != null && t.Description.ToLower().Contains(keyword))
    ).ToList();
    
    Console.WriteLine($"\n검색 결과: {results.Count}개");
    foreach (var todo in results)
    {
        string status = todo.IsCompleted ? "✓" : " ";
        Console.WriteLine($"[{status}] {todo.Id}. {todo.Title}");
    }
}
```

### 미완료 항목만 조회

```csharp
void ShowIncompleteTodos()
{
    var incomplete = todos.Where(t => !t.IsCompleted).ToList();
    
    Console.WriteLine($"\n미완료 할 일: {incomplete.Count}개");
    foreach (var todo in incomplete)
    {
        Console.WriteLine($"[ ] {todo.Id}. {todo.Title}");
    }
}
```

### 우선순위별 정렬

```csharp
void ShowTodosByPriority()
{
    Console.WriteLine("\n[ 우선순위별 할 일 ]");
    
    var sorted = todos
        .OrderByDescending(t => t.Priority)
        .ThenBy(t => t.DueDate)
        .ToList();
    
    foreach (var todo in sorted)
    {
        string status = todo.IsCompleted ? "✓" : " ";
        string priority = GetPriorityString(todo.Priority);
        Console.WriteLine($"[{status}] {priority} {todo.Title}");
    }
}
```

### 완료율 통계

```csharp
void ShowStatistics()
{
    Console.WriteLine("\n[ 통계 ]");
    
    int total = todos.Count;
    int completed = todos.Count(t => t.IsCompleted);
    int incomplete = total - completed;
    double completionRate = total > 0 ? (completed * 100.0 / total) : 0;
    
    Console.WriteLine($"전체: {total}개");
    Console.WriteLine($"완료: {completed}개");
    Console.WriteLine($"미완료: {incomplete}개");
    Console.WriteLine($"완료율: {completionRate:F1}%");
    
    // 우선순위별 통계
    var byPriority = todos.GroupBy(t => t.Priority)
        .Select(g => new { Priority = g.Key, Count = g.Count() });
    
    Console.WriteLine("\n우선순위별:");
    foreach (var group in byPriority)
    {
        Console.WriteLine($"  {GetPriorityString(group.Priority)}: {group.Count}개");
    }
}
```

---

## 20.5 우선순위 및 마감일 관리

시간 관리(Time Management)와 우선순위 설정은 생산성 시스템의 핵심입니다. David Allen의 GTD(Getting Things Done), Stephen Covey의 "7 Habits", 그리고 Eisenhower Matrix는 이 분야의 이론적 기반을 제공합니다.

**시간 기반 데이터의 컴퓨터 과학:**

1. **시간적 데이터베이스(Temporal Database)**: 
   - **유효 시간(Valid Time)**: 실제 세계에서 사실이 참인 시간 (우리의 DueDate)
   - **트랜잭션 시간(Transaction Time)**: 데이터베이스에 저장된 시간 (CreatedAt)
   - Chris Date와 Richard Snodgrass의 연구

2. **시간 복잡도의 다른 의미**: 
   - 알고리즘의 시간 복잡도(Big-O)와 구별되는, 실제 시간(Clock Time) 관리

3. **우선순위 큐(Priority Queue)**:
   - 힙(Heap) 자료구조로 구현: O(log n) 삽입/삭제
   - 우리는 단순 정렬 사용: O(n log n)
   - 대량 데이터에서는 힙이 효율적

**Eisenhower Matrix의 디지털화:**

```
           긴급(Urgent)        긴급하지 않음
중요    | 1. 즉시 실행      | 2. 계획하여 실행
(Important)|  Priority=3     |    Priority=2
----------|------------------|------------------
중요하지  | 3. 위임/최소화   | 4. 제거
않음     |  Priority=1      |    (저장 안 함)
```

**DateTime 처리의 복잡성:**

Jon Skeet의 "Noda Time"은 .NET의 `DateTime`이 가진 문제들을 지적합니다:

1. **Kind 모호성**: Local, UTC, Unspecified의 혼란
2. **타임존 부재**: `DateTime`은 타임존 정보를 저장하지 않음
3. **DST(Daylight Saving Time)**: 일광 절약 시간제로 인한 모호한 시간
4. **윤초(Leap Second)**: 정확한 시간 계산의 복잡성

**날짜 비교와 범위 쿼리:**

```csharp
// 오늘
DateTime.Today

// 날짜만 비교 (시간 무시)
dueDate.Value.Date == DateTime.Today

// 범위 쿼리 (7일 이내)
dueDate.Value.Date <= DateTime.Today.AddDays(7)
```

SQL에서는 `BETWEEN`, 관계 대수에서는 **선택(Selection)** 연산에 해당합니다.

### 마감일 설정 - Update 작업의 세분화

```csharp
void SetDueDate()
{
    Console.WriteLine("\n[ 마감일 설정 ]");
    
    Console.Write("할 일 ID: ");
    if (!int.TryParse(Console.ReadLine(), out int id))
    {
        Console.WriteLine("❌ 올바른 ID를 입력하세요.");
        return;
    }
    
    var todo = todos.FirstOrDefault(t => t.Id == id);
    if (todo == null)
    {
        Console.WriteLine("❌ 해당 ID의 할 일을 찾을 수 없습니다.");
        return;
    }
    
    Console.Write("마감일 (yyyy-MM-dd): ");
    if (DateTime.TryParse(Console.ReadLine(), out DateTime dueDate))
    {
        todo.DueDate = dueDate;
        Console.WriteLine($"✓ 마감일이 설정되었습니다: {dueDate:yyyy-MM-dd}");
    }
    else
    {
        Console.WriteLine("❌ 올바른 날짜 형식이 아닙니다.");
    }
}
```

### 마감 임박 항목 조회

```csharp
void ShowUpcomingTodos()
{
    Console.WriteLine("\n[ 마감 임박 할 일 ]");
    
    var upcoming = todos
        .Where(t => !t.IsCompleted && t.DueDate.HasValue)
        .Where(t => t.DueDate.Value.Date >= DateTime.Today)
        .Where(t => t.DueDate.Value.Date <= DateTime.Today.AddDays(7))
        .OrderBy(t => t.DueDate)
        .ToList();
    
    if (upcoming.Count == 0)
    {
        Console.WriteLine("마감 임박한 할 일이 없습니다.");
        return;
    }
    
    foreach (var todo in upcoming)
    {
        int daysLeft = (todo.DueDate.Value.Date - DateTime.Today).Days;
        string urgency = daysLeft switch
        {
            0 => "🔴 오늘",
            1 => "🟡 내일",
            _ => $"🟢 {daysLeft}일 후"
        };
        
        Console.WriteLine($"{urgency} - {todo.Title}");
    }
}
```

### 우선순위 변경

```csharp
void ChangePriority()
{
    Console.WriteLine("\n[ 우선순위 변경 ]");
    
    Console.Write("할 일 ID: ");
    if (!int.TryParse(Console.ReadLine(), out int id))
    {
        Console.WriteLine("❌ 올바른 ID를 입력하세요.");
        return;
    }
    
    var todo = todos.FirstOrDefault(t => t.Id == id);
    if (todo == null)
    {
        Console.WriteLine("❌ 해당 ID의 할 일을 찾을 수 없습니다.");
        return;
    }
    
    Console.WriteLine($"현재 우선순위: {GetPriorityString(todo.Priority)}");
    Console.Write("새 우선순위 (1:낮음, 2:보통, 3:높음): ");
    
    if (int.TryParse(Console.ReadLine(), out int newPriority) && newPriority >= 1 && newPriority <= 3)
    {
        todo.Priority = newPriority;
        Console.WriteLine($"✓ 우선순위가 {GetPriorityString(newPriority)}(으)로 변경되었습니다.");
    }
    else
    {
        Console.WriteLine("❌ 올바른 우선순위를 입력하세요 (1-3).");
    }
}
```

### 지난 마감일 경고

```csharp
void ShowOverdueTodos()
{
    Console.WriteLine("\n[ 마감 지난 할 일 ]");
    
    var overdue = todos
        .Where(t => !t.IsCompleted && t.DueDate.HasValue)
        .Where(t => t.DueDate.Value.Date < DateTime.Today)
        .OrderBy(t => t.DueDate)
        .ToList();
    
    if (overdue.Count == 0)
    {
        Console.WriteLine("✓ 마감 지난 할 일이 없습니다.");
        return;
    }
    
    Console.WriteLine($"⚠️  {overdue.Count}개의 할 일이 마감을 넘겼습니다:");
    foreach (var todo in overdue)
    {
        int daysOverdue = (DateTime.Today - todo.DueDate.Value.Date).Days;
        Console.WriteLine($"🔴 {todo.Title} - {daysOverdue}일 초과");
    }
}
```

---

## 완전한 프로그램 예시

모든 기능을 통합한 메뉴 시스템:

```csharp
using System.Text.Json;

List<TodoItem> todos = new List<TodoItem>();
int nextId = 1;

// 프로그램 시작 시 파일에서 불러오기
LoadFromFile();

Console.WriteLine("═══════════════════════════════");
Console.WriteLine("   할 일 관리 앱 v1.0");
Console.WriteLine("═══════════════════════════════");

while (true)
{
    Console.WriteLine("\n메뉴:");
    Console.WriteLine("  1. 할 일 추가");
    Console.WriteLine("  2. 할 일 목록");
    Console.WriteLine("  3. 할 일 완료");
    Console.WriteLine("  4. 할 일 삭제");
    Console.WriteLine("  5. 검색");
    Console.WriteLine("  6. 미완료 항목");
    Console.WriteLine("  7. 우선순위별 보기");
    Console.WriteLine("  8. 통계");
    Console.WriteLine("  9. 마감일 설정");
    Console.WriteLine(" 10. 마감 임박 항목");
    Console.WriteLine(" 11. 우선순위 변경");
    Console.WriteLine(" 12. 저장");
    Console.WriteLine("  0. 종료");
    Console.Write("선택: ");
    
    string choice = Console.ReadLine();
    
    switch (choice)
    {
        case "1": AddTodo(); break;
        case "2": ListTodos(); break;
        case "3": CompleteTodo(); break;
        case "4": DeleteTodo(); break;
        case "5": SearchTodos(); break;
        case "6": ShowIncompleteTodos(); break;
        case "7": ShowTodosByPriority(); break;
        case "8": ShowStatistics(); break;
        case "9": SetDueDate(); break;
        case "10": ShowUpcomingTodos(); break;
        case "11": ChangePriority(); break;
        case "12": SaveToFile(); break;
        case "0":
            Console.Write("저장하고 종료하시겠습니까? (y/n): ");
            if (Console.ReadLine()?.ToLower() == "y")
            {
                SaveToFile();
            }
            Console.WriteLine("프로그램을 종료합니다.");
            return;
        default:
            Console.WriteLine("❌ 올바른 메뉴를 선택하세요.");
            break;
    }
}

// 여기에 앞서 정의한 모든 메서드들을 추가
```

---

## 20장 정리 및 소프트웨어 공학의 통합적 이해

이 장에서 구현한 할 일 관리 앱은 단순한 CRUD 애플리케이션을 넘어서, **현대 소프트웨어 개발의 핵심 원칙과 패턴**을 통합한 실무 시스템입니다. Frederick Brooks의 "The Mythical Man-Month"에서 강조한 것처럼, 우리는 **본질적 복잡성(Essential Complexity)**과 **우발적 복잡성(Accidental Complexity)**을 분리하여 관리하는 방법을 학습했습니다.

### 핵심 개념의 이론적 통합

**1. 도메인 주도 설계(Domain-Driven Design)**

Eric Evans의 DDD 원칙을 통해 현실 세계의 개념을 소프트웨어로 변환했습니다:

- **엔티티(Entity)**: `TodoItem`은 고유 식별자로 정의되는 도메인 객체
- **값 객체(Value Object)**: `Priority`, `DueDate`는 불변 값으로 표현 가능
- **집합 루트(Aggregate Root)**: TodoItem은 자신의 생명주기를 관리
- **유비쿼터스 언어(Ubiquitous Language)**: 도메인 전문가와 개발자가 공유하는 용어를 코드에 직접 반영
- **경계 컨텍스트(Bounded Context)**: 할 일 관리 도메인의 명확한 범위 정의

**2. 아키텍처 패턴의 실천**

Robert C. Martin(Uncle Bob)의 Clean Architecture와 Alistair Cockburn의 Hexagonal Architecture를 단순화하여 적용했습니다:

```
프레젠테이션 계층 (Presentation Layer)
  ↓ Console I/O, 메뉴 시스템
비즈니스 로직 계층 (Business Logic Layer)
  ↓ CRUD 작업, 검증, 우선순위 관리
데이터 접근 계층 (Data Access Layer)
  ↓ List<TodoItem>, JSON 직렬화
영속성 계층 (Persistence Layer)
  ↓ File System
```

**관심사의 분리(Separation of Concerns)**를 통해:
- 각 계층은 독립적으로 테스트 가능
- UI를 콘솔에서 GUI로 전환 용이
- 저장소를 파일에서 데이터베이스로 전환 용이

**3. CRUD와 REST의 대응**

Roy Fielding의 REST 아키텍처와 우리 구현의 매핑:

| CRUD | HTTP | SQL | 우리 구현 | 멱등성 | 안전성 |
|------|------|-----|---------|--------|-------|
| Create | POST | INSERT | `AddTodo()` | ✗ | ✗ |
| Read | GET | SELECT | `ListTodos()` | ✓ | ✓ |
| Update | PUT/PATCH | UPDATE | `CompleteTodo()` | ✓ | ✗ |
| Delete | DELETE | DELETE | `DeleteTodo()` | ✓ | ✗ |

이러한 매핑을 이해하면 웹 API 개발로 자연스럽게 확장할 수 있습니다.

**4. 함수형 프로그래밍과 LINQ**

Erik Meijer의 LINQ는 다음 FP 개념을 C#에 도입했습니다:

- **모나드(Monad)**: IEnumerable<T>는 리스트 모나드
- **펑터(Functor)**: `Select`는 fmap 연산
- **필터(Filter)**: `Where`는 조건부 선택
- **리덕션(Reduction)**: `Aggregate`는 fold 연산
- **합성(Composition)**: 메서드 체이닝으로 파이프라인 구축

**지연 실행(Deferred Execution)**의 장점:
```csharp
var query = todos
    .Where(t => !t.IsCompleted)      // 아직 실행 안 됨
    .OrderBy(t => t.Priority)        // 쿼리 구축만
    .Select(t => t.Title);           // 최적화 가능

var results = query.ToList();        // 여기서 한 번에 실행
```

이는 SQL 쿼리 최적화기(Query Optimizer)처럼 작동하며, 불필요한 중간 컬렉션 생성을 방지합니다.

**5. 데이터 영속성과 직렬화**

Martin Fowler의 저장소 패턴을 파일 시스템으로 구현했습니다:

**추상화 계층**:
```csharp
// 인터페이스 (현재는 암묵적)
interface ITodoRepository
{
    void Save(List<TodoItem> todos);
    List<TodoItem> Load();
}

// 파일 구현 (현재)
class FileSystemTodoRepository : ITodoRepository

// 미래 확장 가능
class SqliteTodoRepository : ITodoRepository
class CloudTodoRepository : ITodoRepository
```

**직렬화의 컴퓨터 과학적 관점**:
- **그래프 직렬화**: 객체 참조 그래프를 선형 바이트 스트림으로 변환
- **타입 보존**: 역직렬화 시 올바른 타입 재구성
- **버전 관리**: 스키마 진화(Schema Evolution) 지원

**6. 시간 관리와 우선순위 알고리즘**

David Allen의 GTD와 Eisenhower Matrix를 코드로 구현:

```csharp
// Priority Queue의 단순 구현
var sorted = todos
    .OrderByDescending(t => t.Priority)  // 1차: 우선순위
    .ThenBy(t => t.DueDate)             // 2차: 마감일
    .ToList();
```

실무에서는 **힙(Heap)** 자료구조를 사용:
- `PriorityQueue<T>` (.NET 6+)
- 삽입/삭제: O(log n)
- 최고 우선순위 조회: O(1)

### 소프트웨어 공학 원칙의 적용

**SOLID 원칙의 실천:**

1. **S**ingle Responsibility: 각 메서드는 하나의 책임만 가짐
2. **O**pen-Closed: 새 기능 추가 시 기존 코드 수정 최소화
3. **L**iskov Substitution: TodoItem의 불변조건 유지
4. **I**nterface Segregation: 필요한 메서드만 노출
5. **D**ependency Inversion: 구체적 구현이 아닌 추상화에 의존

**DRY (Don't Repeat Yourself)**:
- `GetPriorityString()`: 우선순위 표시 로직 재사용
- `GetNumberInput()`: 숫자 입력 검증 재사용 (19장에서 학습)

**YAGNI (You Aren't Gonna Need It)**:
- 복잡한 기능 대신 MVP(Minimum Viable Product)에 집중
- 필요할 때 점진적으로 확장

**KISS (Keep It Simple, Stupid)**:
- 복잡한 디자인 패턴 대신 직관적인 구조
- 가독성 우선, 조기 최적화 배제

### 성능 분석과 확장성

**시간 복잡도 분석:**

| 작업 | 복잡도 | 설명 |
|------|--------|------|
| 추가 (Add) | O(1) amortized | List 동적 확장 |
| 조회 (List) | O(n) | 전체 순회 |
| 검색 (Search) | O(n×m) | n개 항목, m은 문자열 길이 |
| 정렬 (Sort) | O(n log n) | QuickSort/MergeSort |
| 필터 (Filter) | O(n) | 선형 스캔 |
| 완료/삭제 | O(n) | FirstOrDefault + 작업 |

**확장성 고려사항:**

1. **메모리 확장성**: 
   - 현재: O(n) 메모리, 모든 항목을 메모리에 로드
   - 대안: 페이징(Paging), 지연 로딩(Lazy Loading)

2. **검색 성능**:
   - 현재: O(n) 선형 검색
   - 개선: 인덱싱(Dictionary<int, TodoItem>), 전체 텍스트 검색 엔진(Lucene.NET)

3. **동시성(Concurrency)**:
   - 현재: 단일 스레드, 스레드 안전하지 않음
   - 개선: `ConcurrentBag<T>`, 낙관적 동시성 제어(Optimistic Concurrency)

### 실무로의 확장 경로

**단기 개선 (1-2주):**

1. **더 나은 UI**:
   ```csharp
   Console.ForegroundColor = ConsoleColor.Red;  // 긴급 항목
   Console.ForegroundColor = ConsoleColor.Yellow;  // 중요 항목
   ```

2. **설정 파일**:
   ```csharp
   // appsettings.json
   {
     "DefaultPriority": 2,
     "MaxTodoCount": 1000,
     "AutoSaveInterval": 300
   }
   ```

3. **로깅(Logging)**:
   ```csharp
   using Microsoft.Extensions.Logging;
   logger.LogInformation("Todo {Id} created", todo.Id);
   ```

**중기 개선 (1-2개월):**

1. **데이터베이스 전환**:
   ```csharp
   // SQLite with Entity Framework Core
   public class TodoDbContext : DbContext
   {
       public DbSet<TodoItem> Todos { get; set; }
   }
   ```

2. **웹 API**:
   ```csharp
   [ApiController]
   [Route("api/[controller]")]
   public class TodosController : ControllerBase
   {
       [HttpGet]
       public ActionResult<List<TodoItem>> GetTodos() { }
       
       [HttpPost]
       public ActionResult<TodoItem> CreateTodo([FromBody] TodoItem todo) { }
   }
   ```

3. **인증과 권한**:
   ```csharp
   [Authorize]
   public class TodosController { }
   ```

**장기 개선 (3-6개월):**

1. **마이크로서비스 아키텍처**:
   - Todo Service (CRUD)
   - Notification Service (알림)
   - Analytics Service (통계)
   - API Gateway (라우팅)

2. **이벤트 주도 아키텍처**:
   ```csharp
   // Domain Events
   public class TodoCompletedEvent
   {
       public int TodoId { get; set; }
       public DateTime CompletedAt { get; set; }
   }
   ```

3. **CQRS (Command Query Responsibility Segregation)**:
   ```csharp
   // Write Model
   public class CreateTodoCommand { }
   
   // Read Model  
   public class TodoListQuery { }
   ```

### 컴퓨터 과학 원리의 통합

이 프로젝트는 다음 CS 분야를 통합합니다:

- **데이터 구조**: List, 우선순위 큐 개념
- **알고리즘**: 정렬, 검색, 필터링
- **데이터베이스**: CRUD, 트랜잭션, 영속성
- **소프트웨어 공학**: 패턴, 아키텍처, 테스트
- **함수형 프로그래밍**: LINQ, 불변성, 순수 함수
- **운영체제**: 파일 시스템, 동시성
- **네트워크**: 미래 REST API 확장

### 실습 과제 - 깊이 있는 학습

**기초 과제 (Bloom's: Remember, Understand)**:

1. `TodoItem`에 `Tags` 속성(string 배열)을 추가하고 태그로 필터링하는 기능을 구현하세요
2. 완료된 할 일과 미완료 할 일을 별도 파일로 저장하는 아카이브 기능을 만드세요

**중급 과제 (Bloom's: Apply, Analyze)**:

3. 반복 작업 지원: `RecurrencePattern` enum (Daily, Weekly, Monthly)을 추가하고 자동 생성 로직을 구현하세요
4. 할 일 간 의존성: "할 일 A가 완료되어야 할 일 B를 시작할 수 있음"을 표현하는 `DependsOn` 관계를 구현하세요

**고급 과제 (Bloom's: Evaluate, Create)**:

5. 단위 테스트 작성: xUnit으로 `AddTodo()`, `CompleteTodo()` 등의 테스트 작성
6. 명령줄 인터페이스: `todo add "제목"`, `todo list --priority=3` 같은 CLI 구현
7. SQLite 데이터베이스로 전환: Entity Framework Core를 사용하여 파일 대신 DB 저장

**전문가 과제 (Bloom's: Create + Research)**:

8. 자연어 파싱: "내일까지 보고서 작성 우선순위 높음"을 파싱하여 TodoItem 생성
9. 포모도로 타이머 통합: 각 할 일에 소요 시간 추적 기능 추가
10. 웹 API와 Blazor UI: ASP.NET Core로 REST API를 만들고 Blazor로 SPA 구축

### 다음 단계 - 전문가로의 여정

**학습 로드맵:**

1. **Week 1-2**: 기초 과제 완료, LINQ 심화 학습
2. **Week 3-4**: 중급 과제, Entity Framework Core 학습
3. **Week 5-8**: 고급 과제, 웹 API 개발
4. **Week 9-12**: 전문가 과제, 마이크로서비스 아키텍처

**추천 도서:**

- **기초**: "C# in Depth" by Jon Skeet
- **패턴**: "Design Patterns" by GoF, "Patterns of Enterprise Application Architecture" by Martin Fowler
- **아키텍처**: "Clean Architecture" by Robert C. Martin, "Domain-Driven Design" by Eric Evans
- **고급**: "Functional Programming in C#" by Enrico Buonanno, "Concurrency in C# Cookbook" by Stephen Cleary

**오픈소스 프로젝트 분석:**

- **TodoMVC**: 다양한 프레임워크로 구현된 Todo 앱 비교
- **ASP.NET Core**: Microsoft의 웹 프레임워크 소스 코드
- **Entity Framework Core**: ORM 구현 연구

다음 장에서는 더 복잡한 텍스트 기반 게임을 만들어 보며, **상태 기계(State Machine)**, **전략 패턴(Strategy Pattern)**, **옵저버 패턴(Observer Pattern)** 등 고급 디자인 패턴을 실습할 것입니다.

**"프로그래밍은 단순히 코드를 작성하는 것이 아니라, 문제를 분석하고 해결책을 설계하며, 복잡성을 관리하는 예술이자 과학입니다." - Edsger W. Dijkstra**

여러분은 이제 단순한 **프로그래머(Programmer)**를 넘어 **소프트웨어 엔지니어(Software Engineer)**로 성장하는 중요한 단계를 넘었습니다. 작은 할 일 관리 앱이지만, 여기서 배운 원칙들은 대규모 엔터프라이즈 시스템에도 동일하게 적용됩니다.

계속해서 학습하고, 실험하고, 개선하세요. 그것이 마스터로 가는 유일한 길입니다.

---

**부록: 코드 품질 체크리스트**

배포 전 확인사항:

- [ ] 모든 public 메서드에 XML 주석 작성
- [ ] 예외 처리 완료 (try-catch)
- [ ] 입력 검증 (null, 범위 체크)
- [ ] 단위 테스트 작성 (최소 80% 커버리지)
- [ ] 코드 리뷰 완료
- [ ] 성능 프로파일링
- [ ] 보안 취약점 검사
- [ ] 문서화 (README, 사용자 가이드)

**"먼저 동작하게 만들고, 그 다음 올바르게 만들고, 마지막으로 빠르게 만들어라." - Kent Beck**

1. 할 일에 카테고리를 추가하고 카테고리별로 필터링하는 기능을 구현해보세요
2. 오늘의 할 일만 보여주는 "오늘의 할 일" 메뉴를 추가해보세요
3. 완료된 할 일을 별도 파일로 보관하는 아카이브 기능을 만들어보세요
4. 할 일을 수정하는 기능을 추가해보세요 (제목, 설명 변경)

이제 여러분은 실용적인 데이터 관리 애플리케이션을 만들 수 있습니다. 이 할 일 관리 앱은 여러분만의 도구로 사용하거나, 더 발전시켜 다른 사람들과 공유할 수도 있습니다!
