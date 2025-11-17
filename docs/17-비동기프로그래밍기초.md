# 17장. 비동기 프로그래밍 기초

비동기 프로그래밍(Asynchronous Programming)은 현대 소프트웨어 개발의 핵심 패러다임 중 하나로, 응답성(Responsiveness)과 확장성(Scalability)을 동시에 달성할 수 있게 해주는 강력한 메커니즘입니다. 웹 API 호출, 파일 I/O, 데이터베이스 쿼리 등 시간이 오래 걸리는 작업을 효율적으로 처리하면서도 애플리케이션이 다른 작업을 계속 수행할 수 있도록 합니다. 이는 단순한 프로그래밍 기법을 넘어, **동시성(Concurrency)**과 **병렬성(Parallelism)**의 개념을 실용적으로 구현하는 방법입니다.

**비동기 프로그래밍의 역사적 발전:**

비동기 프로그래밍의 개념은 1960년대 초 운영체제의 I/O 처리에서 시작되었습니다. 초기 컴퓨터 시스템에서는 CPU가 I/O 작업이 완료될 때까지 대기하는 것이 큰 자원 낭비였고, 이를 해결하기 위해 **인터럽트(Interrupt)** 기반의 비동기 I/O가 개발되었습니다. 1970년대에는 **이벤트 기반 프로그래밍(Event-driven Programming)**이 등장했으며, 1980년대 GUI 프로그래밍의 발전과 함께 **이벤트 루프(Event Loop)** 모델이 확립되었습니다.

프로그래밍 언어 차원에서 비동기 지원은 더디게 진행되었습니다. C와 C++에서는 **콜백(Callback)** 함수를 통해 비동기를 구현했지만, 이는 악명 높은 **콜백 지옥(Callback Hell)**을 야기했습니다. 1990년대 후반 JavaScript의 등장과 2000년대 Node.js의 인기로 비동기 프로그래밍이 주목받기 시작했고, **Promise**와 **Future** 패턴이 발전했습니다.

C#의 비동기 프로그래밍 여정은 다음과 같이 진화했습니다:

- **C# 1.0-4.0 (2002-2010)**: BeginInvoke/EndInvoke 패턴 (APM - Asynchronous Programming Model), BackgroundWorker
- **C# 5.0 (2012)**: `async`와 `await` 키워드 도입 - **혁명적 변화**
- **C# 7.0-8.0 (2017-2019)**: ValueTask, IAsyncEnumerable, async streams
- **C# 9.0-10.0 (2020-2021)**: 개선된 성능과 패턴 매칭

C# 5.0에서 도입된 `async`/`await`는 **Erik Meijer**, **Anders Hejlsberg** 등 Microsoft 연구진의 작업 결과로, **continuation-passing style (CPS)**와 **모나드(Monad)** 이론을 실용적으로 구현한 것입니다. 이는 F#의 **computation expressions**와 Haskell의 **do-notation**에서 영감을 받았으며, 복잡한 비동기 코드를 동기 코드처럼 읽기 쉽게 작성할 수 있게 만들었습니다.

**비동기 프로그래밍의 이론적 기반:**

비동기 프로그래밍은 다음과 같은 컴퓨터 과학의 개념들과 연결되어 있습니다:

1. **동시성(Concurrency)**: 여러 작업이 논리적으로 동시에 진행되는 것처럼 보이는 상태. Rob Pike는 "Concurrency is about dealing with lots of things at once"라고 정의했습니다.

2. **병렬성(Parallelism)**: 여러 작업이 물리적으로 동시에 실행되는 것. "Parallelism is about doing lots of things at once"입니다. 비동기는 주로 동시성에 관한 것이지만, 병렬성도 활용할 수 있습니다.

3. **논블로킹 I/O (Non-blocking I/O)**: 시스템 호출이 즉시 반환되어 스레드가 차단되지 않는 I/O 방식. Linux의 epoll, Windows의 IOCP(I/O Completion Ports) 등이 대표적입니다.

4. **협력적 멀티태스킹(Cooperative Multitasking)**: 작업이 자발적으로 제어권을 넘겨주는 방식. `await`는 협력적 양보(yield) 지점입니다.

5. **연속(Continuation)**: 프로그램의 실행 상태를 나타내는 개념. `await` 이후의 코드는 **continuation**으로 변환됩니다.

**비동기 프로그래밍이 해결하는 문제:**

1. **I/O 바운드 작업의 비효율성**: 전통적인 동기 I/O에서 스레드는 작업이 완료될 때까지 차단됩니다. 웹 서버가 1000개의 요청을 처리한다면 1000개의 스레드가 필요하고, 각 스레드는 약 1MB의 스택 메모리를 소비합니다. 비동기는 소수의 스레드로 수천 개의 동시 연결을 처리할 수 있게 합니다.

2. **UI 응답성**: 데스크톱 애플리케이션에서 UI 스레드가 차단되면 화면이 멈춥니다. 비동기는 UI 스레드를 차단하지 않으면서 백그라운드 작업을 수행할 수 있게 합니다.

3. **확장성(Scalability)**: 클라우드 시대에 서버는 제한된 리소스로 많은 요청을 처리해야 합니다. 비동기는 스레드 풀을 효율적으로 사용하여 **처리량(Throughput)**을 극대화합니다.

4. **복잡한 비동기 로직의 간결성**: 콜백 지옥을 피하고, 순차적 코드처럼 읽히는 비동기 코드를 작성할 수 있습니다.

**C#의 async/await가 특별한 이유:**

C#의 `async`/`await`는 단순한 문법 설탕(Syntactic Sugar)이 아닙니다. 컴파일러는 비동기 메서드를 **상태 머신(State Machine)**으로 변환하며, 이는 다음과 같은 특징을 가집니다:

- **제로 할당(Zero Allocation)**: 대부분의 경우 추가 힙 할당 없이 동작합니다 (ValueTask 사용 시).
- **예외 전파(Exception Propagation)**: 비동기 경계를 넘어 예외가 올바르게 전파됩니다.
- **컨텍스트 캡처(Context Capture)**: SynchronizationContext를 자동으로 캡처하여 UI 스레드로 돌아갑니다.
- **취소 지원(Cancellation Support)**: CancellationToken을 통한 협력적 취소를 지원합니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 비동기 프로그래밍의 이론적 기반과 실용적 구현을 체계적으로 학습하게 됩니다:

- **동기 vs 비동기의 근본적 차이**: 블로킹(Blocking) vs 논블로킹(Non-blocking), 동시성과 병렬성의 개념, 스레드 풀(Thread Pool)의 동작 원리, I/O 바운드 vs CPU 바운드 작업의 구분, 그리고 실제 성능 측정 방법을 이해합니다.

- **async와 await 키워드의 의미론**: 컴파일러가 생성하는 상태 머신의 구조, 연속(Continuation)의 개념, SynchronizationContext와 ConfigureAwait의 역할, 그리고 비동기 메서드의 실행 모델을 깊이 이해합니다.

- **Task와 Task<T>의 내부 구조**: Task의 생명주기와 상태 전환, TaskScheduler의 역할, Task vs ValueTask의 성능 차이, Task 기반 비동기 패턴(TAP), 그리고 Task Parallel Library (TPL)의 활용 방법을 학습합니다.

- **비동기 메서드 작성의 모범 사례**: 비동기 메서드 명명 규칙, 예외 처리 패턴, 데드락(Deadlock) 방지, 비동기 void의 위험성, 그리고 비동기 디스포즈 패턴(IAsyncDisposable)을 배웁니다.

- **비동기 작업 조율(Orchestration)**: Task.WhenAll과 Task.WhenAny의 활용, 병렬 실행과 순차 실행의 선택, 재시도(Retry)와 회로 차단기(Circuit Breaker) 패턴, 그리고 비동기 스트림(IAsyncEnumerable)의 기초를 익힙니다.

**학습 목표:**

- 비동기 프로그래밍의 이론적 기반과 역사적 맥락 이해
- 동시성과 병렬성의 차이, I/O 바운드 vs CPU 바운드 작업 구분
- async/await의 내부 동작 원리와 컴파일러 변환 이해
- Task 기반 비동기 패턴(TAP)의 올바른 사용법 습득
- 비동기 코드의 성능 특성과 최적화 기법 이해
- 실무에서 마주치는 비동기 안티패턴 회피 능력 배양

---

## 17.1 동기 vs 비동기

동기(Synchronous)와 비동기(Asynchronous) 프로그래밍의 차이는 단순한 실행 순서의 문제가 아니라, **제어 흐름(Control Flow)**과 **자원 활용(Resource Utilization)**에 대한 근본적으로 다른 접근 방식입니다. 이 차이를 이해하는 것은 현대 소프트웨어 아키텍처 설계의 핵심입니다.

