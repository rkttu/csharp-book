# 21장. 간단한 텍스트 기반 게임

텍스트 기반 게임은 **컴퓨터 게임의 원형(Archetype)**이자 **대화형 스토리텔링(Interactive Storytelling)**의 시작점입니다. 1970년대 초 Will Crowther의 Colossal Cave Adventure부터 시작하여, 1980년대 Infocom의 Zork 시리즈, 그리고 현대의 AI Dungeon에 이르기까지, 텍스트 기반 게임은 **상상력과 알고리즘의 만남**을 가장 순수하게 보여줍니다. 이 장에서는 단순히 게임을 만드는 것을 넘어서, **상태 기계(State Machine)**, **게임 루프(Game Loop)**, **엔티티 시스템(Entity System)** 등 모든 게임 프로그래밍의 핵심 패턴을 학습합니다.

**텍스트 게임의 역사적 맥락과 컴퓨터 과학에서의 위치:**

텍스트 기반 게임은 컴퓨터 게임 산업의 탄생과 함께 했습니다. 1962년 Steve Russell의 Spacewar!가 그래픽 게임의 시초라면, 1971년 Don Woods와 Will Crowther의 Adventure(Colossal Cave Adventure)는 텍스트 게임의 효시입니다. MIT의 Zork(1977-1979)는 자연어 처리(Natural Language Processing)의 초기 시도였으며, **파서(Parser)**라는 개념을 도입하여 "GET LAMP", "ATTACK TROLL WITH SWORD" 같은 복잡한 명령을 이해했습니다.

이러한 게임들은 단순한 엔터테인먼트가 아니라 **인공지능 연구의 실험장**이었습니다. Joseph Weizenbaum의 ELIZA(1966)처럼, 텍스트 게임은 인간-컴퓨터 상호작용(Human-Computer Interaction)의 가능성을 탐구했습니다. 현대의 ChatGPT와 같은 대화형 AI도 이러한 전통의 연장선에 있습니다.

**게임 프로그래밍의 보편적 패턴:**

텍스트 게임은 3D 그래픽이 없을 뿐, **모든 게임의 핵심 구조**를 그대로 가지고 있습니다:

1. **게임 루프(Game Loop)**: Glenn Fiedler의 "Fix Your Timestep" 에세이에서 설명한 것처럼, 모든 게임은 입력 처리 → 상태 업데이트 → 렌더링의 순환 구조를 가집니다. 우리의 텍스트 게임도 동일한 구조를 따릅니다.

2. **엔티티-컴포넌트 시스템(Entity-Component System, ECS)**: Unity, Unreal Engine이 사용하는 현대 게임 엔진의 핵심 아키텍처입니다. 플레이어와 적을 Character 기본 클래스로 추상화하는 것은 이 패턴의 단순화된 형태입니다.

3. **상태 기계(State Machine)**: 게임 캐릭터의 행동(idle, attacking, defending, dead)은 **유한 상태 기계(Finite State Machine, FSM)**로 모델링됩니다. 이는 컴파일러 설계, 네트워크 프로토콜, UI 플로우 등 컴퓨터 과학 전반에 적용되는 근본 개념입니다.

4. **이벤트 시스템(Event System)**: 전투 발생, 아이템 획득, 레벨업 등은 **이벤트 주도 프로그래밍(Event-Driven Programming)**의 예시입니다. 이는 Martin Fowler의 "Event Sourcing" 패턴과 연결됩니다.

5. **저장/불러오기 시스템**: 게임 상태의 직렬화(Serialization)는 **멤멘토 패턴(Memento Pattern)**의 구현이며, 실행 취소(Undo) 기능의 기반이 됩니다.

**RPG 장르의 게임 디자인 원칙:**

Role-Playing Game(RPG)은 Gary Gygax와 Dave Arneson의 Dungeons & Dragons(1974)에서 유래한 장르로, **수치적 진행(Numerical Progression)**과 **의사결정(Decision Making)**이 핵심입니다:

- **통계 시스템(Stats System)**: 체력(Health), 공격력(Attack), 방어력(Defense)은 **게임 밸런스(Game Balance)**의 기초입니다. Chris Crawford의 "The Art of Computer Game Design"에서 강조하는 것처럼, 이러한 수치는 플레이어의 선택에 의미를 부여합니다.

- **진행 시스템(Progression System)**: 경험치와 레벨업은 **스키너 상자(Skinner Box)**의 게임화(Gamification)입니다. B.F. Skinner의 조작적 조건화(Operant Conditioning) 이론을 게임에 적용한 것으로, 플레이어의 동기를 유지합니다.

- **리스크-리워드(Risk-Reward)**: 던전 탐험, 전투 선택, 아이템 사용은 모두 **의사결정 이론(Decision Theory)**의 실천입니다. 플레이어는 불확실성 속에서 기대값을 계산하며 최적의 선택을 추구합니다.

**객체지향 프로그래밍의 완성:**

이 프로젝트는 9-11장에서 학습한 객체지향 프로그래밍(OOP)의 모든 개념을 통합합니다:

1. **상속(Inheritance)**: Character → Player/Enemy의 계층 구조는 **is-a 관계**를 표현합니다. Liskov Substitution Principle을 준수하여, Enemy가 필요한 곳에 Player를 대체할 수 있습니다(비록 게임 로직상 그럴 일은 없지만, 타입 시스템 관점에서 가능합니다).

2. **캡슐화(Encapsulation)**: 내부 상태(Health, Attack)를 private로 숨기고 public 메서드(TakeDamage, Heal)로만 접근하는 것은 **정보 은닉(Information Hiding)**의 실천입니다. David Parnas의 1972년 논문 "On the Criteria To Be Used in Decomposing Systems into Modules"의 핵심 원칙입니다.

3. **다형성(Polymorphism)**: 같은 TakeDamage 메서드를 Player와 Enemy가 다르게 구현할 수 있습니다. 이는 **인터페이스 프로그래밍(Programming to Interface)**의 기초입니다.

4. **추상화(Abstraction)**: Item 클래스는 포션, 무기, 방어구라는 다양한 구체적 아이템을 하나의 개념으로 통합합니다. 이는 **본질적 복잡성(Essential Complexity)**만 남기고 **우발적 복잡성(Accidental Complexity)**을 제거하는 과정입니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 게임 프로그래밍의 전체 생명주기를 경험하며 다음을 습득하게 됩니다:

- **게임 시스템 아키텍처**: Robert Nystrom의 "Game Programming Patterns"에서 제시한 게임 루프(Game Loop), 업데이트 메서드(Update Method), 상태 패턴(State Pattern) 등의 고전적 게임 프로그래밍 패턴을 학습합니다. 이는 Unity, Unreal Engine 같은 현대 게임 엔진의 기반이 되는 구조입니다.

- **엔티티 시스템 설계**: 게임 객체(플레이어, 적, 아이템)를 클래스로 모델링하는 방법을 배웁니다. 이는 Bob Nystrom의 Entity-Component-System(ECS) 아키텍처의 단순화된 형태이며, 확장성과 재사용성을 고려한 설계의 기초입니다.

- **턴 기반 전투 시스템**: **순차적 의사결정(Sequential Decision Making)**의 구현입니다. 이는 체스, 바둑 같은 보드 게임의 AI 개발에도 사용되는 **게임 이론(Game Theory)**의 실천입니다. John von Neumann과 Oskar Morgenstern의 "Theory of Games and Economic Behavior"에서 시작된 이론의 실용적 적용입니다.

- **난수 생성과 확률**: Random 클래스를 통한 **의사 난수 생성(Pseudo-Random Number Generation, PRNG)**의 이해와 활용을 학습합니다. Donald Knuth의 "The Art of Computer Programming, Volume 2"에서 설명한 선형 합동 생성기(Linear Congruential Generator)의 원리를 이해하고, 게임 밸런스를 위한 확률 설계를 경험합니다.

- **상태 영속성(State Persistence)**: 게임 저장/불러오기를 통해 **직렬화(Serialization)**의 실전 활용을 배웁니다. 이는 20장의 할 일 관리 앱에서 학습한 JSON 직렬화를 게임이라는 더 복잡한 도메인에 적용하는 것입니다. Gang of Four의 **멤멘토 패턴(Memento Pattern)**의 구현이기도 합니다.

- **게임 밸런스와 수치 설계**: 플레이어 레벨에 따른 적의 강도 조절, 경험치 곡선, 아이템 드롭률 등은 **게임 이코노미(Game Economy)** 설계의 기초입니다. 이는 수학적 모델링과 플레이테스트를 통한 반복적 조정이 필요한 영역입니다.

**프로젝트 기반 학습의 심화:**

19장의 계산기, 20장의 할 일 관리 앱을 거쳐 이제 게임이라는 **복합 시스템(Complex System)**을 구축합니다. 이는 Herbert Simon의 "The Sciences of the Artificial"에서 설명한 **계층적 시스템(Hierarchical System)**의 실례입니다:

```
게임 (Game)
  ↓
게임 루프 (Game Loop)
  ↓
전투 시스템 (Combat System) + 인벤토리 시스템 (Inventory System) + 진행 시스템 (Progression System)
  ↓
캐릭터 (Character) + 아이템 (Item) + 이벤트 (Event)
  ↓
기본 데이터 타입 (Primitive Data Types)
```

