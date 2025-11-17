# 부록 C. Visual Studio Code 단축키

Visual Studio Code(VS Code)는 강력하면서도 가벼운 코드 편집기로, C# 개발에 최적화된 다양한 단축키를 제공합니다. 단축키를 숙지하면 마우스 사용을 최소화하고 개발 생산성을 크게 향상시킬 수 있습니다.

**단축키 표기법:**
- Windows/Linux: `Ctrl`, `Alt`, `Shift`
- macOS: `Cmd` (⌘), `Option` (⌥), `Shift` (⇧)
- 이 부록에서는 Windows/Linux 기준으로 표기하며, macOS는 괄호 안에 표시합니다.

---

## C.1 기본 편집

기본 편집 작업은 코딩의 가장 기본이 되는 작업으로, 효율적인 단축키 사용이 생산성에 직접적인 영향을 미칩니다.

### 파일 관리

```
Ctrl+N (Cmd+N)          새 파일 생성
Ctrl+O (Cmd+O)          파일 열기
Ctrl+S (Cmd+S)          파일 저장
Ctrl+Shift+S (Cmd+Shift+S)  다른 이름으로 저장
Ctrl+W (Cmd+W)          현재 편집기 탭 닫기
Ctrl+K Ctrl+W           모든 편집기 탭 닫기
```

**활용 팁:**
- `Ctrl+P`로 빠른 파일 열기 메뉴를 활용하면 프로젝트 내 파일을 빠르게 찾을 수 있습니다.
- `Ctrl+Tab`으로 최근 편집한 파일 간 전환이 가능합니다.

### 텍스트 편집

```
Ctrl+X                  줄 잘라내기 (선택 없을 시 현재 줄)
Ctrl+C                  줄 복사 (선택 없을 시 현재 줄)
Ctrl+V                  붙여넣기
Ctrl+Z (Cmd+Z)          실행 취소
Ctrl+Y (Cmd+Shift+Z)    다시 실행
```

### 줄 편집

```
Ctrl+Shift+K            현재 줄 삭제
Alt+Up/Down             현재 줄을 위/아래로 이동
Shift+Alt+Up/Down       현재 줄을 위/아래로 복사
Ctrl+Enter              아래에 새 줄 삽입
Ctrl+Shift+Enter        위에 새 줄 삽입
```

**실무 활용:**
- 코드 블록을 빠르게 재배치할 때 `Alt+Up/Down`을 사용하면 복사-붙여넣기-삭제보다 훨씬 빠릅니다.
- `Shift+Alt+Down`으로 유사한 코드 줄을 복제하여 수정하는 패턴이 효율적입니다.

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