**컴퓨터 과학적 관점에서의 동기 vs 비동기:**

동기와 비동기의 차이는 **호출자(Caller)**와 **피호출자(Callee)** 사이의 결합도(Coupling)와 관련이 있습니다:

- **동기(Synchronous)**: 호출자는 피호출자가 완료될 때까지 **차단(Blocking)**됩니다. 이는 **긴밀한 시간적 결합(Tight Temporal Coupling)**을 의미하며, 호출 스택(Call Stack)이 유지됩니다.

- **비동기(Asynchronous)**: 호출자는 피호출자의 완료를 기다리지 않고 **즉시 반환(Immediate Return)**됩니다. 이는 **느슨한 시간적 결합(Loose Temporal Coupling)**이며, 완료 통지는 **콜백(Callback)**, **Promise/Future**, 또는 **이벤트(Event)**를 통해 이루어집니다.

**블로킹(Blocking) vs 논블로킹(Non-blocking):**

비동기 프로그래밍을 이해하려면 블로킹과 논블로킹의 개념을 명확히 해야 합니다:

**블로킹 I/O (Synchronous I/O)**는 운영체제 수준에서 시스템 호출이 완료될 때까지 스레드가 **대기 상태(Waiting State)**로 전환되는 것을 의미합니다. 이 동안 스레드는 CPU를 사용하지 않지만, 스레드 자체는 여전히 메모리를 차지하고 컨텍스트 스위칭(Context Switching) 비용이 발생합니다.

```
블로킹 I/O의 스레드 상태:
Thread → Blocked (waiting for I/O) → Ready → Running
         ^^^^^^^^^^^^^^^^^^^^^^
         이 기간 동안 스레드는 "좀비" 상태
```

**논블로킹 I/O (Asynchronous I/O)**는 시스템 호출이 즉시 반환되고, 작업은 백그라운드에서 진행됩니다. 완료는 **폴링(Polling)**, **이벤트 통지(Event Notification)**, 또는 **완료 포트(Completion Port)**를 통해 확인됩니다.

```
논블로킹 I/O의 흐름:
Thread → Initiates I/O → Returns immediately → Does other work
                                               ↓
         I/O Complete Event ← Hardware/OS ← I/O Device
```

Windows의 **IOCP (I/O Completion Ports)**와 Linux의 **epoll/io_uring**은 고성능 비동기 I/O를 위한 운영체제 메커니즘입니다. .NET의 비동기 I/O는 내부적으로 이러한 플랫폼 API를 활용합니다.

### 동기 프로그래밍 (Synchronous Programming)

동기 방식에서는 한 작업이 완료될 때까지 다음 작업이 대기합니다. 이는 **순차적 실행(Sequential Execution)** 모델로, 프로그램의 제어 흐름이 명확하고 예측 가능합니다. 각 명령어는 이전 명령어가 완료된 후에 실행되며, 호출 스택(Call Stack)은 LIFO(Last-In-First-Out) 순서를 유지합니다.

```csharp
// 동기 방식 - 파일 읽기
string content = File.ReadAllText("data.txt");
Console.WriteLine("파일 내용:");
Console.WriteLine(content);
Console.WriteLine("작업 완료");

// 출력:
// 파일 내용:
// (파일의 내용)
// 작업 완료
```

**동기 방식의 문제점:**

```csharp
// 동기 방식으로 여러 파일 읽기
Console.WriteLine("파일 읽기 시작...");

string file1 = File.ReadAllText("file1.txt");  // 완료될 때까지 대기
string file2 = File.ReadAllText("file2.txt");  // 완료될 때까지 대기
string file3 = File.ReadAllText("file3.txt");  // 완료될 때까지 대기

Console.WriteLine("모든 파일 읽기 완료");

// 각 파일을 순차적으로 읽기 때문에 총 시간 = file1 + file2 + file3
```

### 비동기 프로그래밍 (Asynchronous Programming)

비동기 방식에서는 시간이 오래 걸리는 작업을 시작한 후, 완료를 기다리지 않고 다른 작업을 계속 수행할 수 있습니다.

```csharp
// 비동기 방식 - 파일 읽기
string content = await File.ReadAllTextAsync("data.txt");
Console.WriteLine("파일 내용:");
Console.WriteLine(content);
Console.WriteLine("작업 완료");
```

**비동기 방식의 장점:**

```csharp
// 비동기 방식으로 여러 파일 동시에 읽기
Console.WriteLine("파일 읽기 시작...");

Task<string> task1 = File.ReadAllTextAsync("file1.txt");  // 시작하고 계속 진행
Task<string> task2 = File.ReadAllTextAsync("file2.txt");  // 시작하고 계속 진행
Task<string> task3 = File.ReadAllTextAsync("file3.txt");  // 시작하고 계속 진행

// 모든 작업이 완료될 때까지 대기
await Task.WhenAll(task1, task2, task3);

Console.WriteLine("모든 파일 읽기 완료");

// 여러 파일을 동시에 읽기 때문에 총 시간 ≈ max(file1, file2, file3)
```

### 비동기가 필요한 상황

- **I/O 작업**: 파일 읽기/쓰기, 네트워크 통신
- **웹 요청**: HTTP API 호출
- **데이터베이스 쿼리**: 시간이 걸리는 쿼리 실행
- **UI 애플리케이션**: 사용자 인터페이스가 멈추지 않도록 유지

---

## 17.2 async와 await 키워드

C# 5.0에서 도입된 `async`와 `await` 키워드는 비동기 프로그래밍의 혁명적 전환점이었습니다. 이 키워드들은 단순한 문법 설탕(Syntactic Sugar)이 아니라, 컴파일러가 복잡한 **상태 머신(State Machine)**을 생성하도록 하는 강력한 메타프로그래밍 메커니즘입니다.

**async/await의 이론적 기반:**

`async`/`await`는 **Continuation-Passing Style (CPS)**와 **모나드(Monad)** 이론에 기반합니다. 함수형 프로그래밍에서 continuation은 "프로그램의 나머지 부분"을 나타내는 함수입니다. `await` 키워드는 현재 실행 지점 이후의 코드를 continuation으로 캡처하고, 비동기 작업이 완료되면 이 continuation을 실행합니다.

Haskell의 **IO 모나드**나 F#의 **async 워크플로우**와 유사하게, C#의 `async`/`await`는 **비동기 모나드(Async Monad)**를 구현합니다. 이는 다음과 같은 모나드 법칙을 만족합니다:

```
Left Identity:  Task.FromResult(a).ContinueWith(f) ≡ f(a)
Right Identity: task.ContinueWith(Task.FromResult) ≡ task  
Associativity:  (task >>= f) >>= g ≡ task >>= (x => f(x) >>= g)
```

**컴파일러의 변환 과정:**

`async` 메서드는 컴파일 시점에 **상태 머신 클래스**로 변환됩니다. 이 상태 머신은 **IAsyncStateMachine** 인터페이스를 구현하며, 각 `await` 지점은 상태 전환을 나타냅니다.

```csharp
// 원본 비동기 메서드
async Task<int> ComputeAsync()
{
    await Task.Delay(100);
    int result = 42;
    await Task.Delay(100);
    return result;
}

// 컴파일러가 생성하는 대략적인 구조 (간소화됨)
class ComputeAsyncStateMachine : IAsyncStateMachine
{
    int state = -1;  // 현재 상태
    AsyncTaskMethodBuilder<int> builder;  // Task 빌더
    TaskAwaiter awaiter1;  // 첫 번째 await의 awaiter
    TaskAwaiter awaiter2;  // 두 번째 await의 awaiter
    int result;  // 지역 변수
    
    public void MoveNext()
    {
        int localState = state;
        try
        {
            switch (localState)
            {
                case -1:  // 초기 상태
                    awaiter1 = Task.Delay(100).GetAwaiter();
                    if (!awaiter1.IsCompleted)
                    {
                        state = 0;
                        builder.AwaitUnsafeOnCompleted(ref awaiter1, ref this);
                        return;  // 비동기로 대기
                    }
                    goto case 0;
                    
                case 0:  // 첫 번째 await 이후
                    awaiter1.GetResult();
                    result = 42;
                    awaiter2 = Task.Delay(100).GetAwaiter();
                    if (!awaiter2.IsCompleted)
                    {
                        state = 1;
                        builder.AwaitUnsafeOnCompleted(ref awaiter2, ref this);
                        return;
                    }
                    goto case 1;
                    
                case 1:  // 두 번째 await 이후
                    awaiter2.GetResult();
                    builder.SetResult(result);  // Task 완료
                    return;
            }
        }
        catch (Exception ex)
        {
            state = -2;
            builder.SetException(ex);  // 예외 전파
        }
    }
}
```