각 계층은 하위 계층의 복잡성을 추상화하여, **관심사의 분리(Separation of Concerns)**를 달성합니다.

---

## 21.1 게임 설계

게임 설계(Game Design)는 단순히 프로그래밍 이전의 계획 단계가 아니라, **창의성과 시스템적 사고의 융합**입니다. Sid Meier는 "게임은 흥미로운 선택의 연속"이라고 정의했고, Jesse Schell은 "The Art of Game Design"에서 게임을 렌즈(Lens)라는 다면적 관점으로 분석했습니다. 우리의 텍스트 RPG는 간단하지만, 모든 게임 디자인의 핵심 요소를 포함합니다.

**게임 설계의 이론적 기반:**

게임 설계는 여러 학문의 교차점에 있습니다:

1. **시스템 이론(Systems Theory)**: Ludwig von Bertalanffy의 일반 시스템 이론에 따르면, 게임은 **상호작용하는 요소들의 복합체**입니다. 플레이어, 적, 아이템, 환경이 각각의 규칙에 따라 상호작용하며 **창발적 행동(Emergent Behavior)**을 만듭니다.

2. **심리학(Psychology)**: Mihaly Csikszentmihalyi의 **몰입 이론(Flow Theory)**은 게임의 난이도 곡선 설계에 직접 적용됩니다. 플레이어의 능력과 도전 과제의 균형이 몰입을 만들며, 이는 레벨 디자인과 적 난이도 조절의 이론적 근거입니다.

3. **경제학(Economics)**: 게임 내 자원(골드, 포션, 장비)의 획득과 소비는 **가상 경제(Virtual Economy)**를 형성합니다. 이는 미시경제학의 수요-공급 법칙이 적용되는 폐쇄 시스템입니다.

4. **내러티브 이론(Narrative Theory)**: Janet Murray의 "Hamlet on the Holodeck"에서 제시한 **절차적 저작(Procedural Authorship)** 개념은 게임의 스토리가 플레이어의 선택에 의해 생성됨을 설명합니다.

### 게임 개요와 설계 목표

**MDA 프레임워크(Mechanics-Dynamics-Aesthetics Framework):**

Robin Hunicke, Marc LeBlanc, Robert Zubek이 제안한 MDA 프레임워크로 우리 게임을 분석합니다:

**1. 메커닉스(Mechanics)** - 게임의 규칙:
- 턴 기반 전투: 플레이어와 적이 번갈아 행동
- 체력 시스템: 체력이 0이 되면 사망
- 경험치와 레벨업: 적을 물리치면 강해짐
- 아이템 사용: 포션으로 회복, 장비로 능력 증가

**2. 다이나믹스(Dynamics)** - 플레이 중 발생하는 행동:
- 리스크 관리: 언제 회복하고 언제 공격할지 결정
- 자원 관리: 제한된 포션을 전략적으로 사용
- 진행 감각: 레벨업을 통한 캐릭터 성장 경험

**3. 에스테틱스(Aesthetics)** - 플레이어가 느끼는 감정:
- **도전(Challenge)**: 강한 적과의 전투
- **발견(Discovery)**: 던전 탐험과 아이템 발견
- **환상(Fantasy)**: 용감한 모험가 역할 체험
- **표현(Expression)**: 전투 스타일 선택

**게임 디자인 문서(Game Design Document, GDD):**

실제 게임 개발에서 사용하는 GDD의 축소판을 작성합니다:

```markdown
=== 텍스트 RPG 게임 디자인 문서 ===

[게임 개요]
- 제목: 던전 탐험가
- 장르: 텍스트 기반 RPG
- 타겟: C# 학습자
- 플랫폼: 콘솔 (.NET)
- 핵심 경험: 캐릭터 성장과 전투의 긴장감

[게임 플로우]
시작 → 캐릭터 생성 → 메인 루프(탐험/전투/관리) → 저장/종료

[핵심 메커닉스]
1. 턴 기반 전투
2. 레벨업 시스템
3. 인벤토리 관리
4. 랜덤 인카운터

[밸런스 설계]
- 초기 플레이어: HP 100, 공격 10, 방어 5
- 레벨당 성장: HP +20, 공격 +5, 방어 +3
- 적 난이도: 플레이어 레벨 기반 스케일링
```

### 핵심 구성 요소와 시스템 설계

**1. 캐릭터 시스템(Character System)** - 엔티티 설계의 기초

**속성 기반 시스템(Attribute-Based System):**

RPG의 스탯 시스템은 1974년 D&D에서 확립된 전통을 따릅니다. 우리의 단순화된 시스템:

- **체력(Health Points, HP)**: 생존의 척도. 0이 되면 사망. John Carmack의 Doom(1993)이 현대적 HP 시스템을 정립했습니다.
  - **이론**: 체력은 **버퍼(Buffer)** 역할을 하여 플레이어에게 실수 여지를 줍니다. 이는 UX 디자인의 "관용의 원칙(Principle of Forgiveness)"입니다.
  - **최대 체력**: 캐릭터의 강인함을 표현하며, 레벨업의 가시적 보상입니다.

- **공격력(Attack Power)**: 데미지 출력의 기준값.
  - **이론**: 선형 증가 vs 지수 증가의 선택이 게임 밸런스를 결정합니다. 우리는 선형 증가(+5/레벨)를 선택하여 예측 가능성을 높입니다.
  - **확률 요소**: 나중에 크리티컬 히트(치명타) 같은 확률적 변수를 추가할 수 있습니다.

- **방어력(Defense)**: 피해 감소.
  - **이론**: 방어력은 **감쇠 함수(Attenuation Function)**로 작동합니다. 우리의 단순 모델: `실제 데미지 = Max(1, 공격력 - 방어력)`. 
  - **최소 데미지 보장**: 아무리 방어력이 높아도 최소 1의 데미지는 입도록 하여 무한 교착 상태를 방지합니다.

**레벨링 시스템(Leveling System):**

경험치 기반 진행은 **조작적 조건화(Operant Conditioning)**의 고전적 예시입니다:

```csharp
// 경험치 공식 설계
ExperienceToNextLevel = Level * 100  // 선형 증가
// 대안: Level^2 * 50  // 지수 증가 (후반 성장 둔화)
```

**설계 고려사항:**
- **선형 공식**: 일정한 속도로 레벨업, 예측 가능
- **지수 공식**: 초반 빠르게, 후반 느리게, MMORPG에서 흔함
- **우리의 선택**: 단순성을 위해 선형 채택

**2. 전투 시스템(Combat System)** - 게임의 핵심 루프

**턴 기반 vs 실시간:**

- **턴 기반(Turn-Based)**: XCOM, Civilization의 전통. **이산 시간(Discrete Time)** 모델로, 각 턴은 원자적 단위입니다.
  - 장점: 전략적 사고 시간 제공, 구현 단순
  - 단점: 긴장감 부족 가능성
  
- **실시간(Real-Time)**: Diablo, Dark Souls. **연속 시간(Continuous Time)** 모델.
  - 장점: 액션과 긴장감
  - 단점: 텍스트 기반 구현 복잡

**우리의 선택**: 턴 기반 - 콘솔 I/O에 적합하고 학습 목적에 부합

**전투 흐름의 상태 기계(Finite State Machine):**

```
[전투 시작] → [플레이어 턴] → [행동 선택] → [행동 실행] → [적 생존 체크]
                    ↑                                              ↓
                    ←───────[적 턴]←───────[플레이어 생존 체크]←─┘
                                              ↓
                                          [전투 종료]
```

이는 **Moore Machine**(출력이 상태에만 의존)의 예시입니다. 컴파일러 설계, 네트워크 프로토콜에서 사용하는 동일한 개념입니다.

**행동 선택(Action Selection):**

플레이어의 선택지는 **의사결정 트리(Decision Tree)**를 형성합니다:

```
[행동 선택]
├─ 공격 (Attack): 높은 데미지, 방어 없음
├─ 방어 (Defend): 데미지 50% 감소, 다음 턴 유리
├─ 아이템 (Item): 즉시 효과, 자원 소모
└─ 도망 (Flee): 50% 확률, 보상 없음
```

각 선택은 **트레이드오프(Trade-off)**를 가지며, 이는 게임 이론의 **내시 균형(Nash Equilibrium)** 개념과 연결됩니다.

**3. 아이템 시스템(Item System)** - 전략적 깊이 추가

**아이템 분류학(Item Taxonomy):**

```
아이템 (Item)
├─ 소비 아이템 (Consumable)
│  ├─ 포션 (Potion): 즉시 HP 회복
│  └─ 버프 아이템 (Buff Item): 임시 능력 증가
├─ 장비 (Equipment)
│  ├─ 무기 (Weapon): 공격력 증가
│  └─ 방어구 (Armor): 방어력 증가
└─ 특수 아이템 (Special Item): 퀘스트/스토리 관련
```

**Diablo의 아이템 시스템**이 정립한 현대 RPG 아이템 디자인의 기본 구조를 따릅니다.

**확률 기반 보상(Probabilistic Rewards):**

