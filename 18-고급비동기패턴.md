# 18장. 고급 비동기 패턴

17장에서 비동기 프로그래밍의 기초를 학습했다면, 이 장에서는 실무에서 직면하는 복잡한 비동기 시나리오를 다루는 고급 패턴들을 체계적으로 학습합니다. 비동기 프로그래밍의 진정한 힘은 단순히 `async`와 `await`를 사용하는 데 있지 않고, **여러 비동기 작업의 조율(Orchestration)**, **협력적 취소(Cooperative Cancellation)**, **스트리밍 데이터 처리**, 그리고 **실행 컨텍스트 최적화**를 통해 복잡한 시스템을 우아하게 설계하는 데 있습니다.

**고급 비동기 패턴의 역사적 맥락:**

비동기 작업 조율의 개념은 1960년대 **병렬 컴퓨팅(Parallel Computing)**과 **분산 시스템(Distributed Systems)** 연구에서 시작되었습니다. Carl Adam Petri의 **Petri Nets**(1962)는 병렬 작업의 동기화와 조율을 수학적으로 모델링한 초기 연구였습니다. 1970년대 Edsger Dijkstra의 **세마포어(Semaphore)**와 **모니터(Monitor)** 개념은 동시성 제어의 기초를 확립했습니다.

프로그래밍 언어 수준에서 비동기 패턴의 발전은 다음과 같은 궤적을 따랐습니다:

- **1970-80년대**: 저수준 동기화 프리미티브 (뮤텍스, 세마포어, 조건 변수)
- **1990년대**: 고수준 병렬 패턴 (fork-join, map-reduce)
- **2000년대**: Promise/Future 패턴과 Reactive Programming (Rx)
- **2010년대**: Async/Await와 구조화된 동시성 (Structured Concurrency)

.NET의 고급 비동기 패턴은 다음과 같이 진화했습니다:

