# 부록 C. Visual Studio Code 단축키

소프트웨어 개발의 역사를 돌이켜보면, 개발 도구의 진화는 단순히 기능의 추가를 넘어 개발자의 사고방식과 작업 흐름을 근본적으로 변화시켜 왔습니다. 1960년대 천공 카드로 프로그램을 작성하던 시대부터, 1970년대 vi와 Emacs 같은 텍스트 편집기의 등장, 1990년대 Visual Studio와 IntelliJ IDEA 같은 통합 개발 환경(IDE)의 부상, 그리고 2010년대 중반 Visual Studio Code의 혁신에 이르기까지, 각 세대의 도구는 당대 개발자들의 생산성을 획기적으로 향상시켰습니다.

**Visual Studio Code의 탄생과 철학:**

Visual Studio Code(이하 VS Code)는 Microsoft가 2015년 4월에 발표한 크로스 플랫폼 소스 코드 편집기입니다. Erich Gamma(Eclipse 공동 창시자, Gang of Four 디자인 패턴 저자)가 이끄는 팀이 개발했으며, Electron 프레임워크를 기반으로 구축되어 Windows, macOS, Linux에서 동일한 경험을 제공합니다. VS Code는 "가볍고 빠른 편집기"와 "강력한 IDE 기능" 사이의 균형을 추구하며, 다음과 같은 핵심 설계 원칙을 따릅니다:

1. **속도와 경량화**: Sublime Text의 빠른 시작 시간과 가벼운 메모리 사용량을 목표로 함
2. **확장성**: 마켓플레이스를 통한 무한한 확장 가능성
3. **IntelliSense**: 언어별 지능형 코드 완성 및 제안
4. **통합 디버깅**: 별도 도구 없이 편집기 내에서 디버깅
5. **Git 통합**: 버전 관리를 편집기 워크플로우에 자연스럽게 통합
6. **크로스 플랫폼**: 모든 주요 운영체제에서 동일한 사용자 경험

**키보드 중심 워크플로우의 중요성:**

Bill Joy(vi 편집기 개발자), Richard Stallman(Emacs 개발자), 그리고 Bram Moolenaar(Vim 개발자)가 강조했듯이, 숙련된 개발자의 생산성은 키보드 중심 워크플로우에서 나옵니다. 연구에 따르면:

- **마우스 사용의 비용**: 키보드에서 마우스로 손을 옮기는 데 평균 0.5~1초가 소요되며, 하루 수백 번 반복하면 상당한 시간 손실로 이어집니다.
- **근육 기억(Muscle Memory)**: 단축키는 의식적 사고 없이 실행되는 자동화된 동작으로 발전하여, 개발자가 문제 해결에 더 집중할 수 있게 합니다.
- **흐름 상태(Flow State)**: 지속적인 마우스 사용은 코딩의 몰입 상태를 방해하지만, 단축키는 흐름을 유지하게 합니다.
- **RSI 예방**: 반복적 긴장 손상(Repetitive Strain Injury)을 줄이고 장기적인 건강을 보호합니다.

**VS Code 단축키 시스템의 설계:**

VS Code의 단축키 시스템은 다음과 같은 특징을 갖습니다:

1. **계층적 구조**: `Ctrl+K` 같은 "프리픽스 키"를 통해 수백 개의 명령을 논리적으로 그룹화
2. **상황 인식**: 현재 편집기 상태(언어 모드, 포커스 위치)에 따라 동일한 키 조합이 다른 동작 수행
3. **완전한 커스터마이징**: `keybindings.json`을 통해 모든 단축키를 사용자가 재정의 가능
4. **크로스 플랫폼 일관성**: Windows/Linux의 `Ctrl`이 macOS의 `Cmd`로 자연스럽게 매핑
5. **확장 통합**: 확장 프로그램이 자체 단축키를 등록하고 명령 팔레트에 통합

**이 부록의 구성과 활용:**

이 부록은 C# 개발자가 VS Code를 효율적으로 사용하기 위한 필수 단축키를 체계적으로 다룹니다. 단순한 키 조합 나열이 아니라, 각 단축키의 사용 맥락, 실무 활용 패턴, 그리고 생산성 향상 전략을 함께 제시합니다. 또한 C# 개발에 특화된 단축키와 워크플로우를 중점적으로 설명하여, 독자가 즉시 실무에 적용할 수 있도록 구성했습니다.

**학습 전략:**

단축키 학습은 마라톤이지 단거리 달리기가 아닙니다. 다음 접근 방식을 권장합니다:

1. **점진적 습득**: 한 번에 3-5개의 단축키만 선택하여 1주일간 의식적으로 사용
2. **빈도 기반 우선순위**: 가장 자주 수행하는 작업의 단축키부터 학습
3. **근육 기억 형성**: 처음에는 느리더라도 일관되게 단축키 사용
4. **마우스 제거**: 특정 작업에서 의도적으로 마우스 사용을 금지하고 단축키만 사용
5. **짝 프로그래밍 관찰**: 숙련된 개발자의 단축키 사용 패턴 학습

**단축키 표기법:**

이 부록에서는 다음과 같은 표기법을 사용합니다:

- **Windows/Linux**: `Ctrl`, `Alt`, `Shift`, `Win` (Windows 키)
- **macOS**: `Cmd` (⌘), `Option` (⌥), `Shift` (⇧), `Ctrl` (^)
- **범용 표기**: 주 표기는 Windows/Linux 기준이며, macOS는 괄호 안에 별도 표시
- **순차 입력**: `Ctrl+K Ctrl+S`는 Ctrl+K를 누른 후 손을 떼고 다시 Ctrl+S를 누르는 것
- **동시 입력**: `Ctrl+Shift+P`는 세 키를 동시에 누르는 것

**C# 개발 환경 설정:**

VS Code에서 C# 개발을 위해서는 다음 확장을 설치하는 것을 권장합니다:

- **C# Dev Kit**: Microsoft 공식 C# 확장 (IntelliSense, 디버깅, 리팩토링)
- **.NET Core Test Explorer**: 단위 테스트 실행 및 관리
- **C# Extensions**: 코드 스니펫 및 편의 기능
- **NuGet Gallery**: NuGet 패키지 관리

이러한 확장들이 설치되면, 이 부록에서 설명하는 단축키들이 C# 개발에 최적화된 기능과 함께 작동합니다.

---

## C.1 기본 편집

기본 편집 기능은 모든 소프트웨어 개발의 근간이 되는 작업입니다. Donald Knuth는 "프로그래밍은 생각을 코드로 변환하는 예술"이라고 했는데, 이 변환 과정에서 편집 작업이 얼마나 효율적이냐에 따라 개발자의 사고 흐름이 방해받을 수도, 촉진될 수도 있습니다. 이 섹션에서는 텍스트 편집의 기본부터 고급 기법까지, C# 코드 작성에 필수적인 단축키를 깊이 있게 다룹니다.

### 파일 생명주기 관리: 생성에서 닫기까지

파일 관리는 개발 세션의 시작과 끝을 담당하는 기본 작업입니다. VS Code는 버퍼 기반 편집 모델을 사용하여, 파일을 메모리에 로드하고 수정 사항을 추적한 후 명시적 저장 명령으로 디스크에 기록합니다. 이는 vim이나 Emacs의 전통을 계승한 방식으로, 빠른 편집과 안전한 데이터 관리를 모두 제공합니다.

```
Ctrl+N (Cmd+N)                새 파일 생성
Ctrl+O (Cmd+O)                파일 열기 대화상자
Ctrl+S (Cmd+S)                파일 저장
Ctrl+Shift+S (Cmd+Shift+S)    다른 이름으로 저장
Ctrl+K S                      모두 저장
Ctrl+W (Cmd+W)                현재 편집기 탭 닫기
Ctrl+K Ctrl+W (Cmd+K Cmd+W)   모든 편집기 탭 닫기
Ctrl+Shift+T (Cmd+Shift+T)    최근 닫은 편집기 다시 열기
```

**파일 작업의 심화 이해:**

**1. 새 파일 생성 워크플로우:**

`Ctrl+N`으로 생성된 파일은 "Untitled-1" 같은 임시 이름을 갖습니다. 이 상태에서 코드를 작성하면 VS Code는 언어 모드를 자동으로 감지하려 시도하지만, C# 개발자는 다음 방법으로 명시적으로 언어를 지정할 수 있습니다:

- `Ctrl+K M` (Change Language Mode)를 눌러 "C#" 선택
- 파일 확장자를 `.cs`로 저장하면 자동으로 C# 모드 활성화
- 상태 표시줄 우측 하단의 언어 표시기를 클릭하여 변경