```csharp
// 드롭률 설계
if (Random.Next(100) < 30)  // 30% 확률
{
    Item reward = GenerateRandomItem();
}
```

이는 **변동 비율 강화(Variable Ratio Reinforcement)**로, 슬롯머신과 같은 원리로 플레이어의 동기를 유지합니다. 심리학자 B.F. Skinner의 실험이 입증한 가장 강력한 행동 강화 방식입니다.

**인벤토리(Inventory) 설계:**

- **무제한 인벤토리**: 단순, 관리 부담 없음 (우리의 선택)
- **제한된 인벤토리**: Resident Evil 스타일, 자원 관리 추가
- **무게 시스템**: The Elder Scrolls, 현실성 증가

**4. 진행 시스템(Progression System)** - 플레이어 여정

**던전 탐험의 확률 모델:**

```csharp
// 인카운터 확률 분포
int encounter = Random.Next(100);
if (encounter < 60)        // 60% 전투
else if (encounter < 80)   // 20% 아이템 발견
else                       // 20% 안전 지대
```

이는 **마르코프 체인(Markov Chain)**의 단순화된 형태입니다. 각 탐험은 이전 상태와 무관한 독립 시행입니다.

**게임 저장/불러오기:**

- **세이브 포인트(Save Point)**: 특정 위치에서만 저장, 긴장감 유지
- **자동 저장(Auto-Save)**: 현대 게임의 표준, 사용자 편의성
- **우리의 선택**: 언제든 수동 저장 가능, 학습 편의성 우선

### 게임 흐름도와 상태 전이

**고수준 게임 플로우:**

```
[프로그램 시작]
    ↓
[타이틀 화면] ───→ 게임 소개, 분위기 조성
    ↓
[메인 메뉴]
    ├─ 새 게임 ───→ [캐릭터 생성] ───→ [게임 세계 진입]
    ├─ 불러오기 ──→ [저장 데이터 로드] ───→ [게임 세계 진입]
    └─ 종료 ────→ [프로그램 종료]
    
[게임 세계 진입]
    ↓
[메인 게임 루프] ←──────────────┐
    ├─ 던전 탐험 ─→ [랜덤 이벤트] │
    │   ├─ 전투 ──→ [전투 시스템] │
    │   ├─ 보물 ──→ [아이템 획득] │
    │   └─ 안전 ──→ [휴식/회복]  │
    ├─ 상태 확인 ─→ [캐릭터 정보] │
    ├─ 인벤토리 ──→ [아이템 관리] │
    ├─ 장비 관리 ──→ [장착/해제]  │
    ├─ 저장 ─────→ [게임 저장]   │
    └─ 종료 ─────→ [저장 확인] ─→ [프로그램 종료]
         ↑                          
         └──────────────────────────┘
```

**전투 시스템의 상세 플로우:**

```
[전투 시작]
    ↓
[적 정보 표시] ───→ 적의 이름, 체력, 레벨
    ↓
[플레이어 턴] ←────────────────┐
    ↓                           │
[행동 메뉴 표시]                │
    ├─ 1. 공격                  │
    ├─ 2. 방어                  │
    ├─ 3. 아이템 사용           │
    └─ 4. 도망치기              │
    ↓                           │
[행동 선택 입력]                │
    ↓                           │
[행동 처리]                     │
    ├─ 공격 → [데미지 계산] → [적 HP 감소]
    ├─ 방어 → [방어 상태 설정]
    ├─ 아이템 → [아이템 효과 적용]
    └─ 도망 → [도망 판정 50%]
         ├─ 성공 → [전투 종료]
         └─ 실패 → [계속]
    ↓
[적 생존 체크]
    ├─ 사망 → [승리 처리] → [경험치/골드 획득] → [전투 종료]
    └─ 생존 ↓
    ↓
[적 턴]
    ↓
[적 AI 행동 결정] ───→ 단순 AI: 항상 공격
    ↓
[적 공격 실행] ───→ [플레이어 HP 감소]
    ↓
[플레이어 생존 체크]
    ├─ 사망 → [패배 처리] → [게임 오버]
    └─ 생존 ──────────────────┘
```

**상태 기계 다이어그램(State Machine Diagram):**

게임의 주요 상태를 UML 상태 다이어그램으로 표현합니다:

```
┌───────────────┐
│  메인 메뉴    │◄─────┐
└───────────────┘      │
        │              │
    [새 게임]          │
        ↓              │
┌───────────────┐      │
│   탐험 중     │◄──┐  │
└───────────────┘   │  │
        │           │  │
    [전투 조우]      │  │
        ↓           │  │
┌───────────────┐   │  │
│   전투 중     │   │  │
└───────────────┘   │  │
    │       │       │  │
[승리]  [패배]     │  │
    │       │       │  │
    │       └─→[게임 오버]
    │               │
    └───────────────┘
```

### 클래스 설계와 객체지향 아키텍처

**UML 클래스 다이어그램:**

```
┌─────────────────────┐
│     Character       │ ◄─── 추상 기본 클래스
├─────────────────────┤
│ + Name: string      │
│ + Health: int       │
│ + MaxHealth: int    │
│ + Attack: int       │
│ + Defense: int      │
│ + IsAlive: bool     │
├─────────────────────┤
│ + TakeDamage(int)   │
│ + ShowStatus()      │
└─────────────────────┘
         △
         │
    ┌────┴────┐
    │         │
┌───────┐ ┌────────┐
│Player │ │ Enemy  │
├───────┤ ├────────┤
│+Level │ │+ExpRwd │
│+Exp   │ │+GoldRwd│
│+Gold  │ └────────┘
│+Inven │
├───────┤
│+GainEx│
│+LevelU│
│+Heal  │
└───────┘

┌─────────────────────┐
│        Item         │
├─────────────────────┤
│ + Name: string      │
│ + Type: string      │
│ + Value: int        │
│ + Description: str  │
├─────────────────────┤
│ + Use(Player)       │
└─────────────────────┘

┌─────────────────────┐
│        Game         │ ◄─── 게임 관리자 (Singleton)
├─────────────────────┤
│ - player: Player    │
│ - isRunning: bool   │
├─────────────────────┤
│ + Start()           │
│ + GameLoop()        │
│ + StartBattle()     │
│ + Explore()         │
│ + SaveGame()        │
│ + LoadGame()        │
└─────────────────────┘
```

**설계 패턴의 적용:**

1. **상속 계층(Inheritance Hierarchy)**:
   ```csharp
   Character (기본 클래스)
     ↓
   ┌─────────┬──────────┐
   │         │          │
   Player  Enemy    (미래) NPC
   ```
   - **Liskov Substitution Principle**: Character 타입이 필요한 곳에 Player나 Enemy를 대체 가능
   - **Open-Closed Principle**: 새로운 캐릭터 타입(NPC, Boss) 추가 시 기존 코드 수정 없이 확장

2. **캡슐화(Encapsulation)**:
   ```csharp
   class Character
   {
       private int health;  // 외부 직접 접근 금지
       
       public int Health 
       { 
           get => health; 
           protected set => health = Math.Max(0, Math.Min(value, MaxHealth));
       }
       
       public void TakeDamage(int damage)  // 통제된 인터페이스
       {
           Health -= damage;
           // 이벤트 발생, 로깅 등 추가 로직 가능
       }
   }
   ```
   - **정보 은닉(Information Hiding)**: 내부 상태를 private로 숨김
   - **불변조건(Invariant) 유지**: Health는 항상 0 ~ MaxHealth 범위 내

3. **싱글톤 패턴(Singleton Pattern)**:
   ```csharp
   class Game
   {
       private static Game instance;
       public static Game Instance => instance ??= new Game();
       
       private Game() { }  // private 생성자
   }
   ```
   - **전역 접근점**: 어디서든 Game.Instance로 접근
   - **단일 인스턴스 보장**: 게임 상태의 일관성 유지

4. **팩토리 메서드(Factory Method)**:
   ```csharp
   class Enemy
   {
       public static Enemy CreateRandom(int playerLevel)
       {
           // 플레이어 레벨에 맞는 적 생성
           // 복잡한 생성 로직 캡슐화
       }
   }
   ```
   - **생성 로직 캡슐화**: 클라이언트 코드는 생성 세부사항을 몰라도 됨
   - **유연성**: 난이도, 랜덤 시드 등에 따라 다양한 적 생성 가능

### 데이터 구조 선택과 성능 고려사항

**컬렉션 사용 분석:**

```csharp
class Player
{
    public List<Item> Inventory { get; set; }  // 인벤토리
    // Dictionary<string, Item>도 고려 가능
}

class Game
{
    private List<Enemy> enemies;  // 던전의 적들 (미래 확장)
    // Queue<Event>로 이벤트 관리도 가능
}
```

**List<T> vs Dictionary<TKey, TValue> vs Queue<T>:**

| 자료구조 | 추가 | 검색 | 삭제 | 사용 사례 |
|---------|------|------|------|----------|
| List<Item> | O(1)* | O(n) | O(n) | 순서가 중요한 인벤토리 |
| Dictionary<string, Item> | O(1) | O(1) | O(1) | 이름으로 빠른 조회 필요 시 |
| Queue<Event> | O(1) | - | O(1) | 이벤트 순차 처리 |