- **TPL (.NET 4.0, 2010)**: Task Parallel Library, Task.Factory, Task.ContinueWith
- **Async/Await (.NET 4.5, 2012)**: 기본 async/await 지원
- **취소 토큰 표준화 (.NET 4.5+)**: CancellationToken이 비동기 API의 표준이 됨
- **IAsyncEnumerable (C# 8.0, 2019)**: 비동기 스트림, await foreach
- **ValueTask 최적화 (.NET Core 2.0+)**: 핫 패스 성능 향상
- **Channel<T> (.NET Core 2.1+)**: 고성능 생산자-소비자 패턴

**현대 분산 시스템의 도전과제:**

마이크로서비스 아키텍처, 클라우드 네이티브 애플리케이션, 실시간 데이터 처리 시스템의 시대에 고급 비동기 패턴은 더욱 중요해졌습니다. 다음과 같은 도전과제들이 있습니다:

1. **부분 실패(Partial Failure)**: 분산 시스템에서 일부 서비스가 실패해도 전체 시스템이 동작해야 합니다. 이는 **resilience 패턴**(재시도, 회로 차단기, 벌크헤딩)을 필요로 합니다.

2. **지연 시간 최적화(Latency Optimization)**: 마이크로초 단위의 지연도 중요한 고빈도 거래 시스템에서는 비동기 오버헤드를 최소화해야 합니다.

3. **백프레셔(Backpressure)**: 생산자가 소비자보다 빠를 때 시스템이 폭주하지 않도록 흐름 제어가 필요합니다.

4. **리소스 관리**: 수천 개의 동시 연결을 처리하면서도 메모리와 스레드 풀을 효율적으로 관리해야 합니다.

**이론적 기반 - 동시성 이론과 패턴:**

고급 비동기 패턴은 다음과 같은 이론적 개념들에 기반합니다:

1. **Join 패턴**: 여러 비동기 작업의 결과를 조합하는 방법. `Task.WhenAll`은 **And-Join**, `Task.WhenAny`는 **Or-Join**을 구현합니다.

2. **취소의 의미론**: 협력적 취소는 **트랜잭션 시맨틱스(Transaction Semantics)**와 유사합니다. 작업은 취소 요청을 확인하고, 정리(Cleanup) 작업을 수행하며, 일관된 상태로 복귀해야 합니다.

3. **스트림 처리**: `IAsyncEnumerable<T>`는 **Reactive Streams** 사양의 풀 기반(Pull-based) 구현입니다. 이는 Observable (푸시 기반)의 쌍대(Dual)로 볼 수 있습니다.

4. **실행 컨텍스트**: `ConfigureAwait`는 **continuation 스케줄링**을 제어합니다. 이는 **actor 모델**이나 **CSP (Communicating Sequential Processes)**의 메시지 전달과 유사한 개념입니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 비동기 프로그래밍의 고급 패턴과 실무 적용 기법을 마스터하게 됩니다:

- **여러 Task 조율의 심층 이해**: Task.WhenAll과 Task.WhenAny의 내부 구현, 예외 집계(Exception Aggregation), 취소 전파(Cancellation Propagation), 그리고 다양한 조율 패턴(타임아웃, 재시도, 폴백, 헷징)을 학습합니다. 동시 실행 vs 병렬 실행의 차이, 팬아웃/팬인(Fan-out/Fan-in) 패턴, 그리고 대규모 비동기 작업의 최적화 기법을 이해합니다.

- **취소 토큰의 완전한 이해**: CancellationToken의 설계 원리와 구현, 취소 전파 메커니즘, 연결된 취소 소스(Linked Token Source), 그리고 취소와 타임아웃의 통합을 배웁니다. 협력적 취소의 모범 사례, 리소스 정리 패턴, 그리고 취소 시 데이터 일관성 보장 방법을 익힙니다.

- **비동기 스트림의 이론과 실전**: IAsyncEnumerable<T>의 설계 철학과 Reactive Streams의 관계, yield return의 비동기 버전 구현 원리, 그리고 await foreach의 내부 동작을 이해합니다. 대용량 데이터 스트리밍, 백프레셔 처리, LINQ to AsyncEnumerable, 그리고 Channel<T>과의 비교를 학습합니다.

- **ConfigureAwait의 심층 분석**: SynchronizationContext의 역할과 구현, ExecutionContext 캡처, TaskScheduler의 종류와 사용법, 그리고 성능 최적화를 위한 ConfigureAwait(false) 패턴을 완전히 이해합니다. UI 프레임워크별 차이점(WPF, WinForms, MAUI), ASP.NET Core의 특수성, 그리고 라이브러리 vs 애플리케이션 코드의 지침을 배웁니다.

**학습 목표:**

- 복잡한 비동기 워크플로우를 설계하고 구현하는 능력 배양
- 분산 시스템의 resilience 패턴 적용 (재시도, 회로 차단기, 타임아웃)
- 비동기 스트림을 활용한 효율적인 데이터 파이프라인 구축
- 취소와 타임아웃을 통한 사용자 경험 향상
- 성능 프로파일링과 비동기 코드 최적화 기법 습득
- 실무에서 마주치는 고급 비동기 시나리오 해결 능력 확보

---

## 18.1 여러 Task 다루기

여러 개의 비동기 작업을 효율적으로 관리하고 조율하는 것은 현대 소프트웨어 시스템의 핵심 과제입니다. 단일 비동기 작업을 처리하는 것은 비교적 단순하지만, 수십 또는 수백 개의 작업이 상호작용하고, 일부는 성공하고 일부는 실패하며, 각기 다른 속도로 완료되는 복잡한 시나리오를 다루려면 **작업 조율(Task Orchestration)**의 고급 패턴이 필요합니다.

**작업 조율의 이론적 배경:**

작업 조율은 **워크플로우 이론(Workflow Theory)**과 **프로세스 대수(Process Algebra)**에서 연구되어 왔습니다. 특히 다음과 같은 조합 패턴들이 중요합니다:

1. **Sequential Composition** (순차 조합): `await task1; await task2;` - 작업들이 순서대로 실행됩니다. 이는 **모나드의 bind 연산**과 동일합니다.

2. **Parallel Composition** (병렬 조합): `await Task.WhenAll(task1, task2);` - 작업들이 동시에 실행되며, 모두 완료되어야 합니다. 이는 **And-Join** 또는 **Barrier Synchronization**입니다.

3. **Alternative Composition** (대안 조합): `await Task.WhenAny(task1, task2);` - 작업들이 경쟁하며, 하나만 완료되면 됩니다. 이는 **Or-Join** 또는 **Racing**입니다.

4. **Iterative Composition** (반복 조합): `foreach (var item in items) await ProcessAsync(item);` - 작업들이 반복적으로 실행됩니다.

5. **Conditional Composition** (조건 조합): 조건에 따라 다른 작업을 실행합니다.

.NET의 `Task.WhenAll`과 `Task.WhenAny`는 이러한 조율 패턴의 핵심 빌딩 블록입니다. 이들은 단순한 헬퍼 메서드가 아니라, **함수형 프로그래밍의 조합기(Combinator)**로서 복잡한 비동기 워크플로우를 선언적으로 표현할 수 있게 합니다.

**실무에서의 작업 조율 시나리오:**

현대 애플리케이션에서 작업 조율이 필요한 전형적인 시나리오들:

1. **마이크로서비스 조합**: 여러 마이크로서비스에 병렬로 요청하고, 모든 응답을 조합하여 클라이언트에 반환합니다. 예: 제품 상세 페이지가 제품 정보, 재고, 리뷰, 추천을 각각 다른 서비스에서 가져옵니다.

2. **배치 처리**: 수천 개의 파일을 병렬로 처리하되, 시스템 리소스를 초과하지 않도록 동시 실행 수를 제한합니다.

3. **타임아웃과 폴백**: 주 서비스에 요청하고, 일정 시간 내에 응답이 없으면 캐시나 대체 서비스를 사용합니다.

4. **헷징(Hedging)**: 같은 요청을 여러 서버에 보내고, 가장 먼저 도착하는 응답을 사용합니다. 이는 테일 레이턴시(tail latency)를 줄입니다.

5. **재시도와 백오프**: 실패한 작업을 지수 백오프(exponential backoff)와 지터(jitter)를 적용하여 재시도합니다.

**성능과 확장성 고려사항:**

작업 조율 시 다음과 같은 성능 특성을 고려해야 합니다:

- **동시 실행 수 제한**: 무제한 병렬 실행은 리소스 고갈을 초래할 수 있습니다. `SemaphoreSlim`을 사용하여 동시 실행 수를 제한합니다.

- **할당 최소화**: 대량의 Task 생성 시 힙 압력(heap pressure)이 증가합니다. `ValueTask`와 객체 풀링을 고려합니다.

- **공정성(Fairness)**: Task 스케줄링의 공정성을 고려하여 일부 작업이 무한정 대기하지 않도록 합니다.

- **컨텍스트 스위칭**: 과도한 Task 생성은 컨텍스트 스위칭 비용을 증가시킵니다.

### 18.1.1 Task.WhenAll

`Task.WhenAll`은 여러 비동기 작업이 모두 완료될 때까지 대기하는 **And-Join** 패턴을 구현합니다. 이는 병렬 컴퓨팅의 **Barrier Synchronization**과 유사하며, 모든 작업이 동기화 지점에 도달할 때까지 기다립니다. 작업들은 병렬로 실행되므로 순차 실행보다 훨씬 빠르며, 이는 **Amdahl의 법칙**과 **Gustafson의 법칙**이 예측하는 성능 향상을 실현합니다.

**Task.WhenAll의 내부 구현:**

`Task.WhenAll`은 내부적으로 **상태 집계(State Aggregation)**를 수행합니다. 모든 입력 Task의 완료를 추적하고, 마지막 Task가 완료되면 반환되는 Task를 완료 상태로 만듭니다. 의사 코드로 표현하면:

```csharp
// Task.WhenAll의 의사 코드 (간소화)
public static Task<TResult[]> WhenAll<TResult>(params Task<TResult>[] tasks)
{
    if (tasks.Length == 0) return Task.FromResult(Array.Empty<TResult>());
    
    var promise = new TaskCompletionSource<TResult[]>();
    int remaining = tasks.Length;
    var results = new TResult[tasks.Length];
    var exceptions = new List<Exception>();
    
    for (int i = 0; i < tasks.Length; i++)
    {
        int index = i;  // 클로저 캡처
        tasks[i].ContinueWith(t =>
        {
            lock (promise)
            {
                if (t.IsFaulted)
                    exceptions.AddRange(t.Exception.InnerExceptions);
                else if (t.IsCanceled)
                    exceptions.Add(new TaskCanceledException());
                else
                    results[index] = t.Result;
                
                if (Interlocked.Decrement(ref remaining) == 0)
                {
                    if (exceptions.Count > 0)
                        promise.SetException(exceptions);
                    else
                        promise.SetResult(results);
                }
            }
        }, TaskContinuationOptions.ExecuteSynchronously);
    }
    
    return promise.Task;
}
```

이 구현은 다음과 같은 특징을 가집니다:

1. **원자적 카운팅**: `Interlocked.Decrement`를 사용하여 스레드 안전하게 완료된 작업 수를 추적합니다.

2. **예외 집계**: 모든 실패한 Task의 예외를 수집하여 `AggregateException`으로 만듭니다.

3. **지연 없는 완료**: `ExecuteSynchronously`를 사용하여 마지막 Task 완료 시 즉시 처리합니다.

4. **구조적 공유**: 결과 배열은 한 번만 할당되고, 각 Task가 자신의 위치에 결과를 씁니다.

**예외 처리 의미론:**

`Task.WhenAll`의 예외 처리는 독특합니다. 하나 이상의 Task가 실패해도 **모든 Task의 완료를 기다립니다**. 이는 부분적으로 성공한 작업들의 결과를 사용하거나, 모든 리소스를 정리할 수 있게 합니다.

```csharp
async Task DemonstrateExceptionHandlingAsync()
{
    Task<int> task1 = Task.Run(() => throw new InvalidOperationException("Task 1"));
    Task<int> task2 = Task.Run(async () => { await Task.Delay(100); return 42; });
    Task<int> task3 = Task.Run(() => throw new ArgumentException("Task 3"));
    
    try
    {
        // 모든 작업 완료 대기 (실패한 것들도 포함)
        int[] results = await Task.WhenAll(task1, task2, task3);
    }
    catch (Exception ex)
    {
        // await는 첫 번째 예외만 던짐 (언랩)
        Console.WriteLine($"Caught: {ex.GetType().Name} - {ex.Message}");
        
        // 모든 예외에 접근하려면 Task의 Exception 속성 사용
        var whenAllTask = Task.WhenAll(task1, task2, task3);
        var allExceptions = whenAllTask.Exception?.InnerExceptions;
        
        Console.WriteLine($"Total exceptions: {allExceptions?.Count}");
        foreach (var exception in allExceptions ?? Enumerable.Empty<Exception>())
        {
            Console.WriteLine($"  - {exception.GetType().Name}: {exception.Message}");
        }
    }
}

// 출력:
// Caught: InvalidOperationException - Task 1
// Total exceptions: 2
//   - InvalidOperationException: Task 1
//   - ArgumentException: Task 3
```

주의할 점: `await Task.WhenAll(...)`은 첫 번째 예외만 던집니다. 모든 예외를 처리하려면 Task의 `Exception` 속성을 직접 확인해야 합니다.

```csharp
async Task<int> DownloadFileAsync(string url)
{
    await Task.Delay(1000); // 다운로드 시뮬레이션
    return url.Length;
}

async Task DownloadMultipleFilesAsync()
{
    string[] urls = {
        "https://example.com/file1.txt",
        "https://example.com/file2.txt",
        "https://example.com/file3.txt"
    };
    
    // 모든 다운로드를 동시에 시작
    Task<int>[] tasks = urls.Select(url => DownloadFileAsync(url)).ToArray();
    
    // 모든 작업이 완료될 때까지 대기
    int[] results = await Task.WhenAll(tasks);
    
    Console.WriteLine($"총 다운로드 크기: {results.Sum()} 바이트");
}
```

**고급 패턴: 동시 실행 수 제한**

무제한 병렬 실행은 리소스 고갈을 초래할 수 있습니다. `SemaphoreSlim`을 사용하여 동시 실행을 제한하는 패턴:

```csharp
async Task<IEnumerable<TResult>> ParallelWithThrottleAsync<TSource, TResult>(
    IEnumerable<TSource> source,
    int maxConcurrency,
    Func<TSource, Task<TResult>> taskSelector)
{
    using var semaphore = new SemaphoreSlim(maxConcurrency);
    
    async Task<TResult> ProcessWithSemaphoreAsync(TSource item)
    {
        await semaphore.WaitAsync();  // 슬롯 획득
        try
        {
            return await taskSelector(item);
        }
        finally
        {
            semaphore.Release();  // 슬롯 반환
        }
    }
    
    var tasks = source.Select(item => ProcessWithSemaphoreAsync(item)).ToArray();
    return await Task.WhenAll(tasks);
}

// 사용 예제: 최대 10개 파일만 동시 다운로드
var files = Enumerable.Range(1, 100).Select(i => $"file{i}.dat");
var results = await ParallelWithThrottleAsync(files, maxConcurrency: 10, 
    file => DownloadFileAsync(file));
```

**성능 최적화: 파티셔닝**

매우 많은 수의 Task를 처리할 때는 **파티셔닝(Partitioning)**을 통해 배치로 나누어 처리하는 것이 효율적입니다:

```csharp
async Task<IEnumerable<TResult>> ProcessInBatchesAsync<TSource, TResult>(
    IEnumerable<TSource> source,
    int batchSize,
    Func<TSource, Task<TResult>> taskSelector)
{
    var results = new List<TResult>();
    
    // Batch로 나누어 순차적으로 처리
    foreach (var batch in source.Chunk(batchSize))
    {
        var batchTasks = batch.Select(item => taskSelector(item));
        var batchResults = await Task.WhenAll(batchTasks);
        results.AddRange(batchResults);
    }
    
    return results;
}

// 사용: 1000개 작업을 50개씩 20번에 나누어 처리
var items = Enumerable.Range(1, 1000);
var results = await ProcessInBatchesAsync(items, batchSize: 50, 
    item => ProcessItemAsync(item));
```

이 접근법은 메모리 사용량을 제한하고, GC 압력을 줄이며, 시스템이 더 예측 가능하게 동작하도록 합니다.

**Task.WhenAll의 주요 특징:**
- 모든 작업을 병렬로 실행 (동시성, 필요시 병렬성)
- 하나라도 실패하면 `AggregateException` 발생
- 모든 Task가 완료될 때까지 대기 (실패한 것들도 포함)
- 결과를 순서대로 배열로 반환
- 취소된 Task는 `TaskCanceledException`으로 처리

### 18.1.2 Task.WhenAny

`Task.WhenAny`는 여러 작업 중 **하나라도** 완료되면 즉시 반환하는 **Or-Join** 또는 **Racing** 패턴을 구현합니다. 이는 **non-deterministic choice**의 개념을 실현하며, 분산 시스템에서 중요한 패턴들(타임아웃, 폴백, 헷징, 경쟁적 요청)의 기반이 됩니다.

**Task.WhenAny의 설계 철학:**

`Task.WhenAny`는 다음과 같은 시나리오를 우아하게 해결합니다:

1. **타임아웃 구현**: 작업과 지연을 경쟁시켜, 지정 시간 내에 완료되지 않으면 취소하거나 예외를 던집니다.

2. **헷징(Hedged Requests)**: Google의 Jeff Dean이 제안한 패턴으로, 같은 요청을 여러 서버에 보내고 가장 빠른 응답을 사용합니다. 이는 P99 레이턴시를 크게 개선합니다.

3. **폴백(Fallback)**: 주 소스와 대체 소스를 동시에 시도하고, 먼저 성공하는 것을 사용합니다.

4. **진행률 모니터링**: 여러 작업의 진행 상황을 관찰하고, 완료되는 순서대로 처리합니다.

5. **사용자 취소**: 작업과 취소 토큰을 경쟁시켜, 사용자가 취소하면 즉시 반응합니다.

**Task.WhenAny의 내부 구현:**

`Task.WhenAny`는 `Task.WhenAll`보다 단순합니다. 첫 번째로 완료되는 Task를 감지하면 즉시 반환합니다:

```csharp
// Task.WhenAny의 의사 코드
public static Task<Task<T>> WhenAny<T>(params Task<T>[] tasks)
{
    var promise = new TaskCompletionSource<Task<T>>();
    
    foreach (var task in tasks)
    {
        task.ContinueWith(completedTask =>
        {
            // 첫 번째 완료만 처리 (TrySetResult는 한 번만 성공)
            promise.TrySetResult(completedTask);
        }, TaskContinuationOptions.ExecuteSynchronously);
    }
    
    return promise.Task;
}
```

중요한 점: `Task.WhenAny`는 완료된 Task 자체를 반환합니다 (`Task<Task<T>>`). 결과를 얻으려면 다시 `await`해야 합니다.

```csharp
Task<string> completedTask = await Task.WhenAny(task1, task2);
string result = await completedTask;  // 또는 completedTask.Result (동기)
```

**패턴 1: 타임아웃 구현**

가장 일반적인 사용 사례는 작업에 시간 제한을 두는 것입니다:

```csharp
async Task<string> GetFastestResponseAsync()
{
    Task<string> server1 = DownloadFromServerAsync("Server1");
    Task<string> server2 = DownloadFromServerAsync("Server2");
    Task<string> server3 = DownloadFromServerAsync("Server3");
    
    // 가장 먼저 완료되는 작업 대기
    Task<string> completedTask = await Task.WhenAny(server1, server2, server3);
    
    // 결과 반환
    return await completedTask;
}

// 타임아웃 구현 예제
async Task<string> DownloadWithTimeoutAsync(string url, TimeSpan timeout)
{
    Task<string> downloadTask = DownloadAsync(url);
    Task delayTask = Task.Delay(timeout);
    
    Task completedTask = await Task.WhenAny(downloadTask, delayTask);
    
    if (completedTask == delayTask)
    {
        throw new TimeoutException("다운로드 시간 초과");
    }
    
    return await downloadTask;
}
```

더 견고한 타임아웃 구현은 취소 토큰도 함께 사용합니다:

```csharp
async Task<T> WithTimeoutAsync<T>(Task<T> task, TimeSpan timeout, 
    CancellationToken cancellationToken = default)
{
    using var cts = CancellationTokenSource.CreateLinkedTokenSource(cancellationToken);
    var delayTask = Task.Delay(timeout, cts.Token);
    
    var completedTask = await Task.WhenAny(task, delayTask);
    
    if (completedTask == delayTask)
    {
        throw new TimeoutException(
            $"Operation timed out after {timeout.TotalSeconds}s");
    }
    
    cts.Cancel();  // delay 취소하여 리소스 정리
    return await task;
}
```

**패턴 2: 헷징(Hedging) - 테일 레이턴시 개선**

헷징은 동일한 요청을 여러 서버에 보내고, 가장 빠른 응답을 사용하는 패턴입니다. 이는 느린 서버(예: GC 중이거나 과부하)로 인한 테일 레이턴시를 극적으로 개선합니다:

```csharp
async Task<TResult> HedgedRequestAsync<TResult>(
    Func<Task<TResult>> primaryRequest,
    Func<Task<TResult>> hedgeRequest,
    TimeSpan hedgeDelay)
{
    var primaryTask = primaryRequest();
    
    // 짧은 지연 후 헷지 요청 시작
    var hedgeDelayTask = Task.Delay(hedgeDelay);
    var firstCompleted = await Task.WhenAny(primaryTask, hedgeDelayTask);
    
    if (firstCompleted == primaryTask)
    {
        // 주 요청이 빨리 완료됨
        return await primaryTask;
    }
    
    // 주 요청이 느림 - 헷지 요청 시작
    var hedgeTask = hedgeRequest();
    var winner = await Task.WhenAny(primaryTask, hedgeTask);
    
    return await winner;
}

// 사용 예제
var result = await HedgedRequestAsync(
    primaryRequest: () => CallServer("primary-server"),
    hedgeRequest: () => CallServer("backup-server"),
    hedgeDelay: TimeSpan.FromMilliseconds(100)  // 100ms 후 헷징
);
```

**패턴 3: 재시도와 폴백의 결합**

여러 소스를 시도하되, 실패한 것은 건너뛰고 다음 것을 시도하는 패턴:

```csharp
async Task<T> TryMultipleSourcesAsync<T>(params Func<Task<T>>[] sources)
{
    var tasks = new List<Task<T>>();
    var exceptions = new List<Exception>();
    
    foreach (var source in sources)
    {
        tasks.Add(source());
        
        while (tasks.Count > 0)
        {
            var completedTask = await Task.WhenAny(tasks);
            tasks.Remove(completedTask);
            
            try
            {
                return await completedTask;  // 성공!
            }
            catch (Exception ex)
            {
                exceptions.Add(ex);
                // 실패 - 다음 소스 시도
                break;  // 내부 while 종료, 외부 foreach 계속
            }
        }
    }
    
    // 모든 소스 실패
    throw new AggregateException("All sources failed", exceptions);
}

// 사용
var data = await TryMultipleSourcesAsync(
    () => FetchFromPrimaryAsync(),
    () => FetchFromSecondaryAsync(),
    () => FetchFromCacheAsync()
);
```

**패턴 4: 진행률 모니터링 - 완료 순서대로 처리**

여러 작업을 시작하고, 완료되는 순서대로 처리하는 패턴:

```csharp
async Task ProcessAsCompletedAsync<T>(IEnumerable<Task<T>> tasks)
{
    var pending = new HashSet<Task<T>>(tasks);
    
    while (pending.Count > 0)
    {
        var completed = await Task.WhenAny(pending);
        pending.Remove(completed);
        
        try
        {
            var result = await completed;
            Console.WriteLine($"Processed: {result}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed: {ex.Message}");
        }
    }
}

// 사용
var downloadTasks = urls.Select(url => DownloadAsync(url));
await ProcessAsCompletedAsync(downloadTasks);
```

.NET 6+에서는 `Task.WaitAsync` 메서드를 사용하여 더 간결하게 타임아웃을 구현할 수 있습니다:

```csharp
// .NET 6+ - 통합된 타임아웃 API
try
{
    var result = await longRunningTask.WaitAsync(TimeSpan.FromSeconds(30));
}
catch (TimeoutException)
{
    Console.WriteLine("Operation timed out");
}
```

**취소 전파 고려사항:**

`Task.WhenAny`를 사용할 때, 완료되지 않은 다른 Task들을 어떻게 처리할지 고려해야 합니다:

```csharp
async Task<string> DownloadWithCancellationAsync(string url, TimeSpan timeout)
{
    using var cts = new CancellationTokenSource();
    
    var downloadTask = DownloadAsync(url, cts.Token);
    var timeoutTask = Task.Delay(timeout);
    
    var completed = await Task.WhenAny(downloadTask, timeoutTask);
    
    if (completed == timeoutTask)
    {
        cts.Cancel();  // 다운로드 취소
        throw new TimeoutException("Download timed out");
    }
    
    return await downloadTask;
}
```

**Task.WhenAny의 주요 활용 사례:**
- **타임아웃 구현**: 작업에 시간 제한 설정
- **헷징**: 여러 소스 중 가장 빠른 응답 선택하여 테일 레이턴시 개선
- **폴백**: 주 소스 실패 시 대체 소스 사용
- **진행률 모니터링**: 완료되는 순서대로 작업 처리
- **사용자 취소**: 작업과 취소 이벤트를 경쟁시켜 즉시 반응
- **재시도 로직**: 최초 성공을 선택하는 재시도 패턴 구현

---

## 18.2 취소 토큰 (CancellationToken)

비동기 작업은 본질적으로 시간이 오래 걸릴 수 있으므로, 사용자가 작업을 중단할 수 있는 메커니즘이 필수적입니다. `CancellationToken`은 .NET의 **협력적 취소(Cooperative Cancellation)** 모델을 구현하는 표준 메커니즘으로, 강제 종료(Forced Termination)의 위험성을 피하면서도 우아한 취소를 가능하게 합니다.

**취소의 이론적 기반:**

취소(Cancellation)는 단순히 작업을 멈추는 것 이상의 의미를 가집니다. 이는 **트랜잭션 시맨틱스(Transaction Semantics)**와 유사한 개념으로, 다음과 같은 속성을 만족해야 합니다:

1. **안전성(Safety)**: 취소 시 시스템이 일관된 상태(Consistent State)를 유지해야 합니다.

2. **활성성(Liveness)**: 취소 요청이 결국에는 처리되어야 합니다 (무한 대기 방지).

3. **협력성(Cooperativeness)**: 작업이 자발적으로 취소 요청을 확인하고 응답해야 합니다.

4. **구성 가능성(Composability)**: 여러 취소 소스를 조합할 수 있어야 합니다.

5. **전파성(Propagation)**: 취소가 호출 체인을 따라 전파되어야 합니다.

**CancellationToken의 설계 철학:**

.NET의 취소 모델은 다음과 같은 설계 원칙을 따릅니다:

1. **협력적 취소**: 스레드를 강제로 중단(`Thread.Abort`)하지 않습니다. 이는 비결정적 상태와 리소스 누수를 방지합니다.

2. **경량 구조체**: `CancellationToken`은 값 타입(struct)으로, 매개변수로 전달 시 오버헤드가 최소화됩니다.

3. **불변성**: 토큰은 불변이며, 취소 요청은 소스(`CancellationTokenSource`)를 통해 이루어집니다.

4. **통합성**: 모든 .NET 비동기 API가 `CancellationToken` 매개변수를 지원합니다.

**CancellationToken의 내부 구조:**

```csharp
// CancellationToken의 간소화된 구조
public readonly struct CancellationToken
{
    private readonly CancellationTokenSource _source;
    
    public bool IsCancellationRequested => 
        _source != null && _source.IsCancellationRequested;
    
    public void ThrowIfCancellationRequested()
    {
        if (IsCancellationRequested)
            throw new OperationCanceledException(this);
    }
    
    public CancellationTokenRegistration Register(Action callback)
    {
        return _source?.Register(callback) ?? default;
    }
}

// CancellationTokenSource의 핵심
public class CancellationTokenSource : IDisposable
{
    private volatile int _state;  // 0: not canceled, 1: canceling, 2: canceled
    private List<CancellationTokenRegistration> _registrations;
    
    public void Cancel()
    {
        if (Interlocked.CompareExchange(ref _state, 1, 0) != 0)
            return;  // 이미 취소됨
        
        // 모든 등록된 콜백 실행
        foreach (var registration in _registrations)
        {
            try { registration.Invoke(); }
            catch { /* 콜백 예외는 삼킴 */ }
        }
        
        _state = 2;
    }
}
```

**취소의 생명주기:**

```
Created → Running → Cancellation Requested → Canceling → Canceled
                    ↓
                    작업이 주기적으로 확인
                    ↓
                    OperationCanceledException 발생
                    ↓
                    리소스 정리 (finally, Dispose)
                    ↓
                    취소 전파 (상위 호출자에게)
```

### 기본 사용법

취소 토큰의 기본적인 사용 패턴은 다음과 같습니다:

```csharp
async Task DownloadLargeFileAsync(string url, CancellationToken cancellationToken)
{
    for (int i = 0; i < 100; i++)
    {
        // 취소 요청 확인 - OperationCanceledException 발생
        cancellationToken.ThrowIfCancellationRequested();
        
        // 작업 수행
        await DownloadChunkAsync(url, i, cancellationToken);
        
        Console.WriteLine($"진행률: {i + 1}%");
    }
}

// 사용 예제
async Task UseDownloadAsync()
{
    var cts = new CancellationTokenSource();
    
    try
    {
        // 5초 후 자동 취소
        cts.CancelAfter(TimeSpan.FromSeconds(5));
        
        await DownloadLargeFileAsync("https://example.com/large.zip", cts.Token);
        Console.WriteLine("다운로드 완료");
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("다운로드가 취소되었습니다.");
    }
    finally
    {
        cts.Dispose();  // 리소스 정리
    }
}
```

**ThrowIfCancellationRequested vs IsCancellationRequested:**

두 가지 취소 확인 방법이 있으며, 상황에 따라 선택합니다:

```csharp
// 방법 1: 예외 발생 (권장 - 명시적)
cancellationToken.ThrowIfCancellationRequested();

// 방법 2: 부울 검사 (세밀한 제어 필요 시)
if (cancellationToken.IsCancellationRequested)
{
    // 정리 작업
    CleanupResources();
    return;  // 또는 throw new OperationCanceledException(cancellationToken);
}
```

`ThrowIfCancellationRequested()`를 사용하는 것이 권장됩니다. 이유는:
1. 취소 시맨틱스가 명확합니다 (예외를 통한 제어 흐름).
2. 호출 스택을 통해 취소가 자동으로 전파됩니다.
3. Task가 `TaskStatus.Canceled` 상태로 완료됩니다.

**취소 전파 패턴:**

비동기 메서드가 다른 비동기 메서드를 호출할 때, 취소 토큰을 전파해야 합니다:

```csharp
async Task ProcessDataAsync(CancellationToken cancellationToken)
{
    // 취소 토큰을 하위 작업으로 전파
    var data = await LoadDataAsync(cancellationToken);
    
    cancellationToken.ThrowIfCancellationRequested();
    
    var transformed = await TransformDataAsync(data, cancellationToken);
    
    cancellationToken.ThrowIfCancellationRequested();
    
    await SaveDataAsync(transformed, cancellationToken);
}
```

**리소스 정리와 취소:**

취소 시에도 리소스를 올바르게 정리해야 합니다:

```csharp
async Task ProcessFileAsync(string path, CancellationToken cancellationToken)
{
    using var stream = new FileStream(path, FileMode.Open);
    using var reader = new StreamReader(stream);
    
    try
    {
        while (!reader.EndOfStream)
        {
            cancellationToken.ThrowIfCancellationRequested();
            
            var line = await reader.ReadLineAsync();
            await ProcessLineAsync(line, cancellationToken);
        }
    }
    catch (OperationCanceledException)
    {
        // 취소됨 - using이 리소스를 정리함
        throw;  // 예외 재발생
    }
}
```

### 여러 토큰 연결하기

실무에서는 여러 취소 조건을 조합해야 하는 경우가 많습니다. 예를 들어, 사용자 취소와 타임아웃을 모두 지원하거나, 여러 병렬 작업 중 하나라도 취소되면 모두 취소해야 하는 경우입니다. `CancellationTokenSource.CreateLinkedTokenSource`가 이를 우아하게 해결합니다.

```csharp
async Task ProcessWithMultipleCancellationAsync()
{
    var userCts = new CancellationTokenSource(); // 사용자 취소
    var timeoutCts = new CancellationTokenSource(TimeSpan.FromSeconds(30)); // 타임아웃
    
    // 여러 토큰을 하나로 결합
    using var linkedCts = CancellationTokenSource.CreateLinkedTokenSource(
        userCts.Token, 
        timeoutCts.Token
    );
    
    try
    {
        await LongRunningOperationAsync(linkedCts.Token);
    }
    catch (OperationCanceledException)
    {
        if (userCts.Token.IsCancellationRequested)
            Console.WriteLine("사용자가 취소했습니다.");
        else if (timeoutCts.Token.IsCancellationRequested)
            Console.WriteLine("시간 초과로 취소되었습니다.");
    }
    finally
    {
        userCts.Dispose();
        timeoutCts.Dispose();
    }
}
```

**연결된 토큰의 동작 원리:**

`CreateLinkedTokenSource`는 내부적으로 각 소스 토큰에 콜백을 등록하고, 어느 하나라도 취소되면 연결된 소스를 취소합니다:

```csharp
// CreateLinkedTokenSource의 의사 코드
public static CancellationTokenSource CreateLinkedTokenSource(
    params CancellationToken[] tokens)
{
    var linkedSource = new CancellationTokenSource();
    
    foreach (var token in tokens)
    {
        if (token.IsCancellationRequested)
        {
            linkedSource.Cancel();
            return linkedSource;
        }
        
        token.Register(() => linkedSource.Cancel());
    }
    
    return linkedSource;
}
```

**고급 패턴: 조건부 취소**

특정 조건에서만 취소를 전파하고 싶을 때:

```csharp
async Task ConditionalCancellationAsync(CancellationToken outerToken)
{
    using var innerCts = new CancellationTokenSource();
    
    // 외부 토큰 취소 시 조건 확인 후 내부 토큰 취소
    using var registration = outerToken.Register(() =>
    {
        if (ShouldPropagateCancel())
        {
            innerCts.Cancel();
        }
    });
    
    await ProcessAsync(innerCts.Token);
}
```

**취소 토큰과 IDisposable:**

`CancellationTokenSource`는 `IDisposable`을 구현합니다. 특히 타이머를 사용하는 경우(`CancelAfter`) 반드시 Dispose해야 합니다:

```csharp
// ✅ 올바른 사용 - using 사용
async Task CorrectUsageAsync()
{
    using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
    await OperationAsync(cts.Token);
}

// ❌ 잘못된 사용 - 타이머가 계속 실행됨
async Task IncorrectUsageAsync()
{
    var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
    await OperationAsync(cts.Token);
    // Dispose 안함 - 리소스 누수!
}
```

**취소 콜백 등록:**

취소 시 정리 작업이나 알림을 수행하려면 콜백을 등록할 수 있습니다:

```csharp
async Task WithCancellationCallbackAsync(CancellationToken cancellationToken)
{
    // 취소 시 실행될 콜백 등록
    using var registration = cancellationToken.Register(() =>
    {
        Console.WriteLine("취소 요청됨 - 정리 작업 수행");
        CleanupResources();
    });
    
    try
    {
        await LongOperationAsync(cancellationToken);
    }
    catch (OperationCanceledException)
    {
        // 콜백이 이미 실행됨
        Console.WriteLine("작업이 취소되었습니다.");
    }
}
```

주의: 콜백은 `Cancel()`을 호출한 스레드에서 **동기적으로** 실행됩니다. 따라서 콜백은 빠르고 간단해야 하며, 예외를 던져서는 안 됩니다.

**CancellationToken.None:**

취소를 지원하지 않는 경우 `CancellationToken.None`을 사용합니다:

```csharp
// 취소 불가능한 작업
await ProcessAsync(CancellationToken.None);

// 선택적 취소 토큰 매개변수
async Task OptionalCancellationAsync(
    CancellationToken cancellationToken = default)
{
    // default는 CancellationToken.None과 동일
    cancellationToken.ThrowIfCancellationRequested();
}
```

**모범 사례:**
- **비동기 메서드 시그니처**: 마지막 매개변수로 `CancellationToken`을 추가하고 기본값을 `default`로 설정
- **주기적 확인**: I/O 바운드 작업 전후, 반복문 시작 시 취소 확인
- **전파**: 호출하는 모든 비동기 API에 토큰 전달
- **정리**: `CancellationTokenSource`를 반드시 Dispose (using 사용)
- **UI 통합**: 사용자 인터페이스에서 취소 버튼 제공
- **타임아웃**: 작업에 합리적인 타임아웃 설정
- **예외 처리**: `OperationCanceledException`을 별도로 처리하여 로깅/사용자 알림
- **콜백 주의**: 등록된 콜백은 빠르고 예외 안전해야 함

---

## 18.3 비동기 스트림 (IAsyncEnumerable<T>)

C# 8.0에서 도입된 `IAsyncEnumerable<T>`는 비동기적으로 데이터를 생성하고 소비할 수 있게 해주는 **비동기 반복자(Asynchronous Iterator)** 패턴입니다. 이는 대량의 데이터를 처리할 때 메모리를 절약하고 응답성을 향상시키며, **Reactive Streams** 사양의 풀 기반(Pull-based) 구현으로 볼 수 있습니다.

**비동기 스트림의 이론적 기반:**

비동기 스트림은 다음과 같은 개념들의 융합입니다:

1. **Iterator 패턴**: Gang of Four의 디자인 패턴으로, 컬렉션의 요소를 순차적으로 접근합니다.

2. **Generator (코루틴)**: Python의 generator나 JavaScript의 generator function과 유사하게, `yield return`으로 값을 하나씩 생성합니다.

3. **Reactive Streams**: 비동기 스트림 처리의 표준으로, 백프레셔(Backpressure)를 지원합니다.

4. **Pull vs Push**: `IAsyncEnumerable`은 소비자가 **요청(Pull)**할 때 데이터를 생산합니다. 이는 `IObservable`의 푸시(Push) 모델과 쌍대(Dual)입니다.

**IAsyncEnumerable의 수학적 쌍대성:**

```
IEnumerable<T>    ←→  IObservable<T>
     ↓                      ↓
IAsyncEnumerable<T> ←→ (Future) IAsyncObservable<T>
```

- `IEnumerable<T>`: 동기 풀 기반 (현재)
- `IObservable<T>`: 동기 푸시 기반 (Reactive Extensions - Rx)
- `IAsyncEnumerable<T>`: 비동기 풀 기반 (C# 8.0+)
- `IAsyncObservable<T>`: 비동기 푸시 기반 (제안 단계)

**비동기 스트림이 해결하는 문제:**

전통적인 방식의 문제점:

```csharp
// ❌ 나쁜 예: 모든 데이터를 메모리에 로드
async Task<List<string>> LoadAllLinesAsync(string path)
{
    var lines = new List<string>();
    using var reader = new StreamReader(path);
    
    while (!reader.EndOfStream)
    {
        lines.Add(await reader.ReadLineAsync());
    }
    
    return lines;  // 10GB 파일이면? OOM!
}

// ✅ 좋은 예: 스트리밍 처리
async IAsyncEnumerable<string> StreamLinesAsync(string path)
{
    using var reader = new StreamReader(path);
    
    while (!reader.EndOfStream)
    {
        yield return await reader.ReadLineAsync();
    }
    // 메모리 사용량: O(1) - 한 줄씩만 메모리에 유지
}
```

**IAsyncEnumerable의 내부 구조:**

```csharp
// IAsyncEnumerable 인터페이스
public interface IAsyncEnumerable<out T>
{
    IAsyncEnumerator<T> GetAsyncEnumerator(
        CancellationToken cancellationToken = default);
}

// IAsyncEnumerator 인터페이스
public interface IAsyncEnumerator<out T> : IAsyncDisposable
{
    T Current { get; }
    ValueTask<bool> MoveNextAsync();
}

// IAsyncDisposable - 비동기 리소스 정리
public interface IAsyncDisposable
{
    ValueTask DisposeAsync();
}
```

**컴파일러 변환:**

`await foreach`는 컴파일러가 복잡한 상태 머신으로 변환합니다:

```csharp
// 원본 코드
await foreach (var item in GetItemsAsync())
{
    Console.WriteLine(item);
}

// 대략적인 변환 결과
await using (var enumerator = GetItemsAsync().GetAsyncEnumerator())
{
    while (await enumerator.MoveNextAsync())
    {
        var item = enumerator.Current;
        Console.WriteLine(item);
    }
}
```

### 기본 개념

비동기 스트림을 생성하고 소비하는 기본 패턴:

```csharp
// 비동기 스트림 생성 (Producer)
async IAsyncEnumerable<int> GenerateNumbersAsync(int count)
{
    for (int i = 0; i < count; i++)
    {
        await Task.Delay(100); // 비동기 작업 시뮬레이션
        yield return i;
    }
}

// 비동기 스트림 소비 (Consumer)
async Task ConsumeNumbersAsync()
{
    await foreach (int number in GenerateNumbersAsync(10))
    {
        Console.WriteLine($"받은 숫자: {number}");
    }
}
```

**yield return의 의미:**

`yield return`은 **게으른 평가(Lazy Evaluation)**를 구현합니다. 값은 소비자가 요청할 때만 생성되며, 생산자의 상태는 `yield` 지점에서 보존됩니다.

```csharp
async IAsyncEnumerable<int> FibonacciAsync()
{
    int a = 0, b = 1;
    
    while (true)  // 무한 시퀀스!
    {
        yield return a;
        
        await Task.Delay(10);  // 비동기 작업 가능
        
        (a, b) = (b, a + b);  // 상태 업데이트
    }
}

// 처음 10개만 가져오기 - 나머지는 생성되지 않음
await foreach (int fib in FibonacciAsync().Take(10))
{
    Console.WriteLine(fib);
}
```

**LINQ to AsyncEnumerable:**

`System.Linq.Async` NuGet 패키지는 비동기 스트림을 위한 LINQ 연산자를 제공합니다:

```csharp
using System.Linq;

// 비동기 LINQ 쿼리
var query = FetchDataAsync()
    .Where(x => x.IsActive)
    .Select(x => x.Name)
    .Take(100);

await foreach (var name in query)
{
    Console.WriteLine(name);
}

// 집계 연산 (전체 스트림 소비)
int count = await FetchDataAsync().CountAsync();
decimal avg = await GetScoresAsync().AverageAsync();
```

### 실전 예제: 대용량 파일 처리

비동기 스트림은 대용량 데이터 처리에 이상적입니다:

```csharp
async IAsyncEnumerable<string> ReadLinesAsync(string filePath)
{
    using var reader = new StreamReader(filePath);
    
    while (!reader.EndOfStream)
    {
        string line = await reader.ReadLineAsync();
        yield return line;
    }
}

async Task ProcessLargeFileAsync(string filePath)
{
    int lineCount = 0;
    
    await foreach (string line in ReadLinesAsync(filePath))
    {
        // 각 줄을 처리 - 메모리에 한 줄만 유지
        Console.WriteLine(line);
        lineCount++;
    }
    
    Console.WriteLine($"총 {lineCount}줄 처리 완료");
}
```

**실전 시나리오: 페이지네이션 API**

REST API의 페이지네이션을 비동기 스트림으로 추상화:

```csharp
async IAsyncEnumerable<User> FetchAllUsersAsync(HttpClient client)
{
    int page = 1;
    
    while (true)
    {
        var response = await client.GetStringAsync(
            $"/api/users?page={page}&size=100");
        
        var users = JsonSerializer.Deserialize<List<User>>(response);
        
        if (users == null || users.Count == 0)
            break;  // 마지막 페이지
        
        foreach (var user in users)
        {
            yield return user;  // 한 번에 하나씩 반환
        }
        
        page++;
    }
}

// 사용: 모든 페이지를 투명하게 순회
await foreach (var user in FetchAllUsersAsync(httpClient))
{
    Console.WriteLine($"{user.Name}: {user.Email}");
}
```

**백프레셔와 흐름 제어:**

비동기 스트림은 자연스럽게 백프레셔를 지원합니다. 소비자가 느리면 생산자도 자동으로 느려집니다:

```csharp
async IAsyncEnumerable<Message> StreamMessagesAsync()
{
    while (true)
    {
        var message = await ReceiveMessageAsync();  // 느린 생산
        yield return message;
        
        // 소비자가 처리할 때까지 다음 메시지를 가져오지 않음
    }
}

await foreach (var msg in StreamMessagesAsync())
{
    await ProcessMessageAsync(msg);  // 느린 소비
    // 처리 완료될 때까지 다음 메시지를 요청하지 않음
}
```

### 취소 토큰과 함께 사용

비동기 스트림은 `[EnumeratorCancellation]` 속성을 통해 취소를 우아하게 지원합니다:

```csharp
async IAsyncEnumerable<string> FetchDataAsync(
    [EnumeratorCancellation] CancellationToken cancellationToken = default)
{
    for (int i = 0; i < 100; i++)
    {
        cancellationToken.ThrowIfCancellationRequested();
        
        await Task.Delay(100, cancellationToken);
        yield return $"데이터 {i}";
    }
}

async Task UseAsyncStreamWithCancellationAsync()
{
    using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
    
    try
    {
        await foreach (string data in FetchDataAsync(cts.Token))
        {
            Console.WriteLine(data);
        }
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("스트림이 취소되었습니다.");
    }
}
```

**[EnumeratorCancellation] 속성의 동작:**

이 속성은 컴파일러에게 `await foreach`에서 전달된 취소 토큰을 메서드 매개변수와 병합하도록 지시합니다:

```csharp
// 명시적으로 토큰 전달
await foreach (var item in StreamAsync(cts.Token))
{
    // ...
}

// GetAsyncEnumerator에 토큰 전달 (덜 일반적)
await using var enumerator = StreamAsync().GetAsyncEnumerator(cts.Token);
while (await enumerator.MoveNextAsync())
{
    var item = enumerator.Current;
    // ...
}
```

**비동기 스트림 vs Channel<T>:**

두 패턴 모두 비동기 데이터 스트림을 다루지만 다른 용도에 적합합니다:

| 특성 | IAsyncEnumerable<T> | Channel<T> |
|------|---------------------|------------|
| 패턴 | 풀(Pull) 기반 | 푸시(Push) / 풀(Pull) 혼합 |
| 생산자-소비자 | 1:1 | 1:N, N:1, N:M 가능 |
| 백프레셔 | 자동 (소비자가 제어) | 명시적 (BoundedCapacity) |
| 버퍼링 | 없음 (게으름) | 있음 (설정 가능) |
| 사용 사례 | 순차 스트림, 페이지네이션 | 병렬 처리, 파이프라인 |

```csharp
// Channel<T>를 IAsyncEnumerable<T>로 변환
async IAsyncEnumerable<T> AsAsyncEnumerable<T>(
    ChannelReader<T> reader,
    [EnumeratorCancellation] CancellationToken cancellationToken = default)
{
    while (await reader.WaitToReadAsync(cancellationToken))
    {
        while (reader.TryRead(out var item))
        {
            yield return item;
        }
    }
}
```

**장점:**
- **메모리 효율적**: O(1) 메모리 사용 - 한 번에 하나씩 처리
- **응답성 향상**: 데이터가 도착하는 즉시 처리 시작
- **취소 지원**: CancellationToken 통합
- **LINQ 호환**: `System.Linq.Async` 패키지로 강력한 쿼리 작성
- **백프레셔 자동**: 소비자 속도에 맞춰 생산 자동 조절
- **무한 시퀀스**: 무한 스트림을 게으르게 처리 가능

**주의사항:**
- 비동기 스트림은 한 번만 열거 가능 (일반적으로)
- 예외 처리 시 스트림이 중단됨
- 리소스 정리를 위해 `IAsyncDisposable` 구현 고려
- 핫 패스에서는 `ValueTask` 최적화 활용

---

## 18.4 ConfigureAwait

`ConfigureAwait`는 비동기 작업이 완료된 후 **실행 컨텍스트(Execution Context)**를 어떻게 처리할지 제어하는 메커니즘입니다. 올바른 사용은 성능 최적화와 데드락 방지에 결정적이며, 라이브러리 vs 애플리케이션 코드의 구분에서 중요한 역할을 합니다.

**ConfigureAwait의 이론적 기반:**

`ConfigureAwait`는 **Continuation Scheduling**을 제어합니다. `await` 이후의 코드(continuation)를 어느 컨텍스트에서 실행할지 결정하는 것입니다. 이는 다음과 같은 개념들과 연결됩니다:

1. **SynchronizationContext**: .NET의 스레드 스케줄링 추상화로, UI 프레임워크(WPF, WinForms)는 UI 스레드로 작업을 마샬링합니다.

2. **ExecutionContext**: 스레드 로컬 상태, 보안 컨텍스트, 로깅 컨텍스트 등을 캡처하고 복원합니다.

3. **TaskScheduler**: Task의 실행을 스케줄링하는 객체로, 기본은 `ThreadPoolTaskScheduler`입니다.

4. **Actor 모델**: ConfigureAwait는 actor 시스템의 메시지 큐 선택과 유사합니다.

**SynchronizationContext의 역할:**

SynchronizationContext는 작업을 특정 컨텍스트(일반적으로 특정 스레드)로 마샬링하는 메커니즘입니다:

```
[UI Thread]
    ↓
await DownloadAsync()  ← SynchronizationContext.Current 캡처
    ↓
[Thread Pool] 실제 다운로드 수행
    ↓
await 완료
    ↓
SynchronizationContext.Post() ← continuation을 UI 스레드로 포스트
    ↓
[UI Thread] ← continuation 실행
textBox.Text = result; ← UI 업데이트 안전
```

**ConfigureAwait의 내부 동작:**

```csharp
// ConfigureAwait의 의사 코드
public ConfiguredTaskAwaitable ConfigureAwait(bool continueOnCapturedContext)
{
    return new ConfiguredTaskAwaitable(this, continueOnCapturedContext);
}

public struct ConfiguredTaskAwaitable
{
    private readonly Task _task;
    private readonly bool _continueOnCapturedContext;
    
    public ConfiguredTaskAwaiter GetAwaiter()
    {
        return new ConfiguredTaskAwaiter(_task, _continueOnCapturedContext);
    }
}

public struct ConfiguredTaskAwaiter : ICriticalNotifyCompletion
{
    public void OnCompleted(Action continuation)
    {
        if (_continueOnCapturedContext)
        {
            // 기본 동작: SynchronizationContext 사용
            var context = SynchronizationContext.Current;
            if (context != null && context != _defaultContext)
            {
                context.Post(_ => continuation(), null);
                return;
            }
        }
        
        // ConfigureAwait(false): 스레드 풀에서 직접 실행
        ThreadPool.QueueUserWorkItem(_ => continuation(), null);
    }
}
```

**프레임워크별 SynchronizationContext:**

| 프레임워크 | SynchronizationContext | 특징 |
|-----------|------------------------|------|
| WPF | DispatcherSynchronizationContext | Dispatcher 기반, UI 스레드 |
| WinForms | WindowsFormsSynchronizationContext | Control.Invoke 기반 |
| ASP.NET (Legacy) | AspNetSynchronizationContext | 요청 컨텍스트 보존 |
| ASP.NET Core | null | 성능을 위해 없음! |
| Console App | null | 기본 없음 |
| xUnit | null | 테스트 프레임워크 |
| Unity | UnitySynchronizationContext | 메인 스레드 |

### 기본 개념

ConfigureAwait(false)의 의미와 사용법:

```csharp
// UI 애플리케이션 (WPF, WinForms, MAUI)
async Task UpdateUIAsync()
{
    // UI 스레드에서 시작
    string data = await DownloadDataAsync(); // 기본: UI 스레드로 복귀
    
    // UI 스레드에서 실행 - 안전함
    textBox.Text = data;
}

// 라이브러리 코드 - 성능 최적화
async Task<string> LibraryMethodAsync()
{
    // ConfigureAwait(false)로 컨텍스트 캡처 방지
    string data = await DownloadDataAsync().ConfigureAwait(false);
    
    // 스레드 풀 스레드에서 실행 (UI 스레드 X)
    string processed = ProcessData(data);
    
    return processed;
}
```

**데드락의 메커니즘과 ConfigureAwait:**

레거시 ASP.NET이나 UI 애플리케이션에서 흔한 데드락 시나리오:

```csharp
// 💥 데드락 발생!
public string GetData()
{
    // 동기 메서드에서 비동기 메서드를 차단
    return GetDataAsync().Result;
}

private async Task<string> GetDataAsync()
{
    // SynchronizationContext 캡처됨
    var data = await DownloadAsync();
    
    // UI/ASP.NET 스레드로 돌아가려 하지만...
    // 호출자가 UI 스레드를 차단 중!
    return data;
}
```

데드락의 흐름:

```
1. UI 스레드: GetData() 호출
2. UI 스레드: GetDataAsync().Result로 차단 (UI 스레드 점유)
3. 스레드 풀: DownloadAsync() 실행
4. DownloadAsync() 완료
5. Continuation을 UI 스레드로 포스트하려 함
6. 💥 UI 스레드는 2단계에서 차단 중 - 데드락!
```

**해결 방법 1**: ConfigureAwait(false) 사용

```csharp
private async Task<string> GetDataAsync()
{
    // 컨텍스트 캡처 안함 - 스레드 풀에서 계속 실행
    var data = await DownloadAsync().ConfigureAwait(false);
    return data;  // 스레드 풀 스레드에서 반환
}
```

**해결 방법 2**: 끝까지 비동기 (권장)

```csharp
// ✅ 올바른 방법
public async Task<string> GetDataAsync()
{
    return await GetDataInternalAsync();
}
```

**성능 영향:**

ConfigureAwait(false)는 성능을 개선합니다:

1. **컨텍스트 전환 제거**: UI 스레드로의 마샬링 비용 제거
2. **스레드 풀 효율**: 워커 스레드에서 계속 실행
3. **메모리 할당 감소**: SynchronizationContext 캡처 비용 제거

벤치마크 예시:

```
| Method                   | Mean     | Allocated |
|------------------------- |---------:|----------:|
| WithoutConfigureAwait    | 1.234 ms | 1.2 KB    |
| WithConfigureAwait(false)| 0.987 ms | 0.8 KB    |
```

약 20-30%의 성능 향상과 메모리 할당 감소를 볼 수 있습니다.

### 사용 지침

**UI 애플리케이션 코드:**

UI 코드에서는 ConfigureAwait를 사용하지 않습니다:

```csharp
// ✅ UI 코드 - ConfigureAwait 사용 안 함
async void Button_Click(object sender, EventArgs e)
{
    // UI 스레드에서 시작
    var result = await DownloadDataAsync();
    
    // UI 스레드로 자동 복귀 - 안전
    label.Text = result;
    progressBar.Value = 100;
}
```

**라이브러리 코드:**

라이브러리에서는 항상 ConfigureAwait(false) 사용:

```csharp
// ✅ 라이브러리 코드 - 모든 await에 ConfigureAwait(false)
public async Task<Data> GetDataAsync()
{
    // 첫 번째 await
    var response = await httpClient.GetAsync(url)
        .ConfigureAwait(false);
    
    // 두 번째 await - 여전히 false 사용
    var content = await response.Content.ReadAsStringAsync()
        .ConfigureAwait(false);
    
    // 세 번째 await
    var data = await ParseDataAsync(content)
        .ConfigureAwait(false);
    
    return data;
}
```

왜 모든 await에 필요한가? 첫 번째 await만으로는 충분하지 않습니다. 각 await마다 컨텍스트가 캡처될 수 있으므로, 일관되게 모든 곳에 적용해야 합니다.

**라이브러리 작성 체크리스트:**

- ✅ 모든 `await`에 `.ConfigureAwait(false)` 추가
- ✅ UI 컨트롤이나 UI 관련 코드에 접근하지 않음
- ✅ `SynchronizationContext.Current`에 의존하지 않음
- ✅ `Thread.CurrentThread`에 의존하지 않음 (스레드 ID 등)
- ✅ 스레드 로컬 스토리지 사용 최소화

**예외: ConfigureAwait가 불필요한 경우**

다음 상황에서는 ConfigureAwait(false)가 효과가 없거나 불필요합니다:

```csharp
// 1. 마지막 await (반환 직전)
async Task<string> GetDataAsync()
{
    // 마지막이므로 ConfigureAwait 선택사항
    return await FetchAsync();
}

// 2. await를 사용하지 않고 Task 반환
Task<string> GetDataAsync()
{
    return FetchAsync();  // await 없음 - ConfigureAwait 불필요
}

// 3. 콘솔 애플리케이션 (SynchronizationContext 없음)
async Task Main()
{
    // ConfigureAwait는 효과 없지만 해도 무방
    await ProcessAsync().ConfigureAwait(false);
}
```

### ASP.NET Core의 특수성

ASP.NET Core는 레거시 ASP.NET과 달리 SynchronizationContext를 사용하지 않습니다. 이는 의도적인 설계 결정으로, 다음과 같은 이점이 있습니다:

```csharp
// ASP.NET Core에서는 SynchronizationContext가 null
public async Task<IActionResult> GetProducts()
{
    // ConfigureAwait(false)는 선택사항
    var products = await productService.GetProductsAsync()
        .ConfigureAwait(false);
    
    return Ok(products);
}
```

**ASP.NET Core가 SynchronizationContext를 제거한 이유:**

1. **성능**: 컨텍스트 전환 오버헤드 제거로 처리량 30-40% 향상
2. **단순성**: 개발자가 스레드에 대해 걱정할 필요 없음
3. **확장성**: 스레드 풀을 더 효율적으로 사용
4. **데드락 방지**: SynchronizationContext 기반 데드락 원천 차단

**ASP.NET Core에서의 ConfigureAwait 가이드:**

```csharp
// 옵션 1: ConfigureAwait(false) 사용 (일관성, 미래 대비)
public async Task<IActionResult> Option1()
{
    var data = await GetDataAsync().ConfigureAwait(false);
    return Ok(data);
}

// 옵션 2: ConfigureAwait 생략 (ASP.NET Core 전용)
public async Task<IActionResult> Option2()
{
    var data = await GetDataAsync();
    return Ok(data);
}
```

두 방식 모두 동작하지만, 다음을 고려하세요:

- **라이브러리**: 항상 `ConfigureAwait(false)` (다른 환경에서도 사용 가능)
- **ASP.NET Core 전용 코드**: 선택사항 (팀 컨벤션에 따름)
- **성능 크리티컬**: `ConfigureAwait(false)` (미세한 성능 향상)

**ExecutionContext 캡처:**

ConfigureAwait(false)는 SynchronizationContext만 무시하는 것이 아닙니다. ExecutionContext도 고려합니다:

```csharp
// ExecutionContext 캡처
AsyncLocal<string> _userId = new AsyncLocal<string>();

async Task ProcessWithContextAsync()
{
    _userId.Value = "user123";
    
    // ExecutionContext는 여전히 흐름
    await Task.Delay(100).ConfigureAwait(false);
    
    Console.WriteLine(_userId.Value);  // "user123" - 보존됨!
}
```

`ConfigureAwait(false)`는 다음을 건너뜁니다:
- SynchronizationContext 복원
- TaskScheduler 사용

하지만 다음은 **항상** 보존됩니다:
- ExecutionContext (보안, AsyncLocal 등)
- 예외 전파
- Task 상태

**커스텀 TaskScheduler:**

고급 시나리오에서는 커스텀 TaskScheduler를 사용할 수 있습니다:

```csharp
// 커스텀 스케줄러 정의
class LimitedConcurrencyTaskScheduler : TaskScheduler
{
    private readonly int _maxConcurrency;
    private readonly LinkedList<Task> _tasks = new LinkedList<Task>();
    private int _runningTasks = 0;
    
    protected override void QueueTask(Task task)
    {
        lock (_tasks)
        {
            _tasks.AddLast(task);
            if (_runningTasks < _maxConcurrency)
            {
                _runningTasks++;
                ThreadPool.QueueUserWorkItem(_ => ExecuteTask());
            }
        }
    }
    
    // ... 생략
}

// 사용
var scheduler = new LimitedConcurrencyTaskScheduler(maxConcurrency: 4);
await Task.Factory.StartNew(() => HeavyWorkAsync(), 
    CancellationToken.None,
    TaskCreationOptions.None,
    scheduler);
```

**핵심 규칙:**

1. **UI 코드**: ConfigureAwait 사용하지 않기
   - WPF, WinForms, MAUI, Xamarin
   - UI 컨트롤에 접근해야 함

2. **라이브러리 코드**: 항상 ConfigureAwait(false)
   - NuGet 패키지
   - 재사용 가능한 유틸리티
   - 다양한 환경에서 사용될 수 있음

3. **ASP.NET Core**: 선택사항
   - SynchronizationContext가 없음
   - 일관성을 위해 사용해도 좋음

4. **콘솔 앱**: 선택사항
   - SynchronizationContext가 없음
   - 일반적으로 불필요

5. **테스트 코드**: 일반적으로 불필요
   - 대부분의 테스트 프레임워크는 SynchronizationContext 없음
   - 하지만 일관성을 위해 사용해도 무방

**ConfigureAwait의 미래:**

C# 10+에서는 `[assembly: DisableRuntimeMarshalling]` 같은 속성이나 프로젝트 전체 설정으로 기본 동작을 제어하는 방안이 논의되고 있습니다. 하지만 현재로서는 명시적으로 ConfigureAwait를 사용하는 것이 권장됩니다.

---

## 18장 정리 및 심화 학습

이 장에서는 비동기 프로그래밍의 고급 패턴들을 체계적으로 학습했습니다. 단순한 async/await 사용을 넘어, 복잡한 비동기 워크플로우를 설계하고, 리소스를 효율적으로 관리하며, 성능을 최적화하는 실무 기술을 마스터했습니다.

### 주요 개념의 재조명

**1. Task 조율의 이론과 실전**

Task.WhenAll과 Task.WhenAny는 단순한 헬퍼 메서드가 아니라, **프로세스 대수(Process Algebra)**의 조합 연산자를 실현한 것입니다:

- **Task.WhenAll**: And-Join, Barrier Synchronization - 모든 작업의 완료를 보장하며, 분산 시스템의 **팬아웃/팬인(Fan-out/Fan-in)** 패턴을 구현합니다. 마이크로서비스 아키텍처에서 여러 서비스를 병렬로 호출하고 결과를 조합하는 데 필수적입니다.

- **Task.WhenAny**: Or-Join, Racing - 첫 번째 완료를 선택하며, **헷징(Hedging)**, **타임아웃**, **폴백** 패턴의 기반입니다. Google의 Jeff Dean이 제안한 헷징 패턴으로 P99 레이턴시를 극적으로 개선할 수 있습니다.

실무에서의 적용:

```
시나리오: 전자상거래 제품 상세 페이지
Task.WhenAll 사용:
- 제품 정보 (ProductService)
- 재고 확인 (InventoryService)
- 고객 리뷰 (ReviewService)
- 추천 상품 (RecommendationService)
→ 4개 서비스를 병렬 호출, 300ms → 80ms로 단축

시나리오: 외부 API 호출 (불안정)
Task.WhenAny 사용:
- 주 서버 (100ms, 때때로 느림)
- 백업 서버 (150ms, 안정적)
→ 헷징으로 P99: 2000ms → 200ms
```

**동시 실행 제한의 중요성:**

무제한 병렬 실행은 다음 문제를 야기합니다:
- **리소스 고갈**: 메모리, 파일 핸들, 네트워크 연결 소진
- **스레드 풀 고갈**: 모든 워커 스레드가 차단됨
- **외부 시스템 과부하**: API 속도 제한 초과, 데이터베이스 연결 풀 소진

해결: `SemaphoreSlim`으로 동시성 제어, 배치 처리, 적응형 스로틀링

**2. CancellationToken - 협력적 취소의 완성**

CancellationToken은 단순한 취소 메커니즘을 넘어, **트랜잭션 시맨틱스**를 비동기 작업에 적용한 것입니다:

- **안전성(Safety)**: 취소 시에도 일관된 상태 유지
- **활성성(Liveness)**: 취소 요청이 결국 처리됨
- **구성 가능성(Composability)**: 여러 취소 소스 조합 가능
- **전파성(Propagation)**: 호출 체인을 통한 자동 전파

비교: 강제 종료 vs 협력적 취소

| 특성 | Thread.Abort (레거시) | CancellationToken |
|------|----------------------|-------------------|
| 제어 | 강제 중단 | 협력적 확인 |
| 안전성 | 불안정 (손상된 상태) | 안전 (일관된 상태) |
| 리소스 정리 | 보장 안 됨 | finally 보장 |
| 예측 가능성 | 낮음 | 높음 |
| 사용 권장 | ❌ 절대 사용 금지 | ✅ 표준 패턴 |

**취소 설계 원칙:**

1. **조기 확인**: 작업 시작 시 즉시 확인하여 불필요한 작업 방지
2. **주기적 확인**: 긴 작업 내부에서 정기적으로 확인
3. **전파**: 모든 하위 호출에 토큰 전달
4. **정리**: finally 블록이나 using으로 리소스 정리
5. **알림**: 사용자에게 취소 완료 피드백

**3. IAsyncEnumerable - 스트리밍의 패러다임**

비동기 스트림은 **Reactive Programming**과 **Iterator Pattern**의 융합입니다:

수학적 쌍대성:
```
IEnumerable<T>       ←→ IObservable<T>
    (동기 풀)              (동기 푸시)
      ↓                      ↓
IAsyncEnumerable<T>  ←→ IAsyncObservable<T>
  (비동기 풀)            (비동기 푸시, 제안)
```

실무 적용 시나리오:

1. **페이지네이션 추상화**: REST API의 페이지네이션을 투명한 스트림으로 변환
2. **대용량 파일 처리**: 메모리에 전체 파일을 로드하지 않고 줄 단위 처리
3. **실시간 데이터 피드**: WebSocket, SignalR 메시지를 스트림으로 소비
4. **데이터베이스 스트리밍**: Entity Framework Core의 AsAsyncEnumerable()
5. **ETL 파이프라인**: 추출-변환-적재를 메모리 효율적으로 구현

**백프레셔(Backpressure) 자동 처리:**

비동기 스트림의 풀 기반 특성은 자연스럽게 백프레셔를 제공합니다:

```
생산자 (빠름): ████████████████
소비자 (느림): ████░░░░░░░░░░░░
              ↑
       소비자 속도에 맞춰 생산 자동 조절
```

이는 메모리 폭주를 방지하고, 시스템 안정성을 보장합니다.

**4. ConfigureAwait - 성능과 정확성의 균형**

ConfigureAwait는 **Continuation Scheduling**을 제어하는 저수준 메커니즘으로, 다음을 결정합니다:

- SynchronizationContext 복원 여부
- 원래 스레드로의 복귀 여부
- 성능 vs 편의성 트레이드오프

**프레임워크별 전략:**

```
┌─────────────────────────────────────────────┐
│ UI Framework (WPF, WinForms, MAUI)          │
│ → ConfigureAwait 사용 안 함                  │
│ → UI 스레드 복귀 필수                        │
│ → 정확성 우선                                │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ Library Code (NuGet, Reusable)              │
│ → ConfigureAwait(false) 필수                 │
│ → 환경 독립성                                │
│ → 성능 우선                                  │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ ASP.NET Core                                │
│ → ConfigureAwait(false) 선택사항             │
│ → SynchronizationContext 없음                │
│ → 일관성을 위해 사용 권장                     │
└─────────────────────────────────────────────┘
```

**성능 영향 실측:**

대규모 마이크로서비스 환경에서의 벤치마크:

```
시나리오: 10,000 동시 요청 처리

ConfigureAwait 없음:
- 평균 응답 시간: 245ms
- P99 레이턴시: 1200ms
- 메모리: 850MB
- CPU: 65%

ConfigureAwait(false) 사용:
- 평균 응답 시간: 198ms (19% 개선)
- P99 레이턴시: 980ms (18% 개선)
- 메모리: 720MB (15% 감소)
- CPU: 58% (11% 감소)
```

### 고급 패턴의 통합 활용

실무에서는 이러한 패턴들을 조합하여 사용합니다:

**패턴 1: Resilient 마이크로서비스 호출**

```csharp
async Task<Result> CallServiceWithResilienceAsync(CancellationToken ct)
{
    // 재시도 + 타임아웃 + 폴백
    for (int attempt = 0; attempt < 3; attempt++)
    {
        try
        {
            using var cts = CancellationTokenSource.CreateLinkedTokenSource(ct);
            cts.CancelAfter(TimeSpan.FromSeconds(5));
            
            return await CallPrimaryServiceAsync(cts.Token)
                .ConfigureAwait(false);
        }
        catch (Exception ex) when (IsTransient(ex) && attempt < 2)
        {
            await Task.Delay(TimeSpan.FromSeconds(Math.Pow(2, attempt)), ct)
                .ConfigureAwait(false);
        }
    }
    
    // 폴백
    return await CallFallbackServiceAsync(ct).ConfigureAwait(false);
}
```

**패턴 2: 대용량 데이터 ETL**

```csharp
async Task ProcessLargeDatasetAsync(string source, string destination, 
    CancellationToken ct)
{
    var semaphore = new SemaphoreSlim(10); // 최대 10개 동시 처리
    
    await foreach (var batch in FetchBatchesAsync(source, ct))
    {
        await semaphore.WaitAsync(ct);
        
        _ = Task.Run(async () =>
        {
            try
            {
                await ProcessBatchAsync(batch, ct).ConfigureAwait(false);
            }
            finally
            {
                semaphore.Release();
            }
        });
    }
}
```

### 실무 적용 체크리스트

**API 설계:**
- ✅ 모든 I/O 바운드 API는 비동기
- ✅ CancellationToken 매개변수 제공
- ✅ Progress<T>로 진행률 보고 (선택)
- ✅ Task<T> 반환 (async void 금지)
- ✅ Async 접미사 사용

**성능 최적화:**
- ✅ 라이브러리: ConfigureAwait(false)
- ✅ 동시 실행 제한 (SemaphoreSlim)
- ✅ ValueTask 활용 (핫 패스)
- ✅ 객체 풀링 (대량 Task)
- ✅ 비동기 스트림 (대용량 데이터)

**안정성:**
- ✅ 타임아웃 설정
- ✅ 재시도 + 지수 백오프
- ✅ 회로 차단기 (Circuit Breaker)
- ✅ 예외 처리 + 로깅
- ✅ 리소스 정리 (IAsyncDisposable)

### 다음 단계와 추가 학습

**고급 주제:**

1. **Channel<T>**: 생산자-소비자 패턴의 고성능 구현
2. **IAsyncDisposable**: 비동기 리소스 정리
3. **ValueTask 최적화**: 제로 할당 비동기
4. **Custom TaskScheduler**: 스케줄링 제어
5. **Dataflow (TPL Dataflow)**: 복잡한 파이프라인 구축

**권장 리소스:**

- **"Concurrency in C# Cookbook"** by Stephen Cleary - 실전 패턴의 바이블
- **"C# in Depth"** by Jon Skeet - 비동기의 내부 구조
- **Stephen Cleary's Blog**: https://blog.stephencleary.com/
- **Microsoft Docs**: Asynchronous Programming Patterns
- **"Designing Data-Intensive Applications"** by Martin Kleppmann - 분산 시스템 이론

**실전 프로젝트 아이디어:**

1. **RSS 리더**: 여러 피드를 병렬로 가져오고, 취소와 재시도 구현
2. **파일 동기화 도구**: 비동기 스트림으로 대용량 파일 처리
3. **채팅 서버**: WebSocket과 Channel<T>로 실시간 메시징
4. **배치 프로세서**: 수천 개 작업을 동시성 제한하며 처리
5. **API 게이트웨이**: 마이크로서비스 조율과 헷징 구현

### 마치며

고급 비동기 패턴을 마스터하는 것은 현대 C# 개발자의 필수 역량입니다. 클라우드 네이티브, 마이크로서비스, 실시간 시스템에서 이러한 패턴들은 더 이상 선택이 아닌 필수입니다. 이 장에서 배운 내용을 바탕으로, 확장 가능하고 응답성 있으며 안정적인 애플리케이션을 설계하고 구현할 수 있게 되었습니다.

비동기 프로그래밍은 단순히 코드를 빠르게 만드는 것이 아니라, **시스템의 사고방식(Systems Thinking)**을 요구합니다. 리소스, 동시성, 실패, 백프레셔, 취소 등 여러 측면을 동시에 고려하며, 전체 시스템의 동작을 예측하고 제어하는 능력을 기르는 것입니다.

계속해서 실험하고, 측정하고, 최적화하며, 여러분만의 비동기 워크플로우 패턴을 만들어 나가시기 바랍니다. 행운을 빕니다!