이 변환은 **zero-cost abstraction**을 지향하며, 불필요한 힙 할당을 최소화합니다. 상태 머신 구조체는 최적화된 경우 스택에만 존재할 수 있습니다.

**SynchronizationContext와 실행 컨텍스트:**

`await`는 기본적으로 **SynchronizationContext**를 캡처합니다. 이는 UI 애플리케이션에서 매우 중요한데, `await` 이후의 코드가 원래의 UI 스레드에서 실행되도록 보장합니다.

```csharp
// UI 스레드에서 실행
async Task UpdateUIAsync()
{
    // UI 스레드에서 실행
    var data = await LoadDataAsync();
    
    // await 이후에도 UI 스레드에서 실행됨
    // SynchronizationContext가 자동으로 복원됨
    textBox.Text = data;  // 안전함!
}
```

**SynchronizationContext의 내부 동작:**

```
1. await 전:
   - 현재 SynchronizationContext.Current를 캡처
   - AsyncMethodBuilder에 저장

2. 비동기 작업 완료 시:
   - 캡처된 SynchronizationContext.Post()를 호출
   - Continuation을 원래 컨텍스트에 스케줄링

3. await 후:
   - 원래 컨텍스트에서 실행 재개
```

UI 프레임워크(WPF, WinForms, UWP, MAUI)는 각자의 SynchronizationContext 구현을 제공하며, ASP.NET Core는 성능을 위해 SynchronizationContext를 사용하지 않습니다.

**ConfigureAwait의 필요성:**

라이브러리 코드에서는 `ConfigureAwait(false)`를 사용하여 컨텍스트 캡처를 방지해야 합니다:

```csharp
// ✅ 라이브러리 코드 - 컨텍스트 캡처 방지
async Task<string> LibraryMethodAsync()
{
    var response = await httpClient.GetStringAsync(url)
        .ConfigureAwait(false);  // UI 스레드로 돌아갈 필요 없음
    
    var data = await ProcessDataAsync(response)
        .ConfigureAwait(false);  // 성능 향상
    
    return data;
}

// ❌ UI 코드 - ConfigureAwait 사용하지 말 것
async Task ButtonClickAsync()
{
    var data = await LoadDataAsync()
        .ConfigureAwait(false);  // 나쁨!
    
    textBox.Text = data;  // 💥 크로스 스레드 오류!
}
```

`ConfigureAwait(false)`의 이점:
1. **성능 향상**: 컨텍스트 전환 오버헤드 제거
2. **데드락 방지**: 특정 동기화 문제 회피
3. **스레드 풀 효율**: 워커 스레드에서 계속 실행

### async 키워드

`async` 키워드는 메서드, 람다 식, 익명 메서드에 적용할 수 있으며, 컴파일러에게 이 메서드가 상태 머신으로 변환되어야 함을 알립니다.

```csharp
// 메서드에 async 적용
async Task ProcessDataAsync()
{
    await Task.Delay(1000);
    Console.WriteLine("처리 완료");
}

// 람다 식에 async 적용
Func<Task<int>> lambda = async () =>
{
    await Task.Delay(500);
    return 42;
};

// 익명 메서드에 async 적용 (구식 문법)
Func<Task> anonymous = async delegate()
{
    await Task.Delay(1000);
};
```

**async 메서드의 특징:**

1. **반환 타입 제약**: `Task`, `Task<T>`, `ValueTask<T>`, 또는 `void` (이벤트 핸들러만)
2. **ref/out 매개변수 불가**: 상태 머신 구조상 ref/out은 지원되지 않음
3. **명명 규칙**: `Async` 접미사 사용 권장 (TAP - Task-based Asynchronous Pattern)
4. **동기 부분 실행**: 첫 `await`까지는 동기적으로 실행됨

**async의 반환 타입 선택:**

```csharp
// Task - 반환값 없음
async Task LogMessageAsync(string message)
{
    await File.AppendAllTextAsync("log.txt", message);
}

// Task<T> - T 타입 반환
async Task<string> DownloadAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url);
}

// ValueTask<T> - 핫 패스 최적화 (자주 동기 완료되는 경우)
async ValueTask<int> GetCachedValueAsync(string key)
{
    if (cache.TryGetValue(key, out int value))
        return value;  // 동기 완료, 할당 없음
    
    value = await LoadFromDatabaseAsync(key);  // 비동기 필요 시만
    cache[key] = value;
    return value;
}

// void - 이벤트 핸들러만 (권장하지 않음)
async void Button_Click(object sender, EventArgs e)
{
    await ProcessAsync();  // 예외 처리 어려움
}
```

**async void의 위험성:**

`async void`는 다음과 같은 문제가 있어 피해야 합니다:

1. **예외 추적 불가**: 예외가 SynchronizationContext로 전파되어 애플리케이션 크래시 가능
2. **완료 대기 불가**: Task를 반환하지 않으므로 완료 시점을 알 수 없음
3. **테스트 어려움**: 단위 테스트에서 완료를 기다릴 방법이 없음

```csharp
// ❌ 나쁜 예 - async void
async void ProcessDataAsync()
{
    await Task.Delay(1000);
    throw new Exception("오류!");  // 💥 처리되지 않은 예외로 앱 크래시
}

// ✅ 좋은 예 - async Task
async Task ProcessDataAsync()
{
    await Task.Delay(1000);
    throw new Exception("오류!");  // Task에 캡슐화되어 호출자가 처리 가능
}
```

### await 키워드

`await` 키워드는 **awaitable 패턴**을 구현한 모든 타입에 적용할 수 있습니다. `Task`와 `Task<T>`가 가장 일반적이지만, `GetAwaiter()` 메서드를 구현한 모든 타입이 awaitable입니다.

```csharp
// Task를 await
await Task.Delay(1000);

// Task<T>를 await - 결과 값 반환
int result = await Task.Run(() => 42);

// ValueTask<T>를 await
int cached = await GetCachedValueAsync("key");

// 사용자 정의 awaitable
await CustomAwaitable.CreateAsync();
```

**Awaitable 패턴의 요구사항:**

```csharp
// Awaitable 타입의 구조
public class MyAwaitable
{
    // GetAwaiter() 메서드 필요
    public MyAwaiter GetAwaiter() => new MyAwaiter();
}

public class MyAwaiter : INotifyCompletion
{
    // IsCompleted - 이미 완료되었는지 확인
    public bool IsCompleted => false;
    
    // GetResult() - 결과 반환 (또는 void)
    public void GetResult() { }
    
    // OnCompleted() - continuation 등록
    public void OnCompleted(Action continuation)
    {
        // 완료 시 continuation 호출
        ThreadPool.QueueUserWorkItem(_ => continuation());
    }
}
```

**await의 실행 흐름:**

```csharp
async Task DownloadDataAsync()
{
    Console.WriteLine("다운로드 시작...");
    
    // await 지점
    await Task.Delay(2000);
    
    Console.WriteLine("다운로드 완료!");
}
```

내부 동작:
1. `Task.Delay(2000)`의 `GetAwaiter()` 호출
2. `IsCompleted` 확인
   - `true`이면 즉시 계속 (동기 완료)
   - `false`이면 continuation 등록 후 반환
3. Task 완료 시 continuation 실행
4. `GetResult()` 호출로 결과 획득 또는 예외 재발생
5. 다음 코드 실행

**await와 스레드:**

```csharp
async Task DemonstrateThreadsAsync()
{
    Console.WriteLine($"시작: Thread {Thread.CurrentThread.ManagedThreadId}");
    
    await Task.Delay(100);
    Console.WriteLine($"Delay 후: Thread {Thread.CurrentThread.ManagedThreadId}");
    
    await Task.Run(() => 
        Console.WriteLine($"Task.Run 내부: Thread {Thread.CurrentThread.ManagedThreadId}"));
    
    Console.WriteLine($"Task.Run 후: Thread {Thread.CurrentThread.ManagedThreadId}");
}

// 출력 예시 (콘솔 앱):
// 시작: Thread 1
// Delay 후: Thread 4  (스레드 풀 스레드)
// Task.Run 내부: Thread 5
// Task.Run 후: Thread 4 또는 다른 스레드 풀 스레드
```

중요한 점: `await`는 스레드를 **차단하지 않습니다**. 스레드는 스레드 풀로 반환되어 다른 작업을 수행할 수 있습니다.

### 기본 사용 예제