**실무 패턴**: 빠른 프로토타이핑을 위해 `Ctrl+N`으로 새 파일을 만들고, 코드를 실험한 후, 유용하다고 판단되면 적절한 위치에 저장합니다. 이는 "스크래치패드" 방식으로, 메인 코드베이스를 어지럽히지 않고 아이디어를 테스트할 수 있습니다.

**2. 빠른 파일 열기: Ctrl+P의 강력함:**

`Ctrl+P` (Quick Open)는 VS Code에서 가장 자주 사용되는 단축키 중 하나입니다. 이는 Sublime Text의 "Goto Anything" 기능에서 영감을 받았으며, 퍼지 매칭(fuzzy matching) 알고리즘을 사용하여 파일 이름의 일부만 입력해도 원하는 파일을 찾을 수 있습니다.

```
Ctrl+P (Cmd+P)                빠른 파일 열기
Ctrl+P, @                     현재 파일의 심볼로 이동
Ctrl+P, #                     작업 영역의 심볼 검색
Ctrl+P, :                     줄 번호로 이동
Ctrl+Tab                      최근 열린 파일 순환
Ctrl+Shift+Tab                최근 열린 파일 역순환
```

**퍼지 매칭 활용 예시:**

대형 프로젝트에서 `CustomerOrderRepository.cs` 파일을 찾으려면:
- `Ctrl+P` → `corder` 입력: "C"ustomer "Order"의 문자만으로 매칭
- `Ctrl+P` → `cor.cs` 입력: 약어와 확장자 조합
- `Ctrl+P` → `customer/order` 입력: 경로 힌트 포함

이 방식은 마우스로 폴더 트리를 탐색하는 것보다 5-10배 빠르며, 프로젝트 구조를 완벽히 기억할 필요가 없습니다.

**3. 자동 저장의 철학:**

VS Code는 세 가지 저장 전략을 지원합니다:

- **수동 저장** (기본값): `Ctrl+S`로 명시적 저장, 전통적인 방식으로 개발자가 완전히 제어
- **자동 저장 (포커스 변경 시)**: 다른 편집기나 애플리케이션으로 전환 시 자동 저장
- **자동 저장 (지연 시)**: 일정 시간(기본 1초) 타이핑이 없으면 자동 저장

설정 방법:
```json
{
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000
}
```

**C# 개발에서의 고려사항:**

- **빌드 시스템과의 통합**: 일부 빌드 도구는 파일 변경을 감시하므로, 자동 저장이 불필요한 재빌드를 유발할 수 있습니다.
- **Git과의 상호작용**: 자동 저장은 작업 디렉터리를 지속적으로 변경하므로, Git 상태가 자주 바뀝니다.
- **페어 프로그래밍**: 공유 화면에서 자동 저장은 변경 사항을 즉시 반영하여 협업을 원활하게 합니다.

**4. 탭 관리 전략:**

현대 개발자는 동시에 수십 개의 파일을 열어두는 경우가 많습니다. 효율적인 탭 관리는 생산성의 핵심입니다:

```
Ctrl+K Ctrl+W             모든 편집기 닫기
Ctrl+K W                  편집기 그룹 닫기
Ctrl+K F                  폴더 닫기
Alt+Click (탭)            탭 유지/해제 전환
```

**핀(Pin) 기능**: 탭을 우클릭하고 "Pin"을 선택하거나 `Alt+Click`하면 탭이 고정되어 실수로 닫히지 않습니다. 자주 참조하는 파일(예: `appsettings.json`, 인터페이스 정의)에 유용합니다.

**편집기 그룹**: `Ctrl+\`로 화면을 분할하면 여러 파일을 동시에 볼 수 있습니다. 예를 들어, 테스트 파일과 프로덕션 코드를 나란히 배치하거나, 인터페이스와 구현을 비교할 때 사용합니다.

### 텍스트 편집의 기본: 잘라내기, 복사, 붙여넣기의 진화

클립보드 작업은 컴퓨터 역사상 가장 오래되고 보편적인 패러다임 중 하나입니다. 1970년대 Xerox PARC의 연구원들이 개발한 "Cut, Copy, Paste" 개념은 오늘날까지 모든 텍스트 편집기의 핵심으로 남아있습니다. VS Code는 이 전통적인 개념을 현대적으로 재해석하여, 줄 단위 작업과 스마트한 컨텍스트 인식을 추가했습니다.

```
Ctrl+X (Cmd+X)              잘라내기 (선택 없으면 현재 줄)
Ctrl+C (Cmd+C)              복사 (선택 없으면 현재 줄)
Ctrl+V (Cmd+V)              붙여넣기
Ctrl+Shift+V (Cmd+Shift+V)  Markdown 미리보기로 붙여넣기
Ctrl+Z (Cmd+Z)              실행 취소 (Undo)
Ctrl+Y (Cmd+Shift+Z)        다시 실행 (Redo)
Ctrl+Shift+Z (Cmd+Shift+Z)  다시 실행 (대체 키)
Ctrl+U                      마지막 커서 위치 실행 취소
```

**줄 단위 작업의 혁명:**

전통적인 편집기에서는 `Ctrl+C`를 누르기 전에 반드시 텍스트를 선택해야 했습니다. VS Code는 선택 영역이 없을 때 현재 줄 전체를 대상으로 하는 "스마트 기본값"을 도입했습니다. 이는 다음과 같은 시나리오에서 매우 효율적입니다:

**실무 예제 1: 디버깅 로그 추가**

```csharp
public void ProcessOrder(Order order)
{
    // 커서가 이 줄에 있음
    ValidateOrder(order);
    // Ctrl+C (줄 전체 복사)
    // 아래로 이동
    // Ctrl+V (붙여넣기)
}