*amortized complexity

**메모리 효율성:**

```csharp
// 플레이어 객체 메모리 사용량 추정 (64비트 시스템)
class Player : Character
{
    // Character 부분
    string Name;            // 8 bytes (참조) + 문자열 객체
    int Health;             // 4 bytes
    int MaxHealth;          // 4 bytes
    int Attack;             // 4 bytes
    int Defense;            // 4 bytes
    
    // Player 추가 부분
    int Level;              // 4 bytes
    int Experience;         // 4 bytes
    int Gold;               // 4 bytes
    List<Item> Inventory;   // 8 bytes (참조) + List 객체
    Item EquippedWeapon;    // 8 bytes (참조)
    Item EquippedArmor;     // 8 bytes (참조)
    
    // 총 약 60 bytes + 동적 할당 부분
}
```

인벤토리에 100개 아이템이 있어도 수 KB 수준으로, 콘솔 게임에서는 메모리 최적화가 불필요합니다. 하지만 이러한 분석은 모바일이나 대규모 게임에서 필수적입니다.

### 게임 밸런스 설계와 수학적 모델

**경험치 곡선(Experience Curve):**

```csharp
// 선형 모델 (우리의 선택)
int ExpToNextLevel = Level * 100;

// 지수 모델 (레벨 상한 명확)
int ExpToNextLevel = (int)Math.Pow(Level, 2) * 50;

// 혼합 모델 (초반 빠름, 후반 느림)
int ExpToNextLevel = Level * 100 + (Level * Level) * 10;
```

**전투 밸런스 시뮬레이션:**

게임 디자이너는 수학적 모델로 밸런스를 예측합니다:

```
플레이어 레벨 1: HP 100, 공격 10, 방어 5
적 레벨 1: HP 40, 공격 7, 방어 2

전투 시뮬레이션:
- 플레이어 데미지: 10 - 2 = 8
- 적 데미지: 7 - 5 = 2
- 적 처치 턴: 40 / 8 = 5턴
- 플레이어 피해: 2 * 5 = 10 HP
- 생존 여유: (100 - 10) / 100 = 90%

결론: 적절한 난이도, 플레이어 유리
```

이러한 시뮬레이션은 **Monte Carlo 방법**으로 확장 가능하며, 수천 번의 가상 전투를 통해 승률과 평균 생존 HP를 계산합니다.

### 필요한 클래스 구조와 구현 로드맵

**MVP (Minimum Viable Product) 접근:**

Eric Ries의 Lean Startup 방법론을 게임 개발에 적용합니다:

**Phase 1: 핵심 전투 시스템**
```csharp
// 기본 캐릭터 클래스
class Character
{
    public string Name { get; set; }
    public int Health { get; set; }
    public int MaxHealth { get; set; }
    public int Attack { get; set; }
    public int Defense { get; set; }
    public bool IsAlive => Health > 0;
    
    public void TakeDamage(int damage) 
    { 
        /* 구현 */ 
    }
}
```

**Phase 2: 플레이어 진행 시스템**
```csharp
// 플레이어 클래스 확장
class Player : Character
{
    public int Level { get; set; }
    public int Experience { get; set; }
    public void GainExperience(int exp) { /* 레벨업 로직 */ }
}
```

**Phase 3: 아이템과 인벤토리**
```csharp
class Item { /* 아이템 정의 */ }
class Player
{
    public List<Item> Inventory { get; set; }
    public void UseItem(Item item) { /* 사용 로직 */ }
}
```

**Phase 4: 저장/불러오기**
```csharp
class GameSave { /* 직렬화 가능한 데이터 */ }
void SaveGame() { /* JSON 직렬화 */ }
void LoadGame() { /* JSON 역직렬화 */ }
```

**Phase 5: 폴리싱(Polishing)**
- UI 개선 (색상, 아스키 아트)
- 밸런스 조정
- 버그 수정
- 추가 콘텐츠 (더 많은 적, 아이템, 던전)

각 Phase는 독립적으로 테스트 가능한 **증분(Increment)**이며, 애자일 개발의 **스프린트(Sprint)**에 해당합니다.

**코드 구조와 파일 조직:**

```
Game Project
│
├── Program.cs          // 메인 진입점
├── Character.cs        // 캐릭터 기본 클래스
├── Player.cs           // 플레이어 클래스
├── Enemy.cs            // 적 클래스
├── Item.cs             // 아이템 클래스
├── Game.cs             // 게임 관리자
├── Combat.cs           // 전투 시스템 (선택사항)
├── Inventory.cs        // 인벤토리 관리 (선택사항)
└── GameSave.cs         // 저장/불러오기
```

**의존성 다이어그램:**

```
Program.cs
    ↓
  Game.cs
    ↓
  ┌─┴─┬────┬────┐
  │   │    │    │
Player Enemy Item GameSave
  │
Character
```

명확한 의존성 방향은 **순환 의존성(Circular Dependency)**을 방지하며, 코드의 이해와 테스트를 용이하게 합니다.

---

## 21.2 플레이어와 적 클래스 구현

캐릭터 클래스의 설계는 **엔티티 모델링(Entity Modeling)**의 핵심입니다. 이는 Eric Evans의 Domain-Driven Design에서 제시한 **엔티티(Entity)** 개념의 실천이며, Martin Fowler의 "Analysis Patterns"에서 설명한 **도메인 객체(Domain Object)** 패턴의 적용입니다. 게임 엔티티는 단순한 데이터 구조가 아니라 **행동(Behavior)**과 **상태(State)**를 모두 가진 능동적 객체입니다.

**객체지향 설계의 핵심 원칙 적용:**

1. **단일 책임 원칙(Single Responsibility Principle, SRP)**: Character 클래스는 캐릭터의 상태와 생명주기 관리에만 집중합니다. 렌더링, 입력 처리, 저장 등은 다른 클래스의 책임입니다.

2. **개방-폐쇄 원칙(Open-Closed Principle, OCP)**: Character를 상속하여 Player, Enemy, (미래의) Boss, NPC 등을 추가할 수 있지만, Character 자체는 수정할 필요가 없습니다.

3. **리스코프 치환 원칙(Liskov Substitution Principle, LSP)**: Character 타입이 필요한 곳에 Player나 Enemy를 대체해도 프로그램이 정상 작동해야 합니다. 이는 다형성의 근간입니다.

4. **인터페이스 분리 원칙(Interface Segregation Principle, ISP)**: 클라이언트는 사용하지 않는 메서드에 의존하지 않습니다. IDamageable, IHealable 같은 인터페이스로 분리 가능합니다.

5. **의존성 역전 원칙(Dependency Inversion Principle, DIP)**: 구체적 구현(Player, Enemy)이 아닌 추상화(Character, ICharacter)에 의존합니다.

### Character 기본 클래스 - 추상화의 기초

**클래스 설계의 이론적 기반:**

Character 클래스는 **추상 데이터 타입(Abstract Data Type, ADT)**의 구현입니다. Barbara Liskov가 1974년 제안한 이 개념은 데이터와 연산을 하나의 단위로 묶어 **캡슐화**를 달성합니다.