```csharp
// 비동기 메서드 정의
async Task<string> GetGreetingAsync(string name)
{
    // 시뮬레이션: 데이터베이스나 API 호출
    await Task.Delay(1000);
    return $"안녕하세요, {name}님!";
}

// 비동기 메서드 호출
async Task RunAsync()
{
    Console.WriteLine("작업 시작");
    
    string greeting = await GetGreetingAsync("홍길동");
    Console.WriteLine(greeting);
    
    Console.WriteLine("작업 완료");
}

// 출력:
// 작업 시작
// (1초 대기)
// 안녕하세요, 홍길동님!
// 작업 완료
```

### 동기 vs 비동기 비교

```csharp
// 동기 방식
void DownloadSync()
{
    Console.WriteLine("다운로드 시작...");
    Thread.Sleep(2000);  // 스레드 차단
    Console.WriteLine("다운로드 완료!");
}

// 비동기 방식
async Task DownloadAsync()
{
    Console.WriteLine("다운로드 시작...");
    await Task.Delay(2000);  // 스레드 차단 없음
    Console.WriteLine("다운로드 완료!");
}
```

---

## 17.3 Task와 Task<T>

`Task`는 비동기 작업을 나타내는 객체로, .NET의 **Task Parallel Library (TPL)**의 핵심 구성요소입니다. Task는 단순한 비동기 작업의 표현을 넘어, **Future/Promise 패턴**의 .NET 구현이며, **composable 비동기 연산**의 기반입니다.

**Task의 이론적 기반:**

Task는 함수형 프로그래밍의 **Future** 또는 **Promise** 개념을 구현합니다. 1977년 Henry Baker와 Carl Hewitt이 처음 제안한 Future는 "아직 계산되지 않았지만 미래에 사용 가능할 값"을 나타냅니다. Promise는 1980년대 Barbara Liskov의 연구에서 발전했으며, JavaScript의 Promise, Java의 Future, Scala의 Future 등 많은 언어에서 구현되었습니다.

Task는 **모나드(Monad)**의 일종입니다. 정확히는 **IO 모나드**와 유사한 구조를 가지며, `Task<T>`는 다음과 같은 모나드 연산을 지원합니다:

```csharp
// Return (unit) - 값을 Task로 래핑
Task<int> unit = Task.FromResult(42);

// Bind (flatMap) - Task를 변환하고 평탄화
Task<string> Bind<A, B>(Task<A> task, Func<A, Task<B>> f) =>
    task.ContinueWith(t => f(t.Result)).Unwrap();

// Map (functor) - Task 내부 값 변환
Task<B> Map<A, B>(Task<A> task, Func<A, B> f) =>
    task.ContinueWith(t => f(t.Result));
```

**Task의 내부 구조:**

Task는 복잡한 상태 머신으로, 다음과 같은 주요 필드를 포함합니다:

```csharp
public class Task
{
    // 상태 필드 (비트 플래그)
    private volatile int m_stateFlags;
    
    // 작업 ID (디버깅용)
    private static int s_taskIdCounter;
    private int m_taskId;
    
    // 실행 대리자 (delegate)
    private Delegate m_action;
    
    // 상태 객체 (사용자 정의 데이터)
    private object m_stateObject;
    
    // TaskScheduler (작업 스케줄링 담당)
    private TaskScheduler m_taskScheduler;
    
    // 부모 Task (중첩 Task의 경우)
    private Task m_parent;
    
    // Continuation 체인
    private object m_continuationObject;
    
    // 예외 정보
    private ContingentProperties m_contingentProperties;
}

public class Task<TResult> : Task
{
    // 결과 값
    internal TResult m_result;
}
```

**Task의 생명주기와 상태 전환:**

Task는 다음과 같은 상태를 가지며, 상태 전환은 단방향입니다:

```
Created → WaitingForActivation → WaitingToRun → Running → 
    → RanToCompletion (성공)
    → Faulted (예외 발생)
    → Canceled (취소됨)
```

```csharp
// Task 상태 확인
Task task = SomeOperationAsync();

// 상태 속성
bool isCompleted = task.IsCompleted;      // 완료 여부 (성공, 실패, 취소 모두 포함)
bool isCompletedSuccessfully = task.IsCompletedSuccessfully;  // 성공적으로 완료
bool isFaulted = task.IsFaulted;          // 예외 발생
bool isCanceled = task.IsCanceled;        // 취소됨

TaskStatus status = task.Status;  // 정확한 상태
```

**TaskStatus 열거형:**

```csharp
public enum TaskStatus
{
    Created = 0,                      // 초기 상태
    WaitingForActivation = 1,         // 스케줄 대기
    WaitingToRun = 2,                 // 스케줄되었지만 실행 대기
    Running = 3,                      // 현재 실행 중
    WaitingForChildrenToComplete = 4, // 자식 Task 대기
    RanToCompletion = 5,              // 성공적으로 완료
    Canceled = 6,                     // 취소됨
    Faulted = 7                       // 예외로 실패
}
```

**Task vs ValueTask - 성능 최적화:**

`ValueTask<T>`는 .NET Core 2.0에서 도입된 구조체 기반 Task로, **핫 패스(hot path) 최적화**를 위해 설계되었습니다. 자주 동기 완료되는 경로에서 힙 할당을 제거합니다.

```csharp
// Task<T> - 항상 힙 할당
async Task<int> GetValueTaskAsync()
{
    if (cache.TryGetValue(key, out int value))
        return value;  // Task<int> 객체 할당 발생
    
    return await LoadFromDatabaseAsync(key);
}

// ValueTask<T> - 캐시 히트 시 할당 없음
async ValueTask<int> GetValueValueTaskAsync()
{
    if (cache.TryGetValue(key, out int value))
        return value;  // 구조체, 힙 할당 없음
    
    return await LoadFromDatabaseAsync(key);
}
```

**ValueTask 사용 시 주의사항:**

1. **한 번만 await**: ValueTask는 한 번만 await 해야 합니다 (재사용 불가)
2. **동기 완료가 빈번한 경우만**: 비동기가 대부분이면 Task가 더 나을 수 있음
3. **저장하지 말 것**: 지역 변수로만 사용, 필드에 저장하지 않기
4. **공개 API 고려**: 라이브러리 API는 신중하게 선택

```csharp
// ❌ 잘못된 ValueTask 사용
ValueTask<int> task = GetValueAsync();
int result1 = await task;  // OK
int result2 = await task;  // 💥 예측 불가능한 동작!

// ✅ 올바른 ValueTask 사용
int result = await GetValueAsync();  // 직접 await
```

**Task 기반 비동기 패턴 (TAP - Task-based Asynchronous Pattern):**

TAP는 .NET의 표준 비동기 패턴으로, 다음과 같은 명명 규칙과 특성을 따릅니다:

1. **명명 규칙**: 메서드 이름에 `Async` 접미사
2. **반환 타입**: `Task` 또는 `Task<T>`
3. **취소 지원**: `CancellationToken` 매개변수 (선택적)
4. **진행률 보고**: `IProgress<T>` 매개변수 (선택적)
5. **예외 처리**: 예외는 반환된 Task에 캡슐화

```csharp
// TAP 패턴 완전한 구현
async Task<byte[]> DownloadDataAsync(
    string url,
    CancellationToken cancellationToken = default,
    IProgress<int> progress = null)
{
    using var client = new HttpClient();
    using var response = await client.GetAsync(url, 
        HttpCompletionOption.ResponseHeadersRead, 
        cancellationToken);
    
    response.EnsureSuccessStatusCode();
    
    var totalBytes = response.Content.Headers.ContentLength ?? 0;
    var buffer = new byte[8192];
    var totalRead = 0L;
    
    using var stream = await response.Content.ReadAsStreamAsync();
    using var memoryStream = new MemoryStream();
    
    int bytesRead;
    while ((bytesRead = await stream.ReadAsync(buffer, 0, buffer.Length, 
        cancellationToken)) > 0)
    {
        await memoryStream.WriteAsync(buffer, 0, bytesRead, cancellationToken);
        totalRead += bytesRead;
        
        // 진행률 보고
        if (totalBytes > 0)
            progress?.Report((int)(totalRead * 100 / totalBytes));
    }
    
    return memoryStream.ToArray();
}

// 사용
var cts = new CancellationTokenSource();
var progress = new Progress<int>(percent => 
    Console.WriteLine($"다운로드: {percent}%"));

try
{
    byte[] data = await DownloadDataAsync(url, cts.Token, progress);
}
catch (OperationCanceledException)
{
    Console.WriteLine("다운로드 취소됨");
}
```

### Task - 값을 반환하지 않는 비동기 작업

`Task`는 반환값이 없는 비동기 작업을 나타냅니다 (`void` 메서드의 비동기 버전). `Task`는 작업의 완료, 취소, 또는 실패를 추적할 수 있습니다.