// 결과:
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    ValidateOrder(order);  // 붙여넣어진 줄
}
```

이제 복사한 줄을 수정하여 로깅으로 변환:
```csharp
ValidateOrder(order);
_logger.LogInformation("Validating order {OrderId}", order.Id);
```

**실무 예제 2: 코드 줄 제거**

```csharp
public decimal CalculateTotal(List<Item> items)
{
    var total = 0m;
    var debugCounter = 0;  // 커서가 여기 있음
    foreach (var item in items)
    {
        debugCounter++;  // 디버그 코드
        total += item.Price;
    }
    return total;
}
```

디버그 코드 제거: 첫 번째 디버그 줄에서 `Ctrl+X` → 두 번째 디버그 줄에서 `Ctrl+X`. 선택 없이 바로 줄 단위로 잘라내기가 실행됩니다.

**클립보드 히스토리:**

VS Code는 기본적으로 단일 클립보드만 지원하지만, 확장 프로그램을 통해 클립보드 히스토리를 활성화할 수 있습니다. "Clipboard History" 확장을 설치하면 최근 복사한 여러 항목을 선택하여 붙여넣을 수 있습니다.

**실행 취소/다시 실행의 정교한 제어:**

VS Code의 실행 취소 시스템은 "타임라인 기반"이 아닌 "변경 단위 기반"입니다. 이는 다음을 의미합니다:

- 각 타이핑 세션이 하나의 실행 취소 단위를 형성
- 커서 이동 후 타이핑하면 새로운 실행 취소 단위 시작
- 명령 실행(예: 포맷팅)은 독립적인 실행 취소 단위

**고급 기능: 커서 위치 실행 취소 (Ctrl+U):**

`Ctrl+U`는 텍스트 변경이 아닌 커서 이동만 취소합니다. 이는 다음 시나리오에서 유용합니다:

```csharp
// 시나리오: 클래스의 특정 메서드를 찾기 위해 여러 번 검색하고 이동
public class CustomerService
{
    public void CreateCustomer() { }
    public void UpdateCustomer() { }  // 여기로 점프했지만 잘못된 위치
    public void DeleteCustomer() { }
    // Ctrl+U를 여러 번 눌러 원래 작업하던 위치로 복귀
}
```

이는 "코드 탐험" 후 원래 작업 위치로 돌아올 때 `Alt+Left`보다 더 정확합니다.

### 줄 편집: 코드 조작의 예술

줄 단위 편집은 리팩토링과 코드 재구성의 핵심입니다. 마우스로 줄을 선택하고 잘라내고 붙여넣는 대신, 전용 단축키를 사용하면 코드 구조를 빠르게 변경할 수 있습니다.

```
Ctrl+Shift+K (Cmd+Shift+K)     현재 줄 삭제
Alt+Up (Option+Up)             현재 줄을 위로 이동
Alt+Down (Option+Down)         현재 줄을 아래로 이동
Shift+Alt+Up (Shift+Option+Up)   현재 줄 위에 복사
Shift+Alt+Down (Shift+Option+Down) 현재 줄 아래에 복사
Ctrl+Enter (Cmd+Enter)         아래에 새 줄 삽입 (커서 위치 유지)
Ctrl+Shift+Enter (Cmd+Shift+Enter) 위에 새 줄 삽입
Ctrl+] (Cmd+])                 줄 들여쓰기
Ctrl+[ (Cmd+[)                 줄 내어쓰기
Ctrl+Shift+\\ (Cmd+Shift+\\)   일치하는 괄호로 이동
```

**줄 이동의 강력함: Alt+Up/Down**

이 단축키는 C# 개발에서 특히 유용합니다:

**예제 1: 메서드 순서 재배치**

C# 코딩 규약에서는 메서드를 특정 순서로 배치하는 경우가 많습니다(예: public → private, 생성자 → 메서드). `Alt+Up/Down`을 사용하면 마우스 없이 메서드 전체를 이동할 수 있습니다:

```csharp
public class OrderProcessor
{
    // private 메서드가 public 메서드 위에 있음 (규약 위반)
    private bool ValidateOrder(Order order)
    {
        return order != null && order.Items.Any();
    }

    public void ProcessOrder(Order order)  // 커서가 이 줄에 있음
    {
        if (ValidateOrder(order))
        {
            // 처리 로직
        }
    }
}
```

`ProcessOrder` 메서드의 첫 줄에 커서를 두고 `Alt+Up`을 반복하면 메서드 전체가 위로 이동합니다. VS Code는 지능적으로 전체 메서드 블록을 인식하여 이동합니다.

**예제 2: using 문 정리**

```csharp
using System.Linq;        // 커서가 여기
using System;
using System.Collections.Generic;

// Alt+Down을 눌러 알파벳 순으로 정렬
using System;
using System.Collections.Generic;
using System.Linq;
```

**줄 복사의 효율성: Shift+Alt+Down**

반복적인 코드 패턴을 작성할 때 매우 유용합니다:

```csharp
// 속성 정의 시
public string FirstName { get; set; }  // 이 줄에서 Shift+Alt+Down
public string FirstName { get; set; }  // 복사됨

// 두 번째 줄을 수정하여:
public string LastName { get; set; }

// 유사한 초기화 코드
_logger = logger ?? throw new ArgumentNullException(nameof(logger));
// Shift+Alt+Down으로 복사 후 수정
_repository = repository ?? throw new ArgumentNullException(nameof(repository));
```

**줄 삭제의 정확성: Ctrl+Shift+K**

`Ctrl+X`와 달리 `Ctrl+Shift+K`는 삭제된 내용을 클립보드에 넣지 않으므로, 클립보드 내용을 보존하면서 줄을 제거할 수 있습니다. 이는 다음과 같은 워크플로우에서 중요합니다:

```csharp
// 1. 중요한 코드를 Ctrl+C로 복사
var importantData = CalculateImportantValue();

// 2. 디버그 코드 줄들을 Ctrl+Shift+K로 삭제
Console.WriteLine("Debug 1");  // Ctrl+Shift+K
Console.WriteLine("Debug 2");  // Ctrl+Shift+K

// 3. 복사한 중요한 코드를 Ctrl+V로 붙여넣기
// 클립보드가 여전히 importantData 줄을 갖고 있음
```

**줄 삽입의 컨텍스트 인식: Ctrl+Enter**

`Ctrl+Enter`는 현재 커서 위치와 관계없이 줄 끝에 새 줄을 추가합니다. 이는 세미콜론을 추가하거나 새 문장을 시작할 때 유용합니다:

```csharp
public void Example()
{
    var x = 10;  // 커서가 줄 중간에 있어도
    // Ctrl+Enter를 누르면
    var x = 10;
    |  // 새 줄이 아래에 생성되고 커서가 여기로 이동
}
```

반대로 `Ctrl+Shift+Enter`는 위에 줄을 삽입합니다. 이는 메서드 위에 주석이나 특성을 추가할 때 유용합니다:

```csharp
public void ProcessOrder(Order order)  // 커서가 여기
// Ctrl+Shift+Enter
|  // 새 줄이 위에 생성
public void ProcessOrder(Order order)

// [Authorize] 특성 추가
[Authorize]
public void ProcessOrder(Order order)
```

**일치하는 괄호로 이동: Ctrl+Shift+\\**

C# 코드에서 중괄호, 대괄호, 소괄호는 코드 구조의 핵심입니다. `Ctrl+Shift+\\`는 현재 커서 위치의 괄호에 대응하는 닫는/여는 괄호로 즉시 이동합니다:

```csharp
public class Example
{  // 커서가 여기에 있으면 Ctrl+Shift+\를 누르면
    public void Method()
    {
        if (condition)
        {
            // 중첩된 블록
        }
    }
}  // 여기로 이동 (클래스의 닫는 중괄호)
```

이는 대형 메서드나 클래스의 범위를 빠르게 확인하거나, 블록의 끝에서 시작으로 돌아갈 때 유용합니다.

### 선택과 다중 커서: 동시 편집의 혁명

다중 커서(Multiple Cursors)는 Sublime Text가 2011년에 대중화한 혁명적 기능으로, 여러 위치에서 동시에 편집 작업을 수행할 수 있게 합니다. 이는 반복적인 편집 작업을 마치 "시간을 멈추고 여러 곳을 동시에 수정하는 것"처럼 만들어, 개발자의 생산성을 비약적으로 향상시켰습니다. Steve Klabnik(Rust 핵심 개발자)은 "다중 커서를 배운 후 이전 방식으로 돌아갈 수 없다"고 말했습니다.

```
Shift+Left/Right (Shift+←/→)     문자 단위 선택
Shift+Up/Down (Shift+↑/↓)        줄 단위 선택
Ctrl+Shift+Left/Right            단어 단위 선택
Ctrl+A (Cmd+A)                   전체 선택
Ctrl+L (Cmd+L)                   현재 줄 선택 (반복 시 다음 줄 추가)
Ctrl+D (Cmd+D)                   다음 일치 항목 선택 추가
Ctrl+K Ctrl+D                    마지막 선택 건너뛰기
Ctrl+Shift+L (Cmd+Shift+L)       모든 일치 항목에 커서 생성
Alt+Shift+I (Option+Shift+I)     선택한 줄 끝에 커서 생성
Alt+Click (Option+Click)         클릭 위치에 커서 추가
Ctrl+Alt+Up/Down                 위/아래에 커서 추가
Esc                              다중 커서 해제
```

**선택의 계층 구조:**

VS Code의 선택 시스템은 계층적으로 설계되어 있습니다:

1. **문자 단위** (`Shift+Left/Right`): 가장 세밀한 제어
2. **단어 단위** (`Ctrl+Shift+Left/Right`): 식별자 경계에서 멈춤
3. **줄 단위** (`Ctrl+L`): 전체 줄 선택, 반복 시 여러 줄
4. **블록 단위** (`Ctrl+Shift+\\`로 괄호 매칭): 코드 블록 전체

**실무 시나리오 1: 변수 이름 변경 (리팩토링 미리보기)**

정식 리팩토링 도구(`F2`)를 사용하기 전에, 다중 커서로 빠르게 시뮬레이션할 수 있습니다:

```csharp
public class OrderService
{
    private readonly ILogger _log;  // 'log'를 'logger'로 변경하고 싶음
    
    public OrderService(ILogger log)
    {
        _log = log ?? throw new ArgumentNullException(nameof(log));
    }
    
    public void ProcessOrder(Order order)
    {
        _log.LogInformation("Processing order");
        // log 사용...
    }
}
```

**작업 순서:**
1. `_log` 중 하나에 커서 위치
2. `Ctrl+D` 세 번 눌러 모든 `log` 인스턴스 선택
3. `logger` 타이핑하여 모든 위치를 동시에 변경

**주의**: 이는 빠른 편집에 유용하지만, 정식 Rename 리팩토링(`F2`)은 범위, 주석, 문자열 리터럴을 지능적으로 처리하므로 중요한 변경에는 `F2`를 사용해야 합니다.

**실무 시나리오 2: 생성자 매개변수에서 필드 생성**

```csharp
public class CustomerService
{
    // 필드를 여기에 생성하고 싶음
    
    public CustomerService(
        ICustomerRepository repository,
        ILogger<CustomerService> logger,
        IMapper mapper,
        IValidator<Customer> validator)
    {
        // 매개변수를 필드에 할당하고 싶음
    }
}
```

**작업 순서:**
1. 매개변수 줄들 전체 선택 (첫 줄에서 `Shift+Down` 반복)
2. `Alt+Shift+I`로 각 줄 끝에 커서 생성
3. `Home`으로 모든 커서를 줄 시작으로 이동
4. `private readonly ` 타이핑 (모든 줄에 동시 입력)
5. `Ctrl+Right`로 타입 이름으로 이동
6. `End`로 줄 끝으로, `;` 추가

**실무 시나리오 3: 여러 줄에 동일한 접두사/접미사 추가**

```csharp
// 여러 문자열을 한 번에 nameof()로 감싸기
throw new ArgumentNullException(logger);
throw new ArgumentNullException(repository);
throw new ArgumentNullException(mapper);
throw new ArgumentNullException(validator);

// 목표:
throw new ArgumentNullException(nameof(logger));
throw new ArgumentNullException(nameof(repository));
throw new ArgumentNullException(nameof(mapper));
throw new ArgumentNullException(nameof(validator));
```

**작업 순서:**
1. 첫 번째 변수명에 커서
2. `Ctrl+D` 네 번으로 모든 변수명 선택
3. `Ctrl+Right`로 단어 끝으로 이동
4. `)` 타이핑 (모든 위치에 입력)
5. `Ctrl+Left`로 단어 시작으로 이동
6. `nameof(` 타이핑

**다중 커서의 고급 기법: 선택 건너뛰기**

때로는 패턴의 일부만 선택하고 싶을 때가 있습니다:

```csharp
var x = 10;  // 변경하고 싶지 않음
var y = 20;  // 변경하고 싶음
var x = 30;  // 변경하고 싶지 않음
var z = 40;  // 변경하고 싶음
```

**작업 순서:**
1. `var` 선택
2. `Ctrl+D` 네 번으로 모든 `var` 선택
3. 첫 번째와 세 번째 선택을 건너뛰고 싶음
4. 각 위치에서 `Ctrl+K Ctrl+D`로 선택 해제
5. 이제 두 번째와 네 번째만 선택된 상태

**박스 선택 (열 모드 선택):**

`Shift+Alt+드래그`로 직사각형 영역을 선택할 수 있습니다. 이는 CSV 데이터나 정렬된 코드 블록에서 유용합니다:

```csharp
// 중간 열만 선택하고 싶음
public int    Id       { get; set; }
public string Name     { get; set; }
public int    Age      { get; set; }
public bool   IsActive { get; set; }

// Shift+Alt+드래그로 타입 열만 선택 가능
```

**선택 확장의 지능형 메커니즘:**

VS Code는 C# 문법을 이해하여 지능적으로 선택을 확장합니다:

```
Alt+Shift+Right (Option+Shift+→)    선택 확장 (Expand Selection)
Alt+Shift+Left (Option+Shift+←)     선택 축소 (Shrink Selection)
```

예제:
```csharp
public void ProcessOrder(Order order)
{
    var total = order.Items.Sum(i => i.Price);
    //                              ^ 커서가 여기
}

// Alt+Shift+Right를 반복적으로 누르면:
// 1. i.Price
// 2. (i => i.Price)
// 3. order.Items.Sum(i => i.Price)
// 4. var total = order.Items.Sum(i => i.Price);
// 5. 전체 메서드 본문
// 6. 전체 메서드
```

이는 Treesitter 파서를 사용하여 구문 트리를 기반으로 선택을 확장하므로, 단순히 괄호 매칭보다 훨씬 지능적입니다.

### 검색과 바꾸기: 정보 검색의 예술

코드베이스에서 정보를 찾는 능력은 개발자의 핵심 역량입니다. Fred Brooks는 "프로그래밍의 본질은 기존 코드를 읽고 이해하는 것"이라고 했습니다. VS Code의 검색 시스템은 Ripgrep(rg)이라는 매우 빠른 검색 엔진을 사용하여, 수백만 줄의 코드에서도 밀리초 단위로 결과를 반환합니다.

```
Ctrl+F (Cmd+F)                      현재 파일 검색
Ctrl+H (Cmd+H)                      현재 파일 검색 및 바꾸기
Ctrl+Shift+F (Cmd+Shift+F)          전체 파일 검색
Ctrl+Shift+H (Cmd+Shift+H)          전체 파일 검색 및 바꾸기
F3 (Cmd+G)                          다음 찾기
Shift+F3 (Cmd+Shift+G)              이전 찾기
Ctrl+F3                             현재 선택 항목 찾기
Alt+Enter                           모든 일치 항목 선택
Alt+C                               대소문자 구분 토글
Alt+W                               단어 단위 일치 토글
Alt+R                               정규식 모드 토글
Esc                                 검색 상자 닫기
```

**검색의 세 가지 모드:**

VS Code는 세 가지 검색 모드를 제공합니다:

1. **일반 텍스트 검색** (기본값): 입력한 문자 그대로 검색
2. **단어 단위 검색** (`Alt+W`): 식별자 경계에서만 일치
3. **정규식 검색** (`Alt+R`): 패턴 기반 고급 검색

**단어 단위 검색의 중요성:**

C# 코드에서 `log`를 검색하면 다음이 모두 일치합니다:
- `_log` (필드)
- `logger` (변수)
- `LogInformation` (메서드)
- `"Logging completed"` (문자열)

`Alt+W`를 활성화하면 정확히 `log`라는 단어만 일치합니다.

**정규식 검색의 강력함:**

정규식은 패턴 기반 검색의 핵심입니다. C# 개발에서 자주 사용하는 패턴:

**예제 1: 모든 public 메서드 찾기**
```regex
public\s+\w+\s+\w+\(
```
설명: "public" + 공백 + 타입명 + 공백 + 메서드명 + "("

**예제 2: 모든 숫자 리터럴 찾기**
```regex
\b\d+\b
```
설명: 단어 경계 내의 숫자들

**예제 3: TODO 주석과 작성자 찾기**
```regex
//\s*TODO\s*\((\w+)\):(.+)
```
캡처 그룹으로 작성자와 내용을 추출

**예제 4: 미사용 private 필드 찾기 (패턴 조합)**
```regex
private\s+readonly\s+\w+\s+_(\w+);
```
결과에서 `Ctrl+Shift+F`로 `_필드명`을 다시 검색하여 사용 빈도 확인

**바꾸기의 고급 기능: 캡처 그룹 활용**

정규식 캡처 그룹을 바꾸기 문자열에서 재사용할 수 있습니다:

**예제: 로깅 패턴 현대화**

```csharp
// 변경 전:
_logger.LogInformation("User {0} logged in at {1}", userId, DateTime.Now);
_logger.LogError("Error {0} occurred in {1}", errorCode, methodName);

// 찾기 패턴:
LogInformation\("(.+)", (\w+), (\w+)\)

// 바꾸기 패턴:
LogInformation($"$1", $2, $3)

// 변경 후:
_logger.LogInformation($"User {userId} logged in at {DateTime.Now}");
_logger.LogError($"Error {errorCode} occurred in {methodName}");
```

**$1, $2, $3**는 각각 첫 번째, 두 번째, 세 번째 캡처 그룹을 참조합니다.

**전체 파일 검색의 효율성:**

`Ctrl+Shift+F`는 프로젝트 전체를 검색하며, 다음 옵션을 제공합니다:

- **포함할 파일**: `*.cs`로 C# 파일만 검색
- **제외할 파일**: `**/bin/**, **/obj/**`로 빌드 결과 제외
- **파일 이름 검색**: 검색 상자 오른쪽의 "..." 메뉴

**실무 예제: API 사용법 찾기**

특정 NuGet 패키지를 제거하기 전에 사용 위치를 모두 찾아야 할 때:

```
검색: using Newtonsoft.Json
포함: src/**/*.cs
제외: **/bin/**, **/obj/**
```

결과는 트리 구조로 표시되며, 각 파일의 일치 항목을 클릭하여 즉시 이동할 수 있습니다.

**검색 결과 편집:**

검색 결과 패널에서 `Ctrl+Shift+L`을 누르면 모든 일치 항목에 다중 커서가 생성되어, 검색 없이 일괄 편집이 가능합니다.

**검색 히스토리와 즐겨찾기:**

VS Code는 최근 검색어를 기억합니다. 검색 상자에서 `↓`/`↑`로 히스토리를 탐색할 수 있습니다. 자주 사용하는 검색 패턴은 `.vscode/settings.json`에 저장하여 재사용할 수 있습니다.

### 들여쓰기와 포맷팅: 코드 미학의 자동화

"코드는 기계가 실행하기 위해 작성되지만, 인간이 읽기 위해 존재한다"는 Martin Fowler의 명언처럼, 일관된 포맷팅은 코드 가독성의 핵심입니다. VS Code는 EditorConfig, .editorconfig 파일, 그리고 언어별 포맷터를 통합하여 팀 전체가 일관된 코드 스타일을 유지할 수 있게 합니다.

```
Tab                              들여쓰기
Shift+Tab                        내어쓰기
Ctrl+] (Cmd+])                   줄 들여쓰기 증가
Ctrl+[ (Cmd+[)                   줄 들여쓰기 감소
Shift+Alt+F (Shift+Option+F)     문서 전체 포맷팅
Ctrl+K Ctrl+F (Cmd+K Cmd+F)      선택 영역 포맷팅
Ctrl+K Ctrl+X                    후행 공백 제거
```

**들여쓰기의 철학: Tabs vs Spaces**

컴퓨터 과학 역사상 가장 오래된 논쟁 중 하나입니다. VS Code는 두 진영 모두를 지원하며, 파일별로 자동 감지합니다:

```json
{
  "editor.insertSpaces": true,      // 스페이스 사용
  "editor.tabSize": 4,               // C# 표준: 4 스페이스
  "editor.detectIndentation": true   // 파일 내용에서 자동 감지
}
```

**C# 포맷팅 표준:**

Microsoft의 공식 C# 코딩 규약은 다음을 권장합니다:
- 들여쓰기: 4 스페이스
- 중괄호: Allman 스타일 (새 줄에 중괄호)
- 한 줄 최대 길이: 120자 (논쟁적이지만 4K 모니터 시대의 경향)

**자동 포맷팅의 강력함:**

`Shift+Alt+F`는 OmniSharp C# 언어 서버를 통해 다음 작업을 수행합니다:

1. **들여쓰기 정규화**: 탭/스페이스를 설정에 맞게 통일
2. **중괄호 배치**: Allman 또는 K&R 스타일로 자동 조정
3. **공백 정리**: 연산자 주변, 쉼표 뒤, 괄호 안쪽 공백 규칙 적용
4. **줄 바꿈**: 긴 줄을 적절히 분할 (설정 가능)
5. **using 문 정리**: 알파벳 순 정렬 및 미사용 제거

**실무 예제: 복사한 코드 정리**

Stack Overflow에서 복사한 코드는 들여쓰기가 맞지 않는 경우가 많습니다:

```csharp
// 복사한 코드 (들여쓰기 엉망)
public class Example{
public void Method(){
if(condition){
DoSomething();
}
}
}

// Shift+Alt+F 실행 후:
public class Example
{
    public void Method()
    {
        if (condition)
        {
            DoSomething();
        }
    }
}
```

**선택 영역 포맷팅의 정밀함:**

전체 파일을 포맷하면 Git diff가 방대해질 수 있습니다. `Ctrl+K Ctrl+F`는 선택한 부분만 포맷하여 변경 범위를 최소화합니다:

```csharp
public void ProcessOrder(Order order)
{
    // 이 부분만 선택하여 포맷
    var items=order.Items.Where(i=>i.IsActive).Select(i=>new{
        i.Name,i.Price,Tax=i.Price*0.1m
    });
    // 선택 영역만 깔끔하게 정리됨
}
```

**저장 시 자동 포맷:**

가장 생산적인 워크플로우는 저장할 때 자동으로 포맷하는 것입니다:

```json
{
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,      // 붙여넣기 시에도
  "editor.formatOnType": true,       // 세미콜론, 닫는 중괄호 입력 시
  "[csharp]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.organizeImports": true  // using 문 자동 정리
    }
  }
}
```

**EditorConfig 통합:**

팀 프로젝트에서는 `.editorconfig` 파일로 포맷팅 규칙을 공유합니다:

```ini
# .editorconfig
root = true

[*.cs]
indent_style = space
indent_size = 4
end_of_line = crlf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

# C# 특화 규칙
csharp_new_line_before_open_brace = all
csharp_indent_case_contents = true
csharp_space_after_cast = false
```

VS Code는 이 파일을 자동으로 인식하여 포맷터 설정에 적용합니다.

**후행 공백 정리의 중요성:**

후행 공백(Trailing Whitespace)은 Git diff를 오염시키고 코드 리뷰를 방해합니다. `Ctrl+K Ctrl+X`로 수동 제거하거나, 설정으로 자동화:

```json
{
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true  // POSIX 표준 준수
}
```

### 주석: 코드 문서화의 예술

주석은 "왜"를 설명하는 도구입니다. Donald Knuth의 "Literate Programming" 철학에서, 프로그램은 컴퓨터를 위한 명령이 아니라 인간을 위한 문학이어야 합니다. 하지만 Robert Martin("Clean Code" 저자)은 "좋은 코드는 스스로 설명한다"며 과도한 주석을 경계했습니다. VS Code는 두 접근 모두를 지원합니다.

```
Ctrl+/ (Cmd+/)                   줄 주석 토글 (//)
Shift+Alt+A (Shift+Option+A)     블록 주석 토글 (/* */)
Ctrl+K Ctrl+C                    선택 영역 주석 처리
Ctrl+K Ctrl+U                    선택 영역 주석 해제
```

**줄 주석의 지능적 처리:**

`Ctrl+/`는 현재 언어(C#, XML, JSON 등)를 인식하여 적절한 주석 문법을 사용합니다:

```csharp
var x = 10;  // Ctrl+/ 누르면
// var x = 10;  // 주석이 줄 시작에 추가됨

// 선택 영역에 Ctrl+/
var a = 1;
var b = 2;
var c = 3;

// 결과:
// var a = 1;
// var b = 2;
// var c = 3;

// 다시 Ctrl+/하면 주석 해제 (토글)
```

**블록 주석의 활용:**

여러 줄을 일시적으로 비활성화할 때 블록 주석이 유용합니다:

```csharp
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    
    /* 디버깅을 위해 일시적으로 비활성화
    CalculateTotal(order);
    ApplyDiscounts(order);
    */
    
    SaveOrder(order);
}
```

**C# XML 문서 주석 (/// 주석):**

C#에서는 `///`으로 시작하는 XML 문서 주석을 사용하여 IntelliSense에 표시되는 문서를 작성합니다. VS Code는 이를 자동으로 생성하는 스니펫을 제공합니다:

```csharp
/// <summary>
/// 주문을 처리합니다.
/// </summary>
/// <param name="order">처리할 주문 객체</param>
/// <returns>처리된 주문 ID</returns>
/// <exception cref="ArgumentNullException">order가 null인 경우</exception>
public int ProcessOrder(Order order)
{
    // 구현...
}
```

**자동 생성 방법:**
1. 메서드 위에 `///` 타이핑
2. VS Code가 자동으로 XML 템플릿 생성
3. Tab으로 각 필드를 순회하며 작성

**주석 베스트 프랙티스:**

**좋은 주석:**
```csharp
// 레거시 API와의 호환성을 위해 Unix 타임스탬프 사용
var timestamp = DateTimeOffset.UtcNow.ToUnixTimeSeconds();

// TODO: V2 API로 마이그레이션 후 DateTime으로 변경 (Issue #123)
```

**나쁜 주석 (불필요):**
```csharp
// x에 10을 할당
var x = 10;  // 코드 자체가 명확함

// 고객을 가져옴
var customer = GetCustomer();  // 메서드 이름이 충분히 설명적
```

**주석 하이라이팅:**

VS Code는 특정 키워드를 강조 표시합니다:
- `TODO:` - 미완성 작업
- `FIXME:` - 버그 수정 필요
- `HACK:` - 임시 해결책
- `NOTE:` - 중요한 주의사항

확장 프로그램 "Better Comments"를 설치하면 더 풍부한 주석 스타일을 사용할 수 있습니다.

---

## C.2 코드 탐색: 대규모 코드베이스 정복하기

"코드를 읽는 시간 대 쓰는 시간의 비율은 10:1을 넘는다"는 Robert C. Martin의 관찰은 모든 개발자의 경험과 일치합니다. 실제로 소프트웨어 공학 연구에 따르면, 개발자는 시간의 70%를 코드 읽기와 이해에, 20%를 수정에, 10%를 새 코드 작성에 사용합니다. 따라서 효율적인 코드 탐색은 생산성의 핵심입니다.

VS Code의 코드 탐색 기능은 Language Server Protocol(LSP)을 기반으로 합니다. LSP는 Microsoft가 2016년에 표준화한 프로토콜로, 편집기와 언어 서버를 분리하여 어떤 편집기에서든 동일한 언어 기능을 사용할 수 있게 합니다. C#의 경우 OmniSharp 또는 새로운 Roslyn 기반 언어 서버가 이 역할을 수행하며, 코드 분석, IntelliSense, 리팩토링을 제공합니다.

### 심볼 탐색: 코드 구조의 빠른 이해

프로그래밍에서 "심볼(Symbol)"은 클래스, 메서드, 속성, 변수 등 식별자를 의미합니다. VS Code는 Ctags나 Cscope 같은 전통적인 도구를 넘어, 실시간 구문 분석을 통해 정확하고 빠른 심볼 탐색을 제공합니다.

```
Ctrl+Shift+O (Cmd+Shift+O)        현재 파일의 심볼로 이동
Ctrl+T (Cmd+T)                    작업 영역 전체에서 심볼 검색
Ctrl+G (Cmd+G)                    줄 번호로 이동
Ctrl+P (Cmd+P)                    빠른 파일/심볼 열기
Ctrl+P, @                         현재 파일 심볼 (Ctrl+Shift+O와 동일)
Ctrl+P, #                         작업 영역 심볼 (Ctrl+T와 동일)
Ctrl+P, :                         줄 번호 (Ctrl+G와 동일)
```

**파일 내 심볼 탐색의 실용성:**

대형 C# 클래스는 수백 줄에 달할 수 있습니다. `Ctrl+Shift+O`는 이를 계층적으로 표시합니다:

```csharp
public class CustomerService
{
    // 50개 이상의 메서드와 속성...
}
```

`Ctrl+Shift+O` 실행 시:
```
@  GetCustomer
@  CreateCustomer  
@  UpdateCustomer
@  DeleteCustomer
@  ValidateCustomer
...
```

추가 기능:
- `:` 입력으로 카테고리별 그룹화 (메서드, 속성, 필드)
- 퍼지 매칭으로 `cust`만 입력해도 `CreateCustomer` 찾기
- 화살표 키로 미리보기, Enter로 이동

**작업 영역 전체 심볼 검색:**

`Ctrl+T`는 프로젝트의 모든 파일에서 심볼을 검색합니다. 이는 다음 시나리오에서 강력합니다:

**시나리오 1: 인터페이스 구현 찾기**
```
검색: ICustomerRepository
결과:
  CustomerRepository.cs         (구현)
  MockCustomerRepository.cs     (테스트 목)
  ICustomerRepository.cs        (인터페이스 정의)
```

**시나리오 2: 특정 패턴 클래스 찾기**
```
검색: *Factory
결과:
  CustomerFactory
  OrderFactory
  ProductFactory
```

**줄 번호 이동의 정확성:**

`Ctrl+G`는 디버깅 스택 트레이스에서 특정 줄로 빠르게 이동할 때 필수입니다:

```
Exception in CustomerService.ProcessOrder() at line 127
```

`Ctrl+G` → `127` → Enter로 즉시 해당 줄로 이동합니다.

**고급 기능: 상대 줄 이동**

`Ctrl+G`에서 `+10` 또는 `-5`를 입력하면 현재 위치에서 상대적으로 이동합니다. 이는 "10줄 아래 확인" 같은 리뷰 코멘트에 유용합니다.

### 정의와 참조: 코드 연결 고리 추적

소프트웨어는 상호 연결된 구성 요소의 네트워크입니다. "정의로 이동"과 "참조 찾기"는 이 네트워크를 탐색하는 핵심 도구입니다. 이 기능들은 Smalltalk의 "Message Browser"와 IntelliJ의 "Find Usages"에서 영감을 받았습니다.

```
F12                               정의로 이동 (Go to Definition)
Ctrl+Click (Cmd+Click)            정의로 이동 (마우스)
Alt+F12 (Option+F12)              정의 미리보기 (Peek Definition)
Ctrl+K F12 (Cmd+K F12)            정의를 옆에 열기
Shift+F12                         참조 찾기 (Find All References)
Shift+Alt+F12 (Shift+Option+F12)  참조 미리보기 (Peek References)
Ctrl+K Ctrl+T                     타입 정의로 이동
Alt+Shift+F12                     구현으로 이동 (Go to Implementation)
```

**정의로 이동의 지능성:**

`F12`는 컨텍스트를 이해합니다:

```csharp
public class OrderController
{
    private readonly IOrderService _orderService;
    
    public OrderController(IOrderService orderService)
    //                     ^ F12: 인터페이스 정의로
    {
        _orderService = orderService;
        //              ^ F12: 매개변수 정의로
    }
    
    public IActionResult GetOrder(int id)
    //                            ^ F12: int 타입 정의로 (메타데이터)
    {
        var order = _orderService.GetById(id);
        //          ^ F12: GetById 메서드 정의로
        return Ok(order);
        //     ^ F12: Ok 메서드 정의로 (컨트롤러 베이스)
    }
}
```

**Peek Definition의 혁명:**

`Alt+F12`는 코드 탐색의 게임 체인저입니다. 별도 탭으로 열지 않고 인라인 팝업으로 정의를 표시하여, 컨텍스트를 유지하면서 정보를 확인할 수 있습니다.

```csharp
public void ProcessOrder(Order order)
{
    _validator.Validate(order);
    //          ^ Alt+F12로 미리보기
    
    /*
    ┌─ IOrderValidator.cs ─────────────────┐
    │ public interface IOrderValidator     │
    │ {                                     │
    │     bool Validate(Order order);      │
    │ }                                     │
    └───────────────────────────────────────┘
    */
    
    // 팝업 내에서 F12를 눌러 전체 파일로 전환 가능
    // Esc로 팝업 닫기
}
```

Peek 창 내에서도 편집 가능하며, 변경 사항은 즉시 파일에 반영됩니다. 또한 Peek 창 내에서 다시 `Alt+F12`를 눌러 중첩 탐색도 가능합니다.

**타입 정의 vs 인터페이스 정의:**

`Ctrl+K Ctrl+T`는 변수의 런타임 타입이 아닌, 선언된 타입 정의로 이동합니다:

```csharp
IOrderService service = serviceProvider.GetService<IOrderService>();
//            ^ F12: IOrderService 인터페이스로
//                                  ^ Ctrl+K Ctrl+T: GetService<T> 메서드로
```

**구현 찾기: 다형성 탐색**

`Alt+Shift+F12`는 인터페이스나 추상 메서드의 모든 구현을 찾습니다:

```csharp
public interface INotificationService
{
    void SendNotification(string message);
    //   ^ Alt+Shift+F12
}

// 결과:
// - EmailNotificationService.SendNotification
// - SmsNotificationService.SendNotification
// - PushNotificationService.SendNotification
```

이는 "이 추상화를 실제로 구현한 곳은 어디인가?"라는 질문에 답합니다.

**참조 찾기의 컨텍스트 이해:**

`Shift+F12`는 단순히 텍스트 매칭이 아니라, 의미론적 참조를 찾습니다:

```csharp
public class Product
{
    public string Name { get; set; }  // 속성 정의
}

// Name에서 Shift+F12:
// - product.Name = "Test";     // 쓰기
// - var n = product.Name;       // 읽기
// - Console.WriteLine(product.Name);  // 읽기
// 주석이나 문자열의 "Name"은 제외됨
```

참조 결과는 다음과 같이 그룹화됩니다:
- 파일별
- 읽기/쓰기 구분
- 미리보기 컨텍스트 (주변 코드 몇 줄)

### 탐색 히스토리: 시간 여행

코드 탐색은 종종 "깊이 우선 탐색"처럼 진행됩니다. 여러 메서드를 거쳐 깊이 들어간 후, 원래 위치로 돌아와야 합니다. VS Code는 브라우저처럼 앞/뒤 탐색 히스토리를 유지합니다.

```
Alt+Left (Cmd+Left)               이전 편집 위치로
Alt+Right (Cmd+Right)             다음 편집 위치로
Ctrl+Tab                          최근 열린 파일 순환
Ctrl+Shift+Tab                    역순 순환
Ctrl+P, edt                       최근 편집한 파일 보기
```

**탐색 히스토리의 작동 원리:**

VS Code는 다음 이벤트를 히스토리에 기록합니다:
1. `F12`로 정의로 이동
2. 파일 간 전환
3. 검색 결과 클릭
4. 줄 번호로 이동
5. 북마크로 이동 (확장 기능)

**실무 시나리오:**

```
A.cs (줄 10) → F12 → B.cs (줄 50) → F12 → C.cs (줄 100)
```

이제 `Alt+Left`를 두 번 누르면 `A.cs` 줄 10으로 돌아갑니다. `Alt+Right`로 다시 앞으로 이동 가능합니다.

**최근 파일 빠른 전환:**

`Ctrl+Tab`은 MRU(Most Recently Used) 순서로 파일을 표시합니다. `Ctrl`을 누른 상태로 `Tab`을 반복하여 선택하고, `Ctrl`을 놓으면 해당 파일로 이동합니다.

이는 두 파일 간 빠른 전환에 특히 유용합니다:
- 테스트와 프로덕션 코드
- 인터페이스와 구현
- 컨트롤러와 서비스

### 코드 접기: 시야 관리의 기술

코드 접기(Code Folding)는 Emacs의 "Outline Mode"에서 시작된 개념으로, 관심 없는 코드를 일시적으로 숨겨 집중도를 높입니다. VS Code는 들여쓰기 기반과 구문 인식 접기를 모두 지원합니다.

```
Ctrl+Shift+[ (Cmd+Option+[)       현재 영역 접기
Ctrl+Shift+] (Cmd+Option+])       현재 영역 펼치기
Ctrl+K Ctrl+0 (Cmd+K Cmd+0)       모든 영역 접기 (레벨 0)
Ctrl+K Ctrl+J (Cmd+K Cmd+J)       모든 영역 펼치기
Ctrl+K Ctrl+[                     재귀적으로 접기
Ctrl+K Ctrl+]                     재귀적으로 펼치기
Ctrl+K Ctrl+1~5                   레벨 1~5까지 접기
Ctrl+K Ctrl+/                     모든 블록 주석 접기
```

**계층적 접기의 강력함:**

C# 코드는 자연스럽게 계층 구조를 갖습니다:

```
Level 1: 네임스페이스
  Level 2: 클래스
    Level 3: 메서드
      Level 4: 제어문 블록
        Level 5: 중첩된 블록
```

**실무 예제: 클래스 개요 파악**

```csharp
public class OrderService  // 접힘
{
    // 필드들...
    
    public OrderService(...) { }  // 접힘
    
    public Order GetOrder(int id) { }  // 접힘
    
    public void CreateOrder(Order order) { }  // 접힘
    
    private bool ValidateOrder(Order order) { }  // 접힘
}
```

`Ctrl+K Ctrl+1`을 누르면 메서드 본문만 접혀, 클래스의 public API를 한눈에 볼 수 있습니다.

**영역 마커를 이용한 커스텀 접기:**

C#에서는 `#region`으로 사용자 정의 접기 영역을 만들 수 있습니다:

```csharp
public class CustomerService
{
    #region Fields
    private readonly ILogger _logger;
    private readonly IRepository _repository;
    #endregion
    
    #region Constructors
    public CustomerService(ILogger logger, IRepository repository)
    {
        _logger = logger;
        _repository = repository;
    }
    #endregion
    
    #region Public Methods
    public void CreateCustomer(Customer customer) { }
    public Customer GetCustomer(int id) { }
    #endregion
    
    #region Private Methods
    private bool ValidateCustomer(Customer customer) { }
    #endregion
}
```

VS Code는 `#region`을 접기 지점으로 인식하며, 영역 이름을 접힌 텍스트에 표시합니다.

**주의**: `#region`은 논쟁적인 기능입니다. "Clean Code" 지지자들은 클래스가 region이 필요할 만큼 크다면 분할해야 한다고 주장합니다. 하지만 레거시 코드나 생성된 코드에서는 유용할 수 있습니다.

### 화면 레이아웃: 멀티태스킹 편집

현대 모니터는 넓어졌고, 개발자는 여러 파일을 동시에 봐야 합니다. VS Code의 편집기 그룹(Editor Group) 시스템은 Vim의 split 개념을 GUI로 구현한 것입니다.

```
Ctrl+\\ (Cmd+\\)                    편집기 분할 (수직)
Ctrl+K Ctrl+\\                      편집기 분할 (수평)
Ctrl+1/2/3 (Cmd+1/2/3)             편집기 그룹 1/2/3으로 포커스
Ctrl+K Ctrl+Left/Right             포커스를 좌/우 그룹으로
Ctrl+K Ctrl+Up/Down                포커스를 상/하 그룹으로
Ctrl+K Left/Right/Up/Down          현재 편집기를 그룹으로 이동
Alt+1/2/3                          탭 1/2/3으로 전환
Ctrl+W (Cmd+W)                     현재 편집기 닫기
Ctrl+K W                           현재 그룹의 모든 편집기 닫기
Ctrl+K Ctrl+W                      모든 편집기 닫기
```

**분할 편집의 실용성:**

**시나리오 1: 인터페이스와 구현 비교**

```
┌─────────────────┬─────────────────┐
│ IOrderService   │ OrderService    │
│                 │                 │
│ 메서드 시그니처  │ 구현            │
└─────────────────┴─────────────────┘
```

**시나리오 2: TDD (Test-Driven Development)**

```
┌─────────────────┬─────────────────┐
│ OrderTests.cs   │ OrderService.cs │
│                 │                 │
│ 테스트 작성     │ 구현 작성       │
└─────────────────┴─────────────────┘
```

**시나리오 3: 코드 리뷰**

```
┌───────────────────────────────────┐
│ CustomerService.cs (변경 전)      │
├───────────────────────────────────┤
│ CustomerService.cs (변경 후)      │
└───────────────────────────────────┘
```

수평 분할로 diff를 직접 비교합니다.

**편집기 그룹 이동의 효율성:**

파일을 다른 그룹으로 이동하려면:
1. `Ctrl+K Left/Right`로 드래그 없이 이동
2. 또는 탭을 마우스로 드래그하여 그룹 간 이동

**Grid 레이아웃:**

VS Code는 2x2, 3x3 등 그리드 레이아웃을 지원합니다:
```
View → Editor Layout → Grid (2x2)
```

이는 4개 파일을 동시에 보면서 마이크로서비스 간 상호작용을 분석할 때 유용합니다.

### 사이드바와 패널: 컨텍스트 전환

사이드바와 패널은 편집 영역을 보조하는 도구 창들입니다. 효율적인 전환은 워크플로우의 흐름을 유지하는 핵심입니다.

```
Ctrl+B (Cmd+B)                     사이드바 토글
Ctrl+J (Cmd+J)                     패널 토글
Ctrl+Shift+E (Cmd+Shift+E)         탐색기 (Explorer)
Ctrl+Shift+F (Cmd+Shift+F)         검색 (Search)
Ctrl+Shift+G (Cmd+Shift+G)         소스 제어 (Git)
Ctrl+Shift+D (Cmd+Shift+D)         디버그 (Debug)
Ctrl+Shift+X (Cmd+Shift+X)         확장 (Extensions)
Ctrl+` (Cmd+`)                     터미널 토글
Ctrl+Shift+M (Cmd+Shift+M)         문제 패널 (Problems)
Ctrl+Shift+U                       출력 패널 (Output)
```

**사이드바 최적화 전략:**

**전체 화면 코딩 모드:**
```bash
# 최대 코딩 영역 확보
Ctrl+B  # 사이드바 숨김
Ctrl+J  # 패널 숨김
F11     # 전체 화면

# Zen Mode (Ctrl+K Z):  사이드바, 패널, 탭 모두 숨김
```

**작업별 레이아웃:**

1. **코딩**: 사이드바 숨김, 터미널만 표시
2. **탐색**: 탐색기 + 검색 표시
3. **디버깅**: 디버그 패널 + 터미널
4. **Git 작업**: 소스 제어 + Diff 편집기

VS Code는 이러한 레이아웃을 자동으로 기억하여, 작업에 따라 최적화된 화면을 제공합니다.

---

## C.3 디버깅

디버깅은 소프트웨어 개발에서 필수적인 과정입니다. VS Code의 강력한 디버깅 도구와 단축키를 활용하면 버그를 빠르게 찾고 수정할 수 있습니다.

### 디버깅 시작 및 제어

```
F5                      디버깅 시작/계속
Ctrl+F5                 디버깅 없이 실행
Shift+F5                디버깅 중지
Ctrl+Shift+F5           디버깅 재시작
F10                     프로시저 단위 실행 (Step Over)
F11                     한 단계씩 코드 실행 (Step Into)
Shift+F11               프로시저 나가기 (Step Out)
```

**디버깅 워크플로우:**
1. 중단점 설정 (`F9`)
2. 디버깅 시작 (`F5`)
3. 중단점에서 멈추면 변수 검사
4. `F10`으로 줄 단위 실행하며 흐름 확인
5. 의심스러운 메서드는 `F11`로 진입
6. 메서드 내부가 문제없으면 `Shift+F11`로 빠져나오기

### 중단점 관리

```
F9                      중단점 토글
Ctrl+Shift+F9           모든 중단점 제거
Ctrl+K Ctrl+I           호버 정보 표시
```

**중단점 종류:**
- **일반 중단점**: 해당 줄에 도달하면 항상 중지
- **조건부 중단점**: 특정 조건이 참일 때만 중지 (중단점 우클릭 → 조건부 중단점 편집)
- **로그 중단점**: 중지하지 않고 메시지만 출력 (중단점 우클릭 → 로그 메시지)

**조건부 중단점 예시:**
```csharp
// 조건: i == 50
for (int i = 0; i < 100; i++)
{
    ProcessItem(i);  // 여기에 조건부 중단점 설정
}
```

### 디버깅 정보 확인

```
Ctrl+K Ctrl+I           빠른 정보 (호버와 동일)
```

**디버깅 패널:**
- **변수(Variables)**: 현재 스코프의 지역 변수와 전역 변수
- **조사식(Watch)**: 사용자가 지정한 표현식 감시
- **호출 스택(Call Stack)**: 현재 실행 경로 추적
- **중단점(Breakpoints)**: 설정된 모든 중단점 관리

**조사식 활용:**
- 복잡한 표현식의 결과를 계속 모니터링: `order.Items.Sum(x => x.Price)`
- 조건문 평가: `user != null && user.IsActive`

### 디버그 콘솔

디버그 콘솔에서는 실행 중인 프로그램의 컨텍스트에서 C# 표현식을 평가할 수 있습니다.

```
Ctrl+Shift+Y            디버그 콘솔 표시
```

**디버그 콘솔 활용 예시:**
```csharp
// 디버그 콘솔에서 직접 입력:
> myVariable
> myObject.Property
> SomeMethod(param1, param2)
> list.Count
> list.Where(x => x.IsActive).ToList()
```

**실무 팁:**
- 중단점에서 멈춘 상태에서 변수 값을 즉시 확인
- 임시로 메서드를 호출하여 결과 테스트
- 복잡한 LINQ 쿼리의 결과를 미리 확인

### 예외 처리

```
예외 발생 시 자동 중지 설정: 디버그 패널에서 "Uncaught Exceptions" 체크
```

**예외 중단점 설정:**
1. 실행 및 디버그 패널 열기 (`Ctrl+Shift+D`)
2. "BREAKPOINTS" 섹션에서 예외 중단점 추가
3. 특정 예외 타입 선택 또는 모든 예외에서 중단

---

## C.4 터미널

통합 터미널은 VS Code의 강력한 기능 중 하나로, 편집기를 떠나지 않고 명령줄 작업을 수행할 수 있습니다.

### 터미널 기본 조작

```
Ctrl+` (Cmd+`)          터미널 토글
Ctrl+Shift+`            새 터미널 생성
Ctrl+Shift+5            터미널 분할
```

**터미널 활용 시나리오:**
- .NET CLI 명령어 실행 (`dotnet build`, `dotnet run`)
- Git 명령어 실행
- NuGet 패키지 관리
- 프로젝트 스크립트 실행

### 터미널 탐색

```
Ctrl+Home               터미널 최상단으로 스크롤
Ctrl+End                터미널 최하단으로 스크롤
PageUp/PageDown         페이지 단위 스크롤
```

**터미널 히스토리:**
- 위/아래 화살표로 이전 명령어 탐색
- `Ctrl+R`로 명령어 히스토리 검색 (Bash/Zsh)

### 터미널과 편집기 간 전환

```
Ctrl+` (Cmd+`)          터미널과 편집기 간 포커스 토글
Ctrl+1                  첫 번째 편집기 그룹으로 포커스
```

**효율적인 워크플로우:**
1. 코드 편집 (`Ctrl+1`로 편집기 포커스)
2. `Ctrl+``로 터미널 열기
3. 빌드/실행 명령 실행
4. 결과 확인 후 `Ctrl+``로 다시 편집기로 돌아가기

### 자주 사용하는 .NET CLI 명령어

터미널에서 자주 사용하는 C# 개발 명령어:

```bash
# 프로젝트 생성 및 관리
dotnet new console -n MyApp
dotnet new classlib -n MyLibrary
dotnet sln add MyApp/MyApp.csproj

# 빌드 및 실행
dotnet build
dotnet run
dotnet run -- arg1 arg2

# 테스트
dotnet test
dotnet test --filter "Category=Unit"

# 패키지 관리
dotnet add package Newtonsoft.Json
dotnet restore
dotnet list package
```

**터미널 팁:**
- `clear` (Linux/macOS) 또는 `cls` (Windows)로 화면 정리
- `Ctrl+C`로 실행 중인 프로세스 중단
- `Ctrl+L`로 화면 정리 (대부분의 셸에서)

### 터미널 설정

VS Code는 다양한 셸을 지원합니다:
- Windows: PowerShell, Command Prompt, Git Bash, WSL
- macOS/Linux: bash, zsh, fish

**기본 셸 변경:**
1. 터미널 드롭다운 클릭
2. "Select Default Profile" 선택
3. 원하는 셸 선택

**터미널 자동화:**
- `.vscode/tasks.json`으로 반복 작업 자동화
- 예: 빌드, 테스트, 배포 작업 정의

---

## 추가 유용한 단축키

### 명령 팔레트

```
Ctrl+Shift+P (Cmd+Shift+P)  명령 팔레트 열기
F1                          명령 팔레트 열기 (Ctrl+Shift+P와 동일)
```

명령 팔레트는 VS Code의 모든 기능에 접근할 수 있는 중앙 허브입니다.

**자주 사용하는 명령:**
- "Format Document": 문서 포맷팅
- "Change Language Mode": 파일 언어 모드 변경
- "Reload Window": VS Code 창 새로고침
- "Open Settings (JSON)": 설정 파일 직접 편집

### 설정 및 기본 설정

```
Ctrl+, (Cmd+,)          설정 열기
Ctrl+K Ctrl+S           키보드 단축키 설정 열기
```

**사용자 정의 단축키:**
- 키보드 단축키 설정에서 기존 단축키 수정 또는 새로운 단축키 추가 가능
- JSON 형식으로 직접 편집 가능 (`keybindings.json`)

### IntelliSense 및 자동 완성

```
Ctrl+Space              IntelliSense 트리거
Ctrl+Shift+Space        매개변수 힌트 표시
Ctrl+. (Cmd+.)          빠른 수정 (Quick Fix) 표시
```

**C# IntelliSense 활용:**
- 타입 입력 시 자동으로 제안 표시
- `Ctrl+Space`로 수동 트리거
- `Tab` 또는 `Enter`로 선택 확정
- `Ctrl+.`로 using 문 자동 추가, 리팩토링 제안 등

### 코드 리팩토링

```
F2                      심볼 이름 바꾸기 (Rename)
Ctrl+. (Cmd+.)          빠른 수정 및 리팩토링
```

**리팩토링 작업:**
- 변수/메서드/클래스 이름 변경: `F2`
- 메서드 추출: 코드 선택 → `Ctrl+.` → "Extract Method"
- 인터페이스 추출: 클래스에서 `Ctrl+.` → "Extract Interface"

---

## 단축키 학습 팁

1. **점진적 학습**: 한 번에 모든 단축키를 외우려 하지 말고, 자주 사용하는 작업부터 단축키를 익히세요.

2. **근육 기억**: 단축키는 머리보다 손이 기억합니다. 의식적으로 반복 사용하세요.

3. **마우스 사용 줄이기**: 마우스로 할 수 있는 작업의 단축키를 찾아 사용하세요.

4. **키보드 단축키 참조**:
   - `Ctrl+K Ctrl+S`로 단축키 목록 확인
   - PDF 치트시트 활용: Help → Keyboard Shortcuts Reference

5. **자주 사용하는 작업 확인**: 명령 팔레트 (`Ctrl+Shift+P`)에서 최근 사용한 명령이 상단에 표시됩니다.

---

## 운영체제별 주요 차이점

### Windows/Linux vs macOS

| 작업 | Windows/Linux | macOS |
|------|---------------|-------|
| 복사 | Ctrl+C | Cmd+C |
| 붙여넣기 | Ctrl+V | Cmd+V |
| 저장 | Ctrl+S | Cmd+S |
| 찾기 | Ctrl+F | Cmd+F |
| 모두 선택 | Ctrl+A | Cmd+A |
| 실행 취소 | Ctrl+Z | Cmd+Z |
| 터미널 토글 | Ctrl+` | Cmd+` |

### 일반적인 패턴

- Windows/Linux의 `Ctrl`은 macOS의 `Cmd`에 해당
- Windows/Linux의 `Alt`는 macOS의 `Option`에 해당
- 일부 단축키는 플랫폼별로 다를 수 있으니 공식 문서를 참조하세요.

---

## 참고 자료

**공식 문서:**
- [VS Code 단축키 참조 (Windows)](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
- [VS Code 단축키 참조 (macOS)](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
- [VS Code 단축키 참조 (Linux)](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)

**학습 리소스:**
- [VS Code Tips and Tricks](https://code.visualstudio.com/docs/getstarted/tips-and-tricks)
- [VS Code Keyboard Shortcuts](https://code.visualstudio.com/docs/getstarted/keybindings)

**C# 개발 관련:**
- [C# in VS Code](https://code.visualstudio.com/docs/languages/csharp)
- [C# DevKit 확장](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)

---

## 마치며

단축키 숙달은 개발자의 생산성을 크게 향상시킵니다. 처음에는 익숙하지 않아 느리게 느껴질 수 있지만, 꾸준한 연습을 통해 자연스럽게 사용할 수 있게 됩니다. 이 부록에 나열된 단축키 중 자주 사용하는 것부터 하나씩 익혀나가세요. 

효율적인 코딩 환경을 만들어 나가는 과정 자체가 개발 역량을 키우는 중요한 부분입니다. VS Code의 강력한 기능과 단축키를 마스터하여, 코드를 작성하는 것보다 문제를 해결하는 것에 더 많은 시간을 투자할 수 있기를 바랍니다.