```csharp
/// <summary>
/// 모든 게임 캐릭터의 기본 클래스입니다.
/// GoF의 Template Method 패턴을 적용하여 공통 행동을 정의합니다.
/// </summary>
class Character
{
    // === 필드(Fields) vs 속성(Properties) ===
    // 필드는 실제 데이터 저장소, 속성은 접근자 메서드
    
    /// <summary>
    /// 캐릭터의 이름. 불변(Immutable)하게 만들려면 { get; init; } 사용
    /// </summary>
    public string Name { get; set; }
    
    /// <summary>
    /// 현재 체력. 0이 되면 사망 상태
    /// 불변조건(Invariant): 0 <= Health <= MaxHealth
    /// </summary>
    public int Health { get; set; }
    
    /// <summary>
    /// 최대 체력. 레벨업 시 증가
    /// </summary>
    public int MaxHealth { get; set; }
    
    /// <summary>
    /// 공격력. 데미지 계산의 기준값
    /// </summary>
    public int Attack { get; set; }
    
    /// <summary>
    /// 방어력. 받는 데미지 감소
    /// </summary>
    public int Defense { get; set; }
    
    /// <summary>
    /// 생존 여부를 확인하는 계산된 속성(Computed Property)
    /// Expression-Bodied Member (C# 6.0) 사용
    /// </summary>
    public bool IsAlive => Health > 0;
    
    /// <summary>
    /// 체력 비율 (0.0 ~ 1.0). UI 헬스바에 유용
    /// </summary>
    public double HealthPercentage => MaxHealth > 0 ? (double)Health / MaxHealth : 0;
    
    /// <summary>
    /// 데미지를 받는 메서드. 방어력을 고려한 실제 데미지 계산
    /// </summary>
    /// <param name="damage">받을 기본 데미지</param>
    /// <returns>실제로 받은 데미지</returns>
    public int TakeDamage(int damage)
    {
        // === 데미지 계산 공식 ===
        // 다양한 게임의 데미지 공식 비교:
        // 1. 선형 감소: actualDamage = damage - defense (우리의 선택)
        // 2. 백분율 감소: actualDamage = damage * (1 - defense / 100)
        // 3. 복합: actualDamage = damage * (100 / (100 + defense))
        
        int actualDamage = Math.Max(1, damage - Defense);
        // Math.Max(1, ...)로 최소 1 데미지 보장
        // 이는 무한 교착 상태(Deadlock) 방지
        
        Health -= actualDamage;
        
        // 불변조건 유지: Health가 음수가 되지 않도록
        if (Health < 0) Health = 0;
        
        // === 피드백(Feedback) 제공 ===
        // Donald Norman의 "Design of Everyday Things"에서 강조한
        // 즉각적 피드백의 중요성
        Console.WriteLine($"{Name}이(가) {actualDamage}의 피해를 입었습니다! " +
                         $"(남은 체력: {Health}/{MaxHealth})");
        
        // === 이벤트 시스템 확장 가능성 ===
        // OnDamaged?.Invoke(this, actualDamage);
        // 옵저버 패턴(Observer Pattern) 적용 시
        
        return actualDamage;
    }
    
    /// <summary>
    /// 캐릭터의 현재 상태를 표시합니다.
    /// Template Method 패턴의 기본 구현
    /// </summary>
    public virtual void ShowStatus()
    {
        Console.WriteLine($"\n=== {Name}의 상태 ===");
        Console.WriteLine($"체력: {Health}/{MaxHealth} [{HealthPercentage:P0}]");
        Console.WriteLine($"공격력: {Attack}");
        Console.WriteLine($"방어력: {Defense}");
        
        // === 상태 시각화 ===
        // 헬스바 표시 (선택사항)
        ShowHealthBar();
    }
    
    /// <summary>
    /// 텍스트 기반 헬스바 표시
    /// ASCII 아트를 통한 시각적 피드백
    /// </summary>
    protected void ShowHealthBar()
    {
        int barLength = 20;
        int filledLength = (int)(barLength * HealthPercentage);
        
        Console.Write("체력: [");
        Console.Write(new string('█', filledLength));
        Console.Write(new string('░', barLength - filledLength));
        Console.WriteLine("]");
    }
    
    /// <summary>
    /// 가상 메서드: 파생 클래스에서 오버라이드 가능
    /// 캐릭터가 사망했을 때 호출
    /// </summary>
    public virtual void OnDeath()
    {
        Console.WriteLine($"{Name}이(가) 쓰러졌습니다...");
    }
}
```

**코드 설계 결정 사항 분석:**

1. **속성 vs 필드**:
   ```csharp
   // 필드 (Field)
   private int health;  // 직접 접근 불가, 캡슐화
   
   // 자동 속성 (Auto-Property)
   public int Health { get; set; }  // 간결, C# 3.0+
   
   // 완전 속성 (Full Property)
   public int Health 
   { 
       get => health; 
       set => health = Math.Clamp(value, 0, MaxHealth);
   }
   ```
   우리는 단순성을 위해 자동 속성을 선택했지만, 더 엄격한 검증이 필요하면 완전 속성으로 전환할 수 있습니다.