```csharp
async Task PrintMessageAsync()
{
    await Task.Delay(1000);
    Console.WriteLine("메시지 출력!");
}

// 호출
await PrintMessageAsync();
```

### Task<T> - 값을 반환하는 비동기 작업

`Task<T>`는 `T` 타입의 값을 반환하는 비동기 작업을 나타냅니다.

```csharp
async Task<int> CalculateAsync(int a, int b)
{
    await Task.Delay(500);
    return a + b;
}

// 호출
int result = await CalculateAsync(10, 20);
Console.WriteLine($"결과: {result}");

// 출력:
// 결과: 30
```

### Task 생성 방법

**1. Task.Run() - CPU 집약적 작업**

```csharp
Task<int> task = Task.Run(() =>
{
    // 시간이 걸리는 계산
    int sum = 0;
    for (int i = 0; i < 1000000; i++)
    {
        sum += i;
    }
    return sum;
});

int result = await task;
Console.WriteLine($"합계: {result}");
```

**2. Task.Delay() - 지연**

```csharp
// 1초 대기
await Task.Delay(1000);

// 지정된 시간만큼 대기
await Task.Delay(TimeSpan.FromSeconds(2));
```

**3. Task.FromResult() - 즉시 완료된 Task**

```csharp
Task<int> GetCachedValue()
{
    int cachedValue = 42;
    return Task.FromResult(cachedValue);
}

int value = await GetCachedValue();
```

### Task의 상태

Task는 다음과 같은 상태를 가질 수 있습니다:

```csharp
Task task = SomeAsyncOperation();

// Task 상태 확인
if (task.IsCompleted)
    Console.WriteLine("작업 완료");

if (task.IsFaulted)
    Console.WriteLine("작업 실패");

if (task.IsCanceled)
    Console.WriteLine("작업 취소됨");
```

---

## 17.4 비동기 메서드 작성하기

비동기 메서드를 올바르게 작성하는 것은 단순히 `async`와 `await`를 추가하는 것 이상입니다. **비동기 메서드 설계**는 예외 처리, 취소, 진행률 보고, 리소스 관리, 데드락 방지 등 여러 측면을 고려해야 하는 복잡한 과제입니다.

**비동기 메서드 설계 원칙:**

1. **All async or none**: 비동기 경로를 시작하면 끝까지 비동기로 유지
2. **Avoid async void**: 이벤트 핸들러를 제외하고 async void는 절대 사용하지 않기
3. **ConfigureAwait(false) in libraries**: 라이브러리 코드에서는 컨텍스트 캡처 방지
4. **Cancel gracefully**: CancellationToken을 통한 협력적 취소 지원
5. **Don't block on async code**: `.Result`나 `.Wait()` 사용 금지 (데드락 위험)

**비동기 메서드의 안티패턴:**

```csharp
// ❌ 안티패턴 1: async void (이벤트 핸들러 제외)
async void ProcessDataAsync()  // 💥 예외 추적 불가
{
    await Task.Delay(1000);
    throw new Exception();  // 앱 크래시
}

// ❌ 안티패턴 2: 동기 코드에서 Task를 차단
async Task<int> GetValueAsync()
{
    await Task.Delay(1000);
    return 42;
}

int value = GetValueAsync().Result;  // 💥 데드락 가능

// ❌ 안티패턴 3: 불필요한 async/await
async Task<string> GetDataAsync()
{
    return await File.ReadAllTextAsync("data.txt");  // 불필요한 상태 머신
}

// ✅ 올바른 방법
Task<string> GetDataAsync()
{
    return File.ReadAllTextAsync("data.txt");  // Task를 그대로 반환
}

// ❌ 안티패턴 4: async-over-sync (동기 작업을 Task.Run으로 감싸기)
async Task<int> ComputeAsync()
{
    return await Task.Run(() => ExpensiveComputation());  // I/O가 아니라면 불필요
}
```

**데드락의 원인과 해결:**

ASP.NET (Core 이전)이나 WPF/WinForms에서 가장 흔한 비동기 데드락 시나리오:

```csharp
// 💥 데드락 시나리오
async Task<string> GetDataAsync()
{
    await Task.Delay(1000);  // SynchronizationContext 캡처
    return "Data";
}

// UI 스레드에서 호출
void Button_Click(object sender, EventArgs e)
{
    string result = GetDataAsync().Result;  // 💥 데드락!
    // UI 스레드가 .Result를 기다림
    // GetDataAsync가 UI 스레드로 돌아오려고 함
    // 하지만 UI 스레드는 이미 차단됨
}

// ✅ 해결 방법 1: ConfigureAwait(false)
async Task<string> GetDataAsync()
{
    await Task.Delay(1000).ConfigureAwait(false);  // 컨텍스트 캡처 안함
    return "Data";
}

// ✅ 해결 방법 2: 끝까지 비동기
async void Button_Click(object sender, EventArgs e)
{
    string result = await GetDataAsync();  // 차단하지 않음
}
```

**데드락의 메커니즘:**

```
1. UI 스레드가 GetDataAsync().Result 호출
2. UI 스레드가 Task 완료를 차단 대기
3. Task.Delay 완료 후, continuation이 SynchronizationContext에 포스트됨
4. continuation은 UI 스레드에서 실행되어야 함
5. 하지만 UI 스레드는 2단계에서 차단 중
6. 💥 데드락: UI 스레드는 Task를 기다리고, Task는 UI 스레드를 기다림
```

### 기본 패턴

```csharp
// 반환값이 없는 비동기 메서드
async Task DoWorkAsync()
{
    await Task.Delay(1000);
    Console.WriteLine("작업 완료");
}

// 반환값이 있는 비동기 메서드
async Task<string> GetDataAsync()
{
    await Task.Delay(1000);
    return "데이터";
}
```

### 실용적인 예제 - 파일 읽기

```csharp
async Task<string> ReadFileAsync(string path)
{
    try
    {
        // 비동기로 파일 읽기
        string content = await File.ReadAllTextAsync(path);
        return content;
    }
    catch (FileNotFoundException)
    {
        Console.WriteLine("파일을 찾을 수 없습니다.");
        return string.Empty;
    }
}

// 사용
string content = await ReadFileAsync("data.txt");
Console.WriteLine(content);
```

### 실용적인 예제 - HTTP 요청

```csharp
async Task<string> DownloadContentAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        try
        {
            string content = await client.GetStringAsync(url);
            return content;
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"다운로드 오류: {ex.Message}");
            return string.Empty;
        }
    }
}

// 사용
string html = await DownloadContentAsync("https://example.com");
Console.WriteLine($"다운로드 크기: {html.Length} 문자");
```

### 여러 비동기 작업 조합

```csharp
async Task<int> GetUserCountAsync()
{
    await Task.Delay(500);
    return 100;
}

async Task<int> GetProductCountAsync()
{
    await Task.Delay(500);
    return 50;
}

async Task<string> GetStatisticsAsync()
{
    // 순차 실행 (총 1초)
    int userCount = await GetUserCountAsync();
    int productCount = await GetProductCountAsync();
    
    return $"사용자: {userCount}, 제품: {productCount}";
}

// 더 나은 방법: 동시 실행
async Task<string> GetStatisticsFastAsync()
{
    // 동시 실행 (총 0.5초)
    Task<int> userTask = GetUserCountAsync();
    Task<int> productTask = GetProductCountAsync();
    
    await Task.WhenAll(userTask, productTask);
    
    return $"사용자: {userTask.Result}, 제품: {productTask.Result}";
}
```

### 비동기 메서드의 반환 타입

**1. Task - 반환값 없음**

```csharp
async Task LogMessageAsync(string message)
{
    await Task.Delay(100);
    Console.WriteLine(message);
}
```

**2. Task<T> - T 타입 반환**

```csharp
async Task<int> AddAsync(int a, int b)
{
    await Task.Delay(100);
    return a + b;
}
```

**3. void - 이벤트 핸들러만 (권장하지 않음)**

```csharp
// 예외 처리가 어려우므로 이벤트 핸들러에서만 사용
async void Button_Click(object sender, EventArgs e)
{
    await DoWorkAsync();
}
```

---

## 17.5 비동기 메서드 호출과 대기

비동기 작업의 조율(Orchestration)은 여러 비동기 작업을 조합하고, 조정하고, 조율하는 기술입니다. `Task.WhenAll`, `Task.WhenAny`, `Task.WaitAll`, `Task.WaitAny` 등의 조합기(Combinators)를 사용하여 복잡한 비동기 워크플로우를 구축할 수 있습니다.

