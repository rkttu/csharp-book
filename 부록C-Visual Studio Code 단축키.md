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

### 선택 및 다중 커서

```
Shift+Left/Right        문자 단위 선택
Shift+Up/Down           줄 단위 선택
Ctrl+Shift+Left/Right   단어 단위 선택
Ctrl+A (Cmd+A)          전체 선택
Alt+Click               다중 커서 추가
Ctrl+Alt+Up/Down        위/아래에 커서 추가
Ctrl+D                  다음 일치 항목 선택
Ctrl+Shift+L            모든 일치 항목 선택
```

**다중 커서 활용 예시:**
1. 변수 이름 일괄 변경: 변수명 선택 → `Ctrl+D` 반복 → 동시 수정
2. 여러 줄에 동일한 텍스트 추가: `Ctrl+Alt+Down`으로 커서 생성 → 입력

### 검색 및 바꾸기

```
Ctrl+F (Cmd+F)          현재 파일에서 찾기
Ctrl+H (Cmd+H)          현재 파일에서 바꾸기
Ctrl+Shift+F (Cmd+Shift+F)  전체 파일에서 찾기
Ctrl+Shift+H (Cmd+Shift+H)  전체 파일에서 바꾸기
F3 (Cmd+G)              다음 찾기
Shift+F3 (Cmd+Shift+G)  이전 찾기
Alt+Enter               모든 일치 항목 선택
```

**정규식 활용:**
- 검색창에서 `.*` 아이콘을 클릭하거나 `Alt+R`로 정규식 모드를 활성화할 수 있습니다.
- 예: `\d+`로 모든 숫자 찾기, `[A-Z]\w+`로 대문자로 시작하는 단어 찾기

### 들여쓰기 및 포맷팅

```
Tab                     들여쓰기
Shift+Tab               내어쓰기
Ctrl+] (Cmd+])          줄 들여쓰기
Ctrl+[ (Cmd+[)          줄 내어쓰기
Shift+Alt+F             문서 포맷팅
Ctrl+K Ctrl+F           선택 영역 포맷팅
```

**C# 코드 포맷팅:**
- VS Code는 C# 확장을 통해 자동 포맷팅을 지원합니다.
- 저장 시 자동 포맷팅을 활성화하려면 설정에서 `"editor.formatOnSave": true`를 추가하세요.

### 주석 처리

```
Ctrl+/ (Cmd+/)          줄 주석 토글 (//)
Shift+Alt+A             블록 주석 토글 (/* */)
```

**주석 활용 팁:**
- 코드 블록을 선택하고 `Ctrl+/`를 누르면 선택한 모든 줄에 주석이 토글됩니다.
- 임시로 코드를 비활성화할 때 주석 처리가 유용합니다.

---

## C.2 코드 탐색

대규모 프로젝트에서는 코드 탐색 기능이 필수적입니다. VS Code는 C# 코드베이스를 효율적으로 탐색할 수 있는 다양한 기능을 제공합니다.

### 파일 및 심볼 탐색

```
Ctrl+P (Cmd+P)          빠른 파일 열기
Ctrl+Shift+O (Cmd+Shift+O)  현재 파일의 심볼로 이동
Ctrl+T (Cmd+T)          작업 영역의 심볼로 이동
Ctrl+G (Cmd+G)          줄 번호로 이동
```

**빠른 열기 고급 기능:**
- `Ctrl+P`에서 `@` 입력: 현재 파일의 심볼 목록 (`Ctrl+Shift+O`와 동일)
- `Ctrl+P`에서 `#` 입력: 작업 영역의 심볼 검색 (`Ctrl+T`와 유사)
- `Ctrl+P`에서 `:` 입력: 줄 번호로 이동 (`Ctrl+G`와 동일)

### 정의 및 참조

```
F12                     정의로 이동 (Go to Definition)
Alt+F12                 정의 미리보기 (Peek Definition)
Ctrl+K F12              정의를 옆에서 열기
Shift+F12               참조 찾기 (Find All References)
Shift+Alt+F12           참조 미리보기
```

**C# 개발에서 활용:**
- `F12`로 메서드나 클래스 정의로 빠르게 이동
- `Shift+F12`로 메서드가 어디서 호출되는지 확인
- `Alt+F12`로 정의를 팝업으로 확인하여 컨텍스트를 유지

### 편집기 탐색

```
Ctrl+Tab                최근 파일 간 전환
Alt+Left/Right (Cmd+Left/Right)  이전/다음 위치로 이동
Ctrl+M                  괄호 간 이동 (Toggle Tab Key Moves Focus)
Ctrl+Shift+\            일치하는 괄호로 이동
```

**탐색 히스토리:**
- VS Code는 커서 위치 히스토리를 기억합니다.
- `Alt+Left`로 이전 편집 위치로 돌아가고, `Alt+Right`로 앞으로 이동합니다.

### 코드 접기 및 펼치기

```
Ctrl+Shift+[ (Cmd+Option+[)  현재 영역 접기
Ctrl+Shift+] (Cmd+Option+])  현재 영역 펼치기
Ctrl+K Ctrl+0           모든 영역 접기
Ctrl+K Ctrl+J           모든 영역 펼치기
Ctrl+K Ctrl+[           재귀적으로 접기
Ctrl+K Ctrl+]           재귀적으로 펼치기
Ctrl+K Ctrl+1~5         레벨별 접기
```

**대형 클래스 탐색:**
- 메서드가 많은 클래스에서 `Ctrl+K Ctrl+1`로 첫 번째 레벨만 남기면 개요를 파악하기 쉽습니다.
- 특정 메서드에 집중할 때 다른 부분을 접으면 가독성이 향상됩니다.

### 사이드바 및 패널

```
Ctrl+B (Cmd+B)          사이드바 토글
Ctrl+Shift+E            탐색기 표시
Ctrl+Shift+F            검색 표시
Ctrl+Shift+G (Ctrl+Shift+G)  소스 제어 표시
Ctrl+Shift+D            실행 및 디버그 표시
Ctrl+Shift+X            확장 표시
Ctrl+` (Cmd+`)          통합 터미널 토글
```

**효율적인 화면 구성:**
- 코딩에 집중할 때: `Ctrl+B`로 사이드바를 숨겨 화면을 넓게 사용
- 파일 구조 확인: `Ctrl+Shift+E`로 탐색기 열기
- 터미널 작업: `Ctrl+``로 터미널을 빠르게 토글

### 에디터 레이아웃

```
Ctrl+\                  편집기 분할
Ctrl+1/2/3              편집기 그룹 간 포커스 이동
Ctrl+K Ctrl+Left/Right  편집기 그룹 간 포커스 이동
Ctrl+W                  편집기 닫기
Ctrl+K W                편집기 그룹 닫기
```

**분할 화면 활용:**
- 헤더 파일과 구현 파일을 동시에 볼 때 유용
- 테스트 코드와 프로덕션 코드를 나란히 배치
- `Ctrl+\`로 빠르게 화면 분할 후 `Ctrl+P`로 다른 파일 열기

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