2. **Expression-Bodied Members (C# 6.0)**:
   ```csharp
   // 전통적 방식
   public bool IsAlive 
   { 
       get { return Health > 0; } 
   }
   
   // 간결한 방식
   public bool IsAlive => Health > 0;
   ```
   람다식 문법으로 읽기 전용 계산된 속성을 간결하게 표현합니다.

3. **최소 데미지 보장**:
   ```csharp
   Math.Max(1, damage - Defense)
   ```
   이는 **게임 디자인 원칙**입니다. 아무리 방어력이 높아도 데미지를 완전히 무효화할 수 없어야 합니다. 그렇지 않으면:
   - 무한 전투 가능성
   - 밸런스 붕괴 (무적 빌드)
   - 플레이어 좌절감

### Player 클래스 - 진행 시스템의 구현

Player 클래스는 Character를 상속하여 **플레이어 고유의 진행 메커닉스**를 추가합니다. 이는 RPG의 핵심인 **캐릭터 성장(Character Progression)**을 구현합니다.

```csharp
/// <summary>
/// 플레이어 캐릭터. 경험치, 레벨업, 인벤토리 시스템 포함
/// </summary>
class Player : Character
{
    // === 진행 시스템 속성 ===
    
    /// <summary>
    /// 현재 레벨. 1부터 시작
    /// </summary>
    public int Level { get; set; }
    
    /// <summary>
    /// 현재 경험치. 일정량 달성 시 레벨업
    /// </summary>
    public int Experience { get; set; }
    
    /// <summary>
    /// 다음 레벨까지 필요한 경험치
    /// 설계 선택: 선형 vs 지수 vs 로그 곡선
    /// </summary>
    public int ExperienceToNextLevel => Level * 100;  // 선형 공식
    
    // 대안적 공식들:
    // Level * Level * 50;          // 지수 증가 (레벨업 점점 어려움)
    // (int)(100 * Math.Pow(1.1, Level));  // 기하급수 증가
    // 100 + (Level - 1) * 20;      // 선형이지만 초기값 100
    
    /// <summary>
    /// 레벨업 진행도 (0.0 ~ 1.0)
    /// UI 경험치 바에 유용
    /// </summary>
    public double ExperienceProgress => 
        ExperienceToNextLevel > 0 ? (double)Experience / ExperienceToNextLevel : 1.0;
    
    // === 인벤토리 시스템 ===
    
    /// <summary>
    /// 소지 아이템 목록
    /// List vs Dictionary vs Queue 선택 근거:
    /// - List: 순서 유지, 인덱스 접근, 간단한 구현
    /// - Dictionary: 빠른 검색 (O(1)), 키 필요
    /// - Queue: FIFO, 소모품 자동 사용에 적합
    /// </summary>
    public List<Item> Inventory { get; set; }
    
    /// <summary>
    /// 인벤토리 최대 크기 (제한 없음 = int.MaxValue)
    /// Resident Evil 스타일 제한을 원하면 설정
    /// </summary>
    public int InventoryCapacity { get; set; } = int.MaxValue;
    
    /// <summary>
    /// 소지 골드. 상점 시스템에 사용
    /// </summary>
    public int Gold { get; set; }
    
    // === 장비 시스템 ===
    
    /// <summary>
    /// 장착한 무기. null이면 맨손
    /// </summary>
    public Item EquippedWeapon { get; set; }
    
    /// <summary>
    /// 장착한 방어구. null이면 무방비
    /// </summary>
    public Item EquippedArmor { get; set; }
    
    /// <summary>
    /// 생성자: 플레이어 초기화
    /// Factory Method 패턴의 단순화된 형태
    /// </summary>
    /// <param name="name">플레이어 이름</param>
    public Player(string name)
    {
        // === 초기 스탯 설정 ===
        // 게임 밸런스의 시작점
        
        Name = name;
        Level = 1;
        
        // 초기 체력: 100
        // 설계 근거: 심리학적으로 100이 직관적 (백분율 연상)
        MaxHealth = 100;
        Health = MaxHealth;  // 완전 회복 상태로 시작
        
        // 초기 공격력: 10
        // 설계 근거: 첫 적이 약 5턴에 처치되도록 조정
        Attack = 10;
        
        // 초기 방어력: 5
        // 설계 근거: 첫 적의 공격을 약 50% 감소
        Defense = 5;
        
        // 경험치 0부터 시작
        Experience = 0;
        
        // 빈 인벤토리로 시작
        Inventory = new List<Item>();
        
        // 초기 골드: 50
        // 설계 근거: 첫 포션 2개 정도 구매 가능한 금액
        Gold = 50;
    }
    
    /// <summary>
    /// 경험치 획득 및 레벨업 처리
    /// 게임의 핵심 보상 메커니즘
    /// </summary>
    /// <param name="exp">획득할 경험치</param>
    public void GainExperience(int exp)
    {
        Experience += exp;
        Console.WriteLine($"💫 {exp} 경험치를 획득했습니다!");
        Console.WriteLine($"경험치: {Experience}/{ExperienceToNextLevel}");
        
        // === 연쇄 레벨업 처리 ===
        // 한 번에 여러 레벨 오를 수 있는 경우 대비
        while (Experience >= ExperienceToNextLevel)
        {
            LevelUp();
        }
    }
    
    /// <summary>
    /// 레벨업 처리. 조작적 조건화의 강력한 보상
    /// B.F. Skinner의 행동주의 심리학 적용
    /// </summary>
    private void LevelUp()
    {
        Level++;
        
        // 경험치 오버플로우 처리
        Experience -= ExperienceToNextLevel;
        
        // === 스탯 증가 공식 ===
        // 선형 증가 vs 백분율 증가 vs 혼합
        
        int healthIncrease = 20;     // 고정 증가
        int attackIncrease = 5;      // 고정 증가
        int defenseIncrease = 3;     // 고정 증가
        
        // 대안: 백분율 증가
        // int healthIncrease = (int)(MaxHealth * 0.1);  // 10% 증가
        // int attackIncrease = (int)(Attack * 0.05);    // 5% 증가
        
        MaxHealth += healthIncrease;
        Attack += attackIncrease;
        Defense += defenseIncrease;
        
        // 레벨업 시 완전 회복 (일반적 RPG 관습)
        Health = MaxHealth;
        
        // === 화려한 레벨업 알림 ===
        // 즉각적 피드백으로 도파민 분비 촉진
        Console.WriteLine("\n╔════════════════════════════╗");
        Console.WriteLine("║    🎉 레벨 업! 🎉          ║");
        Console.WriteLine("╚════════════════════════════╝");
        Console.WriteLine($"레벨 {Level - 1} → {Level}");
        Console.WriteLine($"최대 체력: {MaxHealth - healthIncrease} → {MaxHealth} (+{healthIncrease})");
        Console.WriteLine($"공격력: {Attack - attackIncrease} → {Attack} (+{attackIncrease})");
        Console.WriteLine($"방어력: {Defense - defenseIncrease} → {Defense} (+{defenseIncrease})");
        Console.WriteLine("체력이 완전히 회복되었습니다!");
        
        // === 레벨업 이벤트 (확장 가능성) ===
        // OnLevelUp?.Invoke(this, Level);
        // 업적 시스템, 스킬 해금 등에 활용
    }
    
    /// <summary>
    /// 체력 회복. 포션이나 휴식에 사용
    /// </summary>
    /// <param name="amount">회복량</param>
    public void Heal(int amount)
    {
        int oldHealth = Health;
        Health = Math.Min(MaxHealth, Health + amount);
        int actualHealed = Health - oldHealth;
        
        Console.WriteLine($"💚 {actualHealed}만큼 회복했습니다! " +
                         $"(현재 체력: {Health}/{MaxHealth})");
    }
    
    /// <summary>
    /// 아이템을 인벤토리에 추가
    /// </summary>
    /// <param name="item">추가할 아이템</param>
    /// <returns>성공 여부</returns>
    public bool AddItem(Item item)
    {
        // 인벤토리 용량 체크
        if (Inventory.Count >= InventoryCapacity)
        {
            Console.WriteLine("❌ 인벤토리가 가득 찼습니다!");
            return false;
        }
        
        Inventory.Add(item);
        Console.WriteLine($"✅ {item.Name}을(를) 획득했습니다!");
        
        // === 아이템 획득 이벤트 ===
        // OnItemAcquired?.Invoke(this, item);
        
        return true;
    }
    
    /// <summary>
    /// 인벤토리 표시
    /// </summary>
    public void ShowInventory()
    {
        Console.WriteLine("\n╔══════════════════════════╗");
        Console.WriteLine("║       인벤토리           ║");
        Console.WriteLine("╚══════════════════════════╝");
        Console.WriteLine($"💰 골드: {Gold}");
        Console.WriteLine($"📦 아이템: {Inventory.Count}/{InventoryCapacity}");
        
        if (Inventory.Count == 0)
        {
            Console.WriteLine("아이템이 없습니다.");
            return;
        }
        
        Console.WriteLine("\n소지 아이템:");
        for (int i = 0; i < Inventory.Count; i++)
        {
            var item = Inventory[i];
            Console.WriteLine($"  {i + 1}. {item.Name} ({item.Type})");
            Console.WriteLine($"     {item.Description}");
        }
    }
    
    /// <summary>
    /// 플레이어 상태 표시 (오버라이드)
    /// Template Method 패턴의 구체적 구현
    /// </summary>
    public override void ShowStatus()
    {
        Console.WriteLine("\n╔══════════════════════════╗");
        Console.WriteLine($"║  {Name}의 상태        ║");
        Console.WriteLine("╚══════════════════════════╝");
        Console.WriteLine($"레벨: {Level}");
        Console.WriteLine($"경험치: {Experience}/{ExperienceToNextLevel} [{ExperienceProgress:P0}]");
        Console.WriteLine($"체력: {Health}/{MaxHealth} [{HealthPercentage:P0}]");
        ShowHealthBar();
        Console.WriteLine($"공격력: {Attack}" + 
            (EquippedWeapon != null ? $" (+{EquippedWeapon.Value})" : ""));
        Console.WriteLine($"방어력: {Defense}" + 
            (EquippedArmor != null ? $" (+{EquippedArmor.Value})" : ""));
        Console.WriteLine($"골드: {Gold}");
        
        Console.WriteLine("\n장비:");
        Console.WriteLine($"  무기: {EquippedWeapon?.Name ?? "없음"}");
        Console.WriteLine($"  방어구: {EquippedArmor?.Name ?? "없음"}");
    }
}
    
    public void GainExperience(int exp)
    {
        Experience += exp;
        Console.WriteLine($"{exp} 경험치를 획득했습니다!");
        
        while (Experience >= ExperienceToNextLevel)
        {
            LevelUp();
        }
    }
    
    void LevelUp()
    {
        Level++;
        Experience -= ExperienceToNextLevel;
        
        MaxHealth += 20;
        Health = MaxHealth;
        Attack += 5;
        Defense += 3;
        
        Console.WriteLine($"\n🎉 레벨 업! 레벨 {Level}이 되었습니다!");
        Console.WriteLine($"최대 체력: +20, 공격력: +5, 방어력: +3");
    }
    
    public void Heal(int amount)
    {
        Health = Math.Min(MaxHealth, Health + amount);
        Console.WriteLine($"{amount}만큼 회복했습니다! (현재 체력: {Health}/{MaxHealth})");
    }
    
    public void AddItem(Item item)
    {
        Inventory.Add(item);
        Console.WriteLine($"{item.Name}을(를) 획득했습니다!");
    }
    
    public void ShowInventory()
    {
        Console.WriteLine("\n=== 인벤토리 ===");
        Console.WriteLine($"골드: {Gold}");
        
        if (Inventory.Count == 0)
        {
            Console.WriteLine("아이템이 없습니다.");
            return;
        }
        
        for (int i = 0; i < Inventory.Count; i++)
        {
            var item = Inventory[i];
            Console.WriteLine($"{i + 1}. {item.Name} ({item.Type}) - {item.Description}");
        }
    }
}
```

### Enemy 클래스 - 적대적 AI의 구현

Enemy 클래스는 Player의 대항마(Antagonist)로서, **절차적 생성(Procedural Generation)**과 **난이도 스케일링(Difficulty Scaling)**을 구현합니다. 이는 Rogue(1980)가 시작한 **로그라이크(Roguelike)** 장르의 핵심 메커니즘입니다.

```csharp
/// <summary>
/// 적 캐릭터 클래스. 랜덤 생성과 플레이어 레벨 기반 스케일링 지원
/// Factory Method 패턴과 절차적 생성 적용
/// </summary>
class Enemy : Character
{
    /// <summary>
    /// 처치 시 제공할 경험치
    /// 게임 디자인: 플레이어 레벨에 비례하여 증가
    /// </summary>
    public int ExpReward { get; set; }
    
    /// <summary>
    /// 처치 시 제공할 골드
    /// 경제 밸런스: 포션 가격을 고려하여 설정
    /// </summary>
    public int GoldReward { get; set; }
    
    /// <summary>
    /// 적 타입. 나중에 특수 능력 구현에 사용 가능
    /// </summary>
    public string Type { get; set; }
    
    /// <summary>
    /// Factory Method: 플레이어 레벨에 맞는 적을 랜덤 생성
    /// 절차적 생성(Procedural Generation)의 기초
    /// </summary>
    /// <param name="playerLevel">플레이어의 현재 레벨</param>
    /// <returns>생성된 적 인스턴스</returns>
    public static Enemy CreateRandom(int playerLevel)
    {
        // === 난수 생성기 ===
        // Random 인스턴스를 static으로 만들어 시드 문제 방지
        // 짧은 시간 내 반복 호출 시 동일한 시드로 같은 난수 생성 방지
        Random rand = new Random();
        
        // === 적 타입 정의 ===
        // 다양성을 위한 이름 풀(Pool)
        // 확장: 각 타입별로 다른 특성 부여 가능
        string[] enemyNames = { 
            "고블린",      // 약한 적, 빠름
            "오크",        // 강한 적, 느림
            "늑대",        // 중간, 민첩
            "스켈레톤",    // 언데드, 방어력 낮음
            "슬라임"       // 약함, 분열 가능(미구현)
        };
        
        string name = enemyNames[rand.Next(enemyNames.Length)];
        
        // === 난이도 스케일링 공식 ===
        // 플레이어 레벨에 비례하여 적 능력치 증가
        // 설계 목표: 플레이어가 약 5-10턴에 처치 가능하도록
        
        // 체력 공식: 30 + (레벨 * 10)
        // 레벨 1: 40 HP, 레벨 5: 80 HP
        int enemyHealth = 30 + (playerLevel * 10);
        
        // 공격력 공식: 5 + (레벨 * 2)
        // 플레이어 방어력 증가(+3/레벨)보다 빠르게 증가하여 도전 유지
        int enemyAttack = 5 + (playerLevel * 2);
        
        // 방어력 공식: 2 + 레벨
        // 플레이어 공격력 증가(+5/레벨)보다 느리게 증가
        int enemyDefense = 2 + playerLevel;
        
        // === 보상 스케일링 ===
        // 위험-보상 균형 (Risk-Reward Balance)
        
        // 경험치: 20 + (레벨 * 5)
        // 설계: 약 4-5마리 처치 시 레벨업
        int expReward = 20 + (playerLevel * 5);
        
        // 골드: 10 + (레벨 * 3)
        // 설계: 포션 가격(50골드) 고려, 5마리 처치 시 1개 구매 가능
        int goldReward = 10 + (playerLevel * 3);
        
        // === 객체 초기화 구문(Object Initializer) ===
        // C# 3.0의 간결한 초기화 방식
        return new Enemy
        {
            Name = name,
            Type = name,  // 나중에 타입별 특수 능력 구현 시 사용
            MaxHealth = enemyHealth,
            Health = enemyHealth,  // 완전 체력 상태로 생성
            Attack = enemyAttack,
            Defense = enemyDefense,
            ExpReward = expReward,
            GoldReward = goldReward
        };
    }
    
    /// <summary>
    /// 적 AI의 행동 결정. 현재는 단순 공격만 수행
    /// </summary>
    /// <param name="player">공격 대상 플레이어</param>
    /// <returns>수행한 행동 설명</returns>
    public string DecideAction(Player player)
    {
        // === 단순 AI (Version 1.0) ===
        // 항상 공격. 예측 가능하지만 구현 간단
        
        return "attack";
        
        // === 향상된 AI (Future) ===
        // 체력 비율에 따른 의사결정
        /*
        if (HealthPercentage < 0.3 && HasHealingPotion)
        {
            return "heal";  // 체력 낮으면 회복
        }
        else if (HealthPercentage < 0.5)
        {
            return "defend";  // 체력 절반 이하면 방어
        }
        else
        {
            return "attack";  // 그 외에는 공격
        }
        */
    }
    
    /// <summary>
    /// 적 사망 시 호출. 이벤트 처리 및 보상 지급
    /// </summary>
    public override void OnDeath()
    {
        Console.WriteLine($"💀 {Name}을(를) 물리쳤습니다!");
        // 추가 이벤트 처리 가능
    }
}
```

**절차적 생성(Procedural Generation)의 이론:**

적의 랜덤 생성은 **절차적 콘텐츠 생성(Procedural Content Generation, PCG)**의 가장 단순한 형태입니다. 이는 1980년 Rogue에서 시작되어 Minecraft, No Man's Sky로 발전한 핵심 기술입니다.

**난이도 곡선 설계:**

```
플레이어 DPS = (Attack + 5*Level) - (2 + Level) = Attack + 4*Level - 2
적 DPS = (5 + 2*Level) - (5 + 3*Level) = -3*Level (음수!)

실제로는 플레이어가 유리하게 설계됨
이는 "플레이어 권한 부여(Player Empowerment)" 원칙
```

게임은 플레이어가 점진적으로 강해지는 **파워 판타지(Power Fantasy)**를 제공해야 합니다.

---

## 21.3 전투 시스템

턴 기반 전투 시스템을 구현합니다.

### 전투 흐름

1. 전투 시작 (적 등장)
2. 플레이어 턴: 행동 선택 (공격, 방어, 아이템, 도망)
3. 적 턴: AI 행동
4. 승패 판정
5. 전리품 획득 또는 게임 오버

### 전투 메서드 구현

```csharp
class Game
{
    Player player;
    
    void StartBattle()
    {
        Enemy enemy = Enemy.CreateRandom(player.Level);
        Console.WriteLine($"\n⚔️  {enemy.Name}이(가) 나타났다!");
        enemy.ShowStatus();
        
        bool defending = false;
        
        while (player.IsAlive && enemy.IsAlive)
        {
            Console.WriteLine("\n━━━━━━━━━━━━━━━━━━━━");
            Console.WriteLine("1. 공격");
            Console.WriteLine("2. 방어");
            Console.WriteLine("3. 아이템 사용");
            Console.WriteLine("4. 도망");
            Console.Write("선택: ");
            
            string choice = Console.ReadLine();
            
            switch (choice)
            {
                case "1": // 공격
                    PlayerAttack(enemy);
                    break;
                    
                case "2": // 방어
                    Console.WriteLine("방어 자세를 취했습니다!");
                    defending = true;
                    break;
                    
                case "3": // 아이템
                    if (!UseItem())
                    {
                        continue; // 아이템 사용 실패 시 턴 다시
                    }
                    break;
                    
                case "4": // 도망
                    if (TryEscape())
                    {
                        Console.WriteLine("도망쳤습니다!");
                        return;
                    }
                    Console.WriteLine("도망치지 못했습니다!");
                    break;
                    
                default:
                    Console.WriteLine("잘못된 선택입니다.");
                    continue;
            }
            
            if (!enemy.IsAlive)
            {
                Victory(enemy);
                return;
            }
            
            // 적의 턴
            EnemyTurn(enemy, defending);
            defending = false;
            
            if (!player.IsAlive)
            {
                GameOver();
                return;
            }
        }
    }
    
    void PlayerAttack(Enemy enemy)
    {
        Console.WriteLine($"\n{player.Name}의 공격!");
        enemy.TakeDamage(player.Attack);
    }
    
    void EnemyTurn(Enemy enemy, bool playerDefending)
    {
        Console.WriteLine($"\n{enemy.Name}의 공격!");
        int damage = enemy.Attack;
        
        if (playerDefending)
        {
            damage = damage / 2;
            Console.WriteLine("방어로 피해가 감소했습니다!");
        }
        
        player.TakeDamage(damage);
    }
    
    bool UseItem()
    {
        player.ShowInventory();
        
        if (player.Inventory.Count == 0)
        {
            return false;
        }
        
        Console.Write("사용할 아이템 번호 (0: 취소): ");
        if (!int.TryParse(Console.ReadLine(), out int index))
        {
            return false;
        }
        
        if (index == 0) return false;
        
        index--; // 0-based index
        if (index < 0 || index >= player.Inventory.Count)
        {
            Console.WriteLine("잘못된 선택입니다.");
            return false;
        }
        
        var item = player.Inventory[index];
        
        if (item.Type == "포션")
        {
            player.Heal(item.Value);
            player.Inventory.RemoveAt(index);
            return true;
        }
        
        Console.WriteLine("전투 중에는 사용할 수 없는 아이템입니다.");
        return false;
    }
    
    bool TryEscape()
    {
        Random rand = new Random();
        return rand.Next(100) < 50; // 50% 확률
    }
    
    void Victory(Enemy enemy)
    {
        Console.WriteLine($"\n🎊 {enemy.Name}을(를) 물리쳤습니다!");
        player.GainExperience(enemy.ExpReward);
        player.Gold += enemy.GoldReward;
        Console.WriteLine($"{enemy.GoldReward} 골드를 획득했습니다!");
        
        // 랜덤 아이템 드롭
        if (new Random().Next(100) < 30) // 30% 확률
        {
            Item loot = Item.GenerateRandomLoot();
            player.AddItem(loot);
        }
    }
    
    void GameOver()
    {
        Console.WriteLine("\n💀 전투에서 패배했습니다...");
        Console.WriteLine("게임 오버!");
    }
}
```

---

## 21.4 아이템 시스템

다양한 아이템을 관리하는 시스템을 구현합니다.

### Item 클래스

```csharp
class Item
{
    public string Name { get; set; }
    public string Type { get; set; }
    public int Value { get; set; }
    public string Description { get; set; }
    
    public static Item GenerateRandomLoot()
    {
        Random rand = new Random();
        int roll = rand.Next(100);
        
        if (roll < 50) // 포션 (50%)
        {
            return new Item
            {
                Name = "체력 포션",
                Type = "포션",
                Value = 30,
                Description = "체력을 30 회복합니다"
            };
        }
        else if (roll < 70) // 강화 포션 (20%)
        {
            return new Item
            {
                Name = "힘의 물약",
                Type = "버프",
                Value = 5,
                Description = "공격력을 일시적으로 증가시킵니다"
            };
        }
        else if (roll < 85) // 무기 (15%)
        {
            return new Item
            {
                Name = "강철 검",
                Type = "무기",
                Value = 10,
                Description = "공격력 +10"
            };
        }
        else // 방어구 (15%)
        {
            return new Item
            {
                Name = "가죽 갑옷",
                Type = "방어구",
                Value = 5,
                Description = "방어력 +5"
            };
        }
    }
}
```

### 아이템 사용 및 장착

```csharp
class Player : Character
{
    public Item EquippedWeapon { get; set; }
    public Item EquippedArmor { get; set; }
    
    public void EquipItem(int index)
    {
        if (index < 0 || index >= Inventory.Count)
        {
            Console.WriteLine("잘못된 선택입니다.");
            return;
        }
        
        var item = Inventory[index];
        
        if (item.Type == "무기")
        {
            if (EquippedWeapon != null)
            {
                Attack -= EquippedWeapon.Value;
                Inventory.Add(EquippedWeapon);
            }
            
            EquippedWeapon = item;
            Attack += item.Value;
            Inventory.RemoveAt(index);
            Console.WriteLine($"{item.Name}을(를) 장착했습니다! 공격력이 증가했습니다.");
        }
        else if (item.Type == "방어구")
        {
            if (EquippedArmor != null)
            {
                Defense -= EquippedArmor.Value;
                Inventory.Add(EquippedArmor);
            }
            
            EquippedArmor = item;
            Defense += item.Value;
            Inventory.RemoveAt(index);
            Console.WriteLine($"{item.Name}을(를) 장착했습니다! 방어력이 증가했습니다.");
        }
        else
        {
            Console.WriteLine("장착할 수 없는 아이템입니다.");
        }
    }
}
```

---

## 21.5 게임 진행 및 저장

게임의 메인 루프와 저장/불러오기 기능을 구현합니다.

### 게임 저장 데이터 구조

```csharp
class GameSave
{
    public string PlayerName { get; set; }
    public int Level { get; set; }
    public int Health { get; set; }
    public int MaxHealth { get; set; }
    public int Attack { get; set; }
    public int Defense { get; set; }
    public int Experience { get; set; }
    public int Gold { get; set; }
    public List<Item> Inventory { get; set; }
    public Item EquippedWeapon { get; set; }
    public Item EquippedArmor { get; set; }
}
```

### 저장 및 불러오기

```csharp
using System.Text.Json;

class Game
{
    const string SAVE_FILE = "gamesave.json";
    
    void SaveGame()
    {
        try
        {
            var save = new GameSave
            {
                PlayerName = player.Name,
                Level = player.Level,
                Health = player.Health,
                MaxHealth = player.MaxHealth,
                Attack = player.Attack,
                Defense = player.Defense,
                Experience = player.Experience,
                Gold = player.Gold,
                Inventory = player.Inventory,
                EquippedWeapon = player.EquippedWeapon,
                EquippedArmor = player.EquippedArmor
            };
            
            string json = JsonSerializer.Serialize(save, new JsonSerializerOptions 
            { 
                WriteIndented = true 
            });
            
            File.WriteAllText(SAVE_FILE, json);
            Console.WriteLine("게임이 저장되었습니다.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"저장 실패: {ex.Message}");
        }
    }
    
    bool LoadGame()
    {
        try
        {
            if (!File.Exists(SAVE_FILE))
            {
                Console.WriteLine("저장된 게임이 없습니다.");
                return false;
            }
            
            string json = File.ReadAllText(SAVE_FILE);
            var save = JsonSerializer.Deserialize<GameSave>(json);
            
            player = new Player(save.PlayerName)
            {
                Level = save.Level,
                Health = save.Health,
                MaxHealth = save.MaxHealth,
                Attack = save.Attack,
                Defense = save.Defense,
                Experience = save.Experience,
                Gold = save.Gold,
                Inventory = save.Inventory ?? new List<Item>(),
                EquippedWeapon = save.EquippedWeapon,
                EquippedArmor = save.EquippedArmor
            };
            
            Console.WriteLine("게임을 불러왔습니다.");
            return true;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"불러오기 실패: {ex.Message}");
            return false;
        }
    }
}
```

### 메인 게임 루프

```csharp
class Game
{
    Player player;
    bool isRunning;
    
    public void Start()
    {
        Console.WriteLine("═══════════════════════════════");
        Console.WriteLine("  텍스트 RPG 게임");
        Console.WriteLine("═══════════════════════════════");
        
        ShowMainMenu();
    }
    
    void ShowMainMenu()
    {
        while (true)
        {
            Console.WriteLine("\n1. 새 게임");
            Console.WriteLine("2. 게임 불러오기");
            Console.WriteLine("3. 종료");
            Console.Write("선택: ");
            
            string choice = Console.ReadLine();
            
            switch (choice)
            {
                case "1":
                    StartNewGame();
                    if (player != null) GameLoop();
                    return;
                    
                case "2":
                    if (LoadGame())
                    {
                        GameLoop();
                        return;
                    }
                    break;
                    
                case "3":
                    Console.WriteLine("게임을 종료합니다.");
                    return;
                    
                default:
                    Console.WriteLine("잘못된 선택입니다.");
                    break;
            }
        }
    }
    
    void StartNewGame()
    {
        Console.Write("\n캐릭터 이름을 입력하세요: ");
        string name = Console.ReadLine();
        
        if (string.IsNullOrWhiteSpace(name))
        {
            Console.WriteLine("이름을 입력해야 합니다.");
            return;
        }
        
        player = new Player(name);
        Console.WriteLine($"\n환영합니다, {name}님!");
        Console.WriteLine("던전 탐험을 시작합니다...");
    }
    
    void GameLoop()
    {
        isRunning = true;
        
        while (isRunning && player.IsAlive)
        {
            Console.WriteLine("\n━━━━━━━━━━━━━━━━━━━━");
            Console.WriteLine("무엇을 하시겠습니까?");
            Console.WriteLine("1. 던전 탐험");
            Console.WriteLine("2. 상태 확인");
            Console.WriteLine("3. 인벤토리");
            Console.WriteLine("4. 장비 관리");
            Console.WriteLine("5. 게임 저장");
            Console.WriteLine("6. 게임 종료");
            Console.Write("선택: ");
            
            string choice = Console.ReadLine();
            
            switch (choice)
            {
                case "1":
                    Explore();
                    break;
                    
                case "2":
                    player.ShowStatus();
                    break;
                    
                case "3":
                    player.ShowInventory();
                    break;
                    
                case "4":
                    ManageEquipment();
                    break;
                    
                case "5":
                    SaveGame();
                    break;
                    
                case "6":
                    Console.Write("저장하고 종료하시겠습니까? (y/n): ");
                    if (Console.ReadLine()?.ToLower() == "y")
                    {
                        SaveGame();
                    }
                    isRunning = false;
                    break;
                    
                default:
                    Console.WriteLine("잘못된 선택입니다.");
                    break;
            }
        }
        
        if (!player.IsAlive)
        {
            Console.WriteLine("\n게임 오버!");
        }
    }
    
    void Explore()
    {
        Console.WriteLine("\n던전을 탐험합니다...");
        
        Random rand = new Random();
        int encounter = rand.Next(100);
        
        if (encounter < 60) // 60% 전투
        {
            StartBattle();
        }
        else if (encounter < 80) // 20% 아이템 발견
        {
            Console.WriteLine("보물 상자를 발견했습니다!");
            Item treasure = Item.GenerateRandomLoot();
            player.AddItem(treasure);
        }
        else // 20% 안전
        {
            Console.WriteLine("조용합니다... 아무 일도 일어나지 않았습니다.");
            player.Heal(10);
            Console.WriteLine("휴식을 취하며 10의 체력을 회복했습니다.");
        }
    }
    
    void ManageEquipment()
    {
        Console.WriteLine("\n=== 장비 관리 ===");
        Console.WriteLine($"무기: {player.EquippedWeapon?.Name ?? "없음"}");
        Console.WriteLine($"방어구: {player.EquippedArmor?.Name ?? "없음"}");
        
        player.ShowInventory();
        
        Console.Write("\n장착할 아이템 번호 (0: 취소): ");
        if (int.TryParse(Console.ReadLine(), out int index) && index > 0)
        {
            player.EquipItem(index - 1);
        }
    }
}
```

### 프로그램 시작점

```csharp
// 프로그램 실행
Game game = new Game();
game.Start();
```

---

## 21장 정리

이 장에서는 간단한 텍스트 기반 RPG 게임을 만들어 보았습니다. 핵심 개념을 정리하면:

### 주요 학습 내용

**1. 객체지향 설계**
- 상속을 활용한 캐릭터 구조 (Character → Player/Enemy)
- 클래스를 통한 게임 엔티티 모델링
- 캡슐화를 통한 데이터 보호

**2. 게임 시스템 구현**
- 턴 기반 전투 시스템
- 인벤토리 및 장비 관리
- 레벨업과 경험치 시스템

**3. 상태 관리**
- 게임 상태 저장 (JSON 직렬화)
- 저장된 게임 불러오기
- 게임 진행 상태 유지

**4. 랜덤성과 게임 밸런스**
- 랜덤 인카운터
- 확률 기반 아이템 드롭
- 플레이어 레벨에 따른 적 난이도 조절

### 확장 아이디어

이 기본 게임을 더욱 발전시킬 수 있는 방법:

**1. 콘텐츠 확장**
- 다양한 적 타입과 보스
- 더 많은 아이템과 장비
- 스킬 시스템
- 퀘스트 시스템

**2. 시스템 개선**
- 여러 던전 지역
- 상점과 NPC
- 파티 시스템
- 난이도 설정

**3. UI 개선**
- 색상 있는 텍스트 출력
- ASCII 아트
- 더 나은 전투 애니메이션 효과

**4. 고급 기능**
- 업적 시스템
- 순위표
- 멀티플레이 (턴 기반)

### 실습 과제

1. 새로운 적 타입을 추가하고 각기 다른 전투 패턴을 구현해보세요
2. 스킬 시스템을 추가하여 다양한 전투 옵션을 제공해보세요
3. 상점 시스템을 만들어 아이템을 사고팔 수 있게 해보세요
4. 여러 던전 레벨을 만들고 난이도를 조절해보세요

이 장에서 만든 게임은 기본적인 구조이지만, 여러분의 창의성을 더해 독특한 게임으로 발전시킬 수 있습니다. 객체지향 프로그래밍의 원칙을 따르며 확장하면 복잡한 게임 시스템도 체계적으로 관리할 수 있습니다!