**비동기 조율의 이론적 기반:**

비동기 작업 조율은 **concurrent composition**과 **parallel composition**의 개념을 실현합니다. 이는 **Join 계산(Join Calculus)**과 **π-calculus**에서 연구된 프로세스 대수(Process Algebra)의 개념과 연결됩니다.

- **Sequential Composition**: `await task1; await task2;` - 순차 실행
- **Parallel Composition**: `await Task.WhenAll(task1, task2);` - 병렬 실행  
- **Alternative Composition**: `await Task.WhenAny(task1, task2);` - 경쟁 실행
- **Iterative Composition**: `foreach (var item in items) await ProcessAsync(item);` - 반복 실행

**동시성 vs 병렬성 재고:**

```csharp
// 동시성(Concurrency) - 여러 작업이 진행 중
async Task ConcurrentAsync()
{
    Task t1 = DownloadAsync("url1");  // 시작
    Task t2 = DownloadAsync("url2");  // 시작
    Task t3 = DownloadAsync("url3");  // 시작
    
    await Task.WhenAll(t1, t2, t3);  // 모두 완료 대기
    // I/O 작업이므로 실제로는 1개의 스레드만으로도 처리 가능
}

// 병렬성(Parallelism) - CPU 코어를 활용한 동시 실행
async Task ParallelAsync()
{
    Task<int> t1 = Task.Run(() => ExpensiveComputation1());
    Task<int> t2 = Task.Run(() => ExpensiveComputation2());
    Task<int> t3 = Task.Run(() => ExpensiveComputation3());
    
    int[] results = await Task.WhenAll(t1, t2, t3);
    // CPU 바운드 작업이므로 실제로 여러 코어에서 병렬 실행
}
```

**Task.WhenAll의 내부 동작:**

`Task.WhenAll`은 모든 Task가 완료될 때까지 대기하는 Task를 생성합니다. 내부적으로 **상태 집계(State Aggregation)**를 수행합니다:

```csharp
// Task.WhenAll의 의사 코드
public static Task<TResult[]> WhenAll<TResult>(params Task<TResult>[] tasks)
{
    if (tasks == null) throw new ArgumentNullException();
    if (tasks.Length == 0) return Task.FromResult(Array.Empty<TResult>());
    
    // Promise Task 생성
    var promise = new TaskCompletionSource<TResult[]>();
    int remainingCount = tasks.Length;
    var results = new TResult[tasks.Length];
    
    for (int i = 0; i < tasks.Length; i++)
    {
        int index = i;  // 클로저 캡처
        tasks[i].ContinueWith(t =>
        {
            if (t.IsFaulted)
            {
                // 예외 수집 (AggregateException)
                promise.TrySetException(t.Exception);
            }
            else if (t.IsCanceled)
            {
                promise.TrySetCanceled();
            }
            else
            {
                results[index] = t.Result;
                
                if (Interlocked.Decrement(ref remainingCount) == 0)
                {
                    promise.SetResult(results);  // 모두 완료
                }
            }
        });
    }
    
    return promise.Task;
}
```

**Task.WhenAll의 예외 처리:**

`Task.WhenAll`은 하나 이상의 Task가 실패해도 모든 Task의 완료를 기다립니다. 모든 예외는 `AggregateException`으로 수집됩니다:

```csharp
async Task WhenAllExceptionHandlingAsync()
{
    Task t1 = Task.Run(() => throw new InvalidOperationException("Task 1"));
    Task t2 = Task.Delay(100);
    Task t3 = Task.Run(() => throw new ArgumentException("Task 3"));
    
    try
    {
        await Task.WhenAll(t1, t2, t3);  // 모두 완료 대기
    }
    catch (Exception ex)
    {
        // await는 첫 번째 예외만 던짐
        Console.WriteLine($"First exception: {ex.Message}");
        
        // 모든 예외에 접근하려면:
        var allExceptions = Task.WhenAll(t1, t2, t3).Exception?.InnerExceptions;
        foreach (var exception in allExceptions ?? Enumerable.Empty<Exception>())
        {
            Console.WriteLine($"Exception: {exception.Message}");
        }
    }
}
```

**Task.WhenAny의 활용 패턴:**

`Task.WhenAny`는 **경쟁(Racing)** 패턴을 구현합니다. 첫 번째로 완료되는 Task를 반환하며, 타임아웃, 재시도, 대안 선택 등에 유용합니다:

```csharp
// 패턴 1: 타임아웃 구현
async Task<string> DownloadWithTimeoutAsync(string url, TimeSpan timeout)
{
    Task<string> downloadTask = DownloadAsync(url);
    Task timeoutTask = Task.Delay(timeout);
    
    Task completedTask = await Task.WhenAny(downloadTask, timeoutTask);
    
    if (completedTask == timeoutTask)
    {
        throw new TimeoutException("다운로드 시간 초과");
    }
    
    return await downloadTask;  // 결과 또는 예외 얻기
}

// 패턴 2: 여러 소스 중 가장 빠른 응답 사용
async Task<string> GetFastestResponseAsync(params string[] urls)
{
    Task<string>[] tasks = urls.Select(url => DownloadAsync(url)).ToArray();
    
    while (tasks.Length > 0)
    {
        Task<string> completedTask = await Task.WhenAny(tasks);
        
        try
        {
            return await completedTask;  // 첫 성공한 결과 반환
        }
        catch
        {
            // 실패하면 나머지 시도
            tasks = tasks.Where(t => t != completedTask).ToArray();
        }
    }
    
    throw new Exception("모든 소스 실패");
}

// 패턴 3: 작업 처리 파이프라인 (생산자-소비자)
async Task ProcessPipelineAsync()
{
    var tasks = new List<Task<string>>();
    
    for (int i = 0; i < 10; i++)
    {
        tasks.Add(DownloadAsync($"url{i}"));
    }
    
    while (tasks.Count > 0)
    {
        Task<string> completed = await Task.WhenAny(tasks);
        tasks.Remove(completed);
        
        string result = await completed;
        await ProcessResultAsync(result);  // 결과 처리
    }
}
```

**비동기 LINQ 패턴:**

LINQ 스타일로 비동기 작업을 조율할 수 있습니다:

```csharp
// SelectMany를 사용한 비동기 체이닝
async Task<IEnumerable<string>> GetAllUsersEmailsAsync()
{
    var users = await GetUsersAsync();
    
    var emailTasks = users.Select(async user => await GetUserEmailAsync(user.Id));
    
    return await Task.WhenAll(emailTasks);
}

// Where를 사용한 비동기 필터링
async Task<IEnumerable<User>> GetActiveUsersAsync()
{
    var users = await GetAllUsersAsync();
    
    var statusTasks = users.Select(async user => new
    {
        User = user,
        IsActive = await CheckUserActiveAsync(user.Id)
    });
    
    var results = await Task.WhenAll(statusTasks);
    
    return results.Where(r => r.IsActive).Select(r => r.User);
}
```

**재시도 패턴 (Retry Pattern):**

일시적 오류에 대한 재시도는 분산 시스템의 필수 패턴입니다:

```csharp
async Task<T> RetryAsync<T>(
    Func<Task<T>> operation,
    int maxRetries = 3,
    TimeSpan? delay = null)
{
    delay ??= TimeSpan.FromSeconds(1);
    
    for (int attempt = 0; attempt <= maxRetries; attempt++)
    {
        try
        {
            return await operation();
        }
        catch (Exception ex) when (attempt < maxRetries && IsTransient(ex))
        {
            Console.WriteLine($"재시도 {attempt + 1}/{maxRetries}...");
            await Task.Delay(delay.Value * (int)Math.Pow(2, attempt));  // 지수 백오프
        }
    }
    
    throw new Exception("최대 재시도 횟수 초과");
}

bool IsTransient(Exception ex)
{
    // 일시적 오류 판단 (네트워크, 타임아웃 등)
    return ex is HttpRequestException or TimeoutException;
}

// 사용
var data = await RetryAsync(async () => await DownloadAsync("url"));
```

**회로 차단기 패턴 (Circuit Breaker Pattern):**

연속 실패 시 시스템을 보호하는 패턴:

```csharp
class CircuitBreaker
{
    enum State { Closed, Open, HalfOpen }
    
    private State state = State.Closed;
    private int failureCount = 0;
    private DateTime lastFailureTime;
    private readonly int threshold = 5;
    private readonly TimeSpan timeout = TimeSpan.FromSeconds(60);
    
    public async Task<T> ExecuteAsync<T>(Func<Task<T>> operation)
    {
        if (state == State.Open)
        {
            if (DateTime.UtcNow - lastFailureTime > timeout)
            {
                state = State.HalfOpen;  // 재시도 허용
            }
            else
            {
                throw new Exception("Circuit breaker is OPEN");
            }
        }
        
        try
        {
            T result = await operation();
            
            if (state == State.HalfOpen)
            {
                state = State.Closed;  // 성공 - 닫힘
                failureCount = 0;
            }
            
            return result;
        }
        catch (Exception)
        {
            failureCount++;
            lastFailureTime = DateTime.UtcNow;
            
            if (failureCount >= threshold)
            {
                state = State.Open;  // 회로 열림
            }
            
            throw;
        }
    }
}
```

### 기본 await 사용

```csharp
async Task RunAsync()
{
    Console.WriteLine("작업 시작");
    
    // 비동기 메서드 호출 및 완료 대기
    await Task.Delay(1000);
    
    Console.WriteLine("작업 완료");
}
```

### 결과값 받기

```csharp
async Task<int> CalculateAsync()
{
    await Task.Delay(500);
    return 42;
}

async Task UseResultAsync()
{
    int result = await CalculateAsync();
    Console.WriteLine($"결과: {result}");
}
```

### 여러 Task 동시에 대기하기

**1. Task.WhenAll - 모든 작업 완료 대기**

```csharp
async Task DownloadAllAsync()
{
    Task task1 = DownloadFileAsync("file1.txt");
    Task task2 = DownloadFileAsync("file2.txt");
    Task task3 = DownloadFileAsync("file3.txt");
    
    // 모든 작업이 완료될 때까지 대기
    await Task.WhenAll(task1, task2, task3);
    
    Console.WriteLine("모든 다운로드 완료!");
}
```

**2. Task.WhenAll - 결과값 수집**

```csharp
async Task<int> GetNumberAsync(int n)
{
    await Task.Delay(100);
    return n * 2;
}

async Task ProcessNumbersAsync()
{
    Task<int>[] tasks = new Task<int>[]
    {
        GetNumberAsync(1),
        GetNumberAsync(2),
        GetNumberAsync(3)
    };
    
    // 모든 작업 완료 후 결과 배열 반환
    int[] results = await Task.WhenAll(tasks);
    
    Console.WriteLine($"결과: {string.Join(", ", results)}");
    // 출력: 결과: 2, 4, 6
}
```

**3. Task.WhenAny - 첫 번째 완료 작업 대기**

```csharp
async Task<string> QueryServerAsync(string serverName, int delay)
{
    await Task.Delay(delay);
    return $"{serverName} 응답";
}

async Task UseFirstResponseAsync()
{
    Task<string> server1 = QueryServerAsync("서버1", 1000);
    Task<string> server2 = QueryServerAsync("서버2", 2000);
    Task<string> server3 = QueryServerAsync("서버3", 1500);
    
    // 가장 먼저 완료되는 작업 대기
    Task<string> firstCompleted = await Task.WhenAny(server1, server2, server3);
    
    string result = await firstCompleted;
    Console.WriteLine($"첫 응답: {result}");
    // 출력: 첫 응답: 서버1 응답
}
```

### 비동기 작업 체이닝

```csharp
async Task<string> Step1Async()
{
    await Task.Delay(500);
    Console.WriteLine("1단계 완료");
    return "데이터1";
}

async Task<string> Step2Async(string input)
{
    await Task.Delay(500);
    Console.WriteLine("2단계 완료");
    return input + " + 데이터2";
}

async Task<string> Step3Async(string input)
{
    await Task.Delay(500);
    Console.WriteLine("3단계 완료");
    return input + " + 데이터3";
}

async Task RunPipelineAsync()
{
    string result1 = await Step1Async();
    string result2 = await Step2Async(result1);
    string result3 = await Step3Async(result2);
    
    Console.WriteLine($"최종 결과: {result3}");
}

// 출력:
// 1단계 완료
// 2단계 완료
// 3단계 완료
// 최종 결과: 데이터1 + 데이터2 + 데이터3
```

### 예외 처리

```csharp
async Task<string> RiskyOperationAsync()
{
    await Task.Delay(500);
    throw new InvalidOperationException("작업 실패!");
}

async Task HandleExceptionAsync()
{
    try
    {
        string result = await RiskyOperationAsync();
        Console.WriteLine(result);
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine($"오류 처리: {ex.Message}");
    }
}

// 출력:
// 오류 처리: 작업 실패!
```

### 실전 예제 - 여러 파일 동시 처리

```csharp
async Task ProcessFilesAsync(string[] filePaths)
{
    Console.WriteLine($"{filePaths.Length}개 파일 처리 시작...");
    
    // 모든 파일을 동시에 읽기
    Task<string>[] readTasks = filePaths
        .Select(path => File.ReadAllTextAsync(path))
        .ToArray();
    
    try
    {
        // 모든 읽기 작업 완료 대기
        string[] contents = await Task.WhenAll(readTasks);
        
        // 각 파일 내용 처리
        for (int i = 0; i < filePaths.Length; i++)
        {
            Console.WriteLine($"{filePaths[i]}: {contents[i].Length} 문자");
        }
        
        Console.WriteLine("모든 파일 처리 완료!");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"파일 처리 중 오류: {ex.Message}");
    }
}

// 사용
string[] files = { "file1.txt", "file2.txt", "file3.txt" };
await ProcessFilesAsync(files);
```

---

## 17장 정리 및 요약

이 장에서는 C#의 비동기 프로그래밍 기초를 체계적으로 학습했습니다. 비동기 프로그래밍은 현대 소프트웨어 개발의 필수 기술이며, 응답성 있고 확장 가능한 애플리케이션을 구축하는 핵심입니다.

### 핵심 개념의 재조명

**1. 동기 vs 비동기의 근본적 차이**

동기와 비동기의 차이는 단순한 실행 순서가 아니라 **자원 활용**과 **제어 흐름**의 차이입니다:

- **동기(Synchronous)**: 블로킹 방식으로 스레드가 작업 완료까지 대기합니다. 이는 단순하고 이해하기 쉽지만, I/O 바운드 작업에서는 스레드가 "유휴 상태"로 자원을 낭비합니다.

- **비동기(Asynchronous)**: 논블로킹 방식으로 작업을 시작하고 즉시 반환합니다. 스레드는 스레드 풀로 반환되어 다른 작업을 수행할 수 있으며, 작업 완료 시 continuation이 실행됩니다.

**동시성(Concurrency) vs 병렬성(Parallelism)**을 명확히 구분해야 합니다:
- **동시성**: 여러 작업이 논리적으로 동시에 진행 (I/O 작업에 적합)
- **병렬성**: 여러 작업이 물리적으로 동시에 실행 (CPU 바운드 작업에 적합)

비동기는 주로 동시성을 다루며, 소수의 스레드로도 수천 개의 I/O 작업을 효율적으로 처리할 수 있습니다.

**2. async/await의 마법과 내부 동작**

`async`/`await`는 단순한 키워드가 아니라 컴파일러의 정교한 변환입니다:

- **상태 머신 생성**: 컴파일러는 비동기 메서드를 `IAsyncStateMachine`을 구현하는 상태 머신으로 변환합니다.

- **Continuation 캡처**: `await` 이후의 코드는 continuation으로 캡처되어, 비동기 작업 완료 시 실행됩니다.

- **SynchronizationContext**: UI 애플리케이션에서 `await` 이후 코드가 원래 컨텍스트(UI 스레드)에서 실행되도록 자동으로 보장합니다.

- **ConfigureAwait(false)**: 라이브러리 코드에서는 컨텍스트 캡처를 방지하여 성능을 향상시키고 데드락을 방지해야 합니다.

**async/await의 이론적 기반**:
- **Continuation-Passing Style (CPS)**: 프로그램의 나머지 부분을 함수로 전달하는 패턴
- **Monad 이론**: Task는 비동기 모나드로, flatMap (ContinueWith), unit (FromResult) 등을 지원
- **Future/Promise**: 미래에 사용 가능할 값을 나타내는 함수형 프로그래밍 개념

**3. Task의 생명주기와 조율**

Task는 복잡한 상태 머신으로, 생성, 스케줄링, 실행, 완료의 생명주기를 거칩니다:

```
Created → WaitingForActivation → WaitingToRun → Running → 
    → RanToCompletion (성공)
    → Faulted (예외)
    → Canceled (취소)
```

**Task vs ValueTask**: 핫 패스 최적화를 위해 `ValueTask<T>`를 사용하면 자주 동기 완료되는 경로에서 힙 할당을 제거할 수 있습니다.

**Task 조율 패턴**:
- **Task.WhenAll**: 모든 작업 완료 대기 (AND 연산)
- **Task.WhenAny**: 첫 번째 완료 대기 (OR 연산)
- **Sequential Composition**: `await task1; await task2;`
- **Parallel Composition**: `await Task.WhenAll(task1, task2);`

**4. 비동기 메서드 설계 원칙**

비동기 메서드를 올바르게 작성하려면 다음 원칙을 따라야 합니다:

- **All async or none**: 비동기 경로를 시작하면 끝까지 비동기로 유지
- **Avoid async void**: 이벤트 핸들러를 제외하고 절대 사용하지 않기
- **Never block**: `.Result`나 `.Wait()`로 Task를 차단하면 데드락 위험
- **ConfigureAwait in libraries**: 라이브러리 코드는 컨텍스트 캡처 방지
- **Cancel gracefully**: `CancellationToken`을 통한 협력적 취소 지원

**데드락의 메커니즘과 회피**:
데드락은 UI 스레드가 Task 완료를 차단 대기하는 동안, Task의 continuation이 UI 스레드로 돌아오려고 할 때 발생합니다. 해결 방법은 끝까지 비동기를 유지하거나 `ConfigureAwait(false)`를 사용하는 것입니다.

**5. 비동기 패턴과 실무 적용**

실무에서 마주치는 비동기 패턴들:

- **재시도 패턴 (Retry Pattern)**: 일시적 오류에 대한 자동 재시도, 지수 백오프
- **회로 차단기 (Circuit Breaker)**: 연속 실패 시 시스템 보호
- **타임아웃 (Timeout)**: `Task.WhenAny`를 사용한 시간 제한
- **대안 선택 (Fallback)**: 주 소스 실패 시 대체 소스 사용
- **벌킹 (Bulkheading)**: 리소스 격리로 장애 전파 방지

### 비동기 프로그래밍의 실무 가치

**1. 성능과 확장성**

비동기 프로그래밍은 다음과 같은 실측 가능한 이점을 제공합니다:

- **스레드 효율**: 동기 방식에서 1000개 요청 처리 시 1000개 스레드(~1GB 메모리) 필요. 비동기는 소수의 스레드로 처리 가능.

- **처리량 향상**: ASP.NET Core 웹 서버에서 비동기 I/O 사용 시 처리량이 10배 이상 향상될 수 있습니다.

- **응답 시간**: UI 애플리케이션에서 비동기는 60fps 유지에 필수적이며, 프레임 드롭을 방지합니다.

**2. 클라우드 시대의 필수 기술**

마이크로서비스, 서버리스, 클라우드 네이티브 아키텍처에서 비동기는 필수입니다:

- **HTTP API 호출**: 마이크로서비스 간 통신은 모두 비동기로 처리
- **메시지 큐**: 이벤트 기반 아키텍처의 비동기 메시지 처리
- **데이터베이스**: Entity Framework Core 등 모든 ORM이 비동기 지원
- **파일 스토리지**: Azure Blob, S3 등 클라우드 스토리지는 비동기 API 제공

**3. 모바일과 IoT**

배터리와 네트워크가 제한된 환경에서 비동기는 더욱 중요합니다:

- **배터리 효율**: 스레드 차단을 피해 배터리 소모 감소
- **네트워크 최적화**: 동시 요청으로 지연 시간 최소화
- **사용자 경험**: 응답성 있는 UI로 앱 평점 향상

### 비동기 프로그래밍의 함정과 안티패턴

비동기를 사용할 때 피해야 할 일반적인 실수들:

**1. Async Over Sync 안티패턴**
```csharp
// ❌ 나쁜 예
async Task<int> ComputeAsync()
{
    return await Task.Run(() => ExpensiveComputation());
}
// Task.Run은 CPU 바운드 작업에만 사용. I/O는 진정한 비동기 API 사용.
```

**2. Async Void 안티패턴**
```csharp
// ❌ 절대 하지 말 것
async void ProcessDataAsync()  // 예외 추적 불가, 완료 대기 불가
```

**3. Blocking on Async Code**
```csharp
// ❌ 데드락 위험
var result = SomeAsyncMethod().Result;  // 차단 금지!
```

**4. Fire and Forget**
```csharp
// ❌ 예외가 삼켜짐
_ = SomeAsyncMethod();  // Task를 await하지 않음
```

**5. 불필요한 Async/Await**
```csharp
// ❌ 불필요한 상태 머신
async Task<string> GetDataAsync()
{
    return await File.ReadAllTextAsync("file.txt");
}

// ✅ 직접 반환
Task<string> GetDataAsync()
{
    return File.ReadAllTextAsync("file.txt");
}
```

### 비동기 프로그래밍의 미래

C#과 .NET의 비동기 기능은 계속 진화하고 있습니다:

- **IAsyncEnumerable<T>**: 비동기 스트림으로 대량 데이터 처리 (C# 8.0+)
- **ValueTask 최적화**: 더 많은 시나리오에서 제로 할당 달성
- **Channel<T>**: 생산자-소비자 패턴의 고성능 구현
- **System.Threading.Channels**: 비동기 데이터 파이프라인
- **Async Disposable**: `IAsyncDisposable`을 통한 비동기 리소스 정리

**WebAssembly와 Blazor**에서 비동기는 더욱 중요합니다. 브라우저의 단일 스레드 환경에서 모든 I/O는 비동기여야 합니다.

**gRPC와 HTTP/3**: 최신 프로토콜들은 모두 스트리밍과 비동기를 기본으로 지원합니다.

### 학습 리소스와 다음 단계

**권장 도서 및 자료:**

- **"Concurrency in C# Cookbook"** by Stephen Cleary - 비동기 패턴의 바이블
- **"Pro Asynchronous Programming with .NET"** by Richard Blewett & Andrew Clymer
- **Stephen Cleary의 블로그**: https://blog.stephencleary.com/ - 비동기 모범 사례
- **"Async in C# 5.0"** by Alex Davies - async/await 내부 동작
- **Microsoft의 TAP 문서**: Task-based Asynchronous Pattern 공식 가이드

**다음 장 미리보기:**

18장 "고급 비동기 패턴"에서는 다음 주제를 다룹니다:

- **CancellationToken**: 협력적 취소의 완전한 구현
- **IAsyncEnumerable<T>**: 비동기 스트림과 `await foreach`
- **Channel<T>**: 생산자-소비자 패턴과 파이프라인
- **ConfigureAwait 심화**: 커스텀 TaskScheduler와 실행 컨텍스트
- **비동기 성능 최적화**: 프로파일링과 벤치마킹
- **분산 시스템**: 타임아웃, 재시도, 회로 차단기 패턴의 실전 적용

### 실전 활용 시나리오

비동기 프로그래밍은 다음과 같은 경우에 특히 유용합니다:
- 웹 API 호출
- 파일 I/O
- 데이터베이스 쿼리
- 네트워크 통신
- UI 애플리케이션

### 다음 단계

18장에서는 고급 비동기 패턴을 학습하며, 취소 토큰(CancellationToken), 비동기 스트림(IAsyncEnumerable<T>), ConfigureAwait 등의 고급 주제를 다룹니다.

---

## 실습 예제

### 예제 1: 간단한 비동기 다운로드

```csharp
async Task<string> DownloadPageAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        Console.WriteLine($"다운로드 시작: {url}");
        string content = await client.GetStringAsync(url);
        Console.WriteLine($"다운로드 완료: {content.Length} 문자");
        return content;
    }
}

// 사용
string page = await DownloadPageAsync("https://example.com");
```

### 예제 2: 여러 작업 동시 실행

```csharp
async Task<int> ComputeAsync(int value, int delay)
{
    await Task.Delay(delay);
    return value * 2;
}

async Task RunParallelAsync()
{
    var tasks = new[]
    {
        ComputeAsync(1, 1000),
        ComputeAsync(2, 800),
        ComputeAsync(3, 1200)
    };
    
    int[] results = await Task.WhenAll(tasks);
    Console.WriteLine($"결과: {string.Join(", ", results)}");
    // 출력: 결과: 2, 4, 6
}
```

### 예제 3: 파일 처리

```csharp
async Task ProcessLargeFileAsync(string path)
{
    try
    {
        Console.WriteLine("파일 읽기 시작...");
        string content = await File.ReadAllTextAsync(path);
        
        Console.WriteLine("데이터 처리 중...");
        await Task.Delay(1000);  // 처리 시뮬레이션
        
        Console.WriteLine($"처리 완료: {content.Length} 문자");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"오류: {ex.Message}");
    }
}
```

비동기 프로그래밍은 현대 C# 개발의 필수 요소입니다. 이 장에서 배운 기초를 바탕으로 더 복잡한 비동기 패턴을 이해하고 활용할 수 있습니다!
