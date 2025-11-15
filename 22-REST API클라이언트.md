# 22장. REST API 클라이언트

REST API 클라이언트는 현대 분산 시스템 아키텍처의 핵심 구성 요소로, 마이크로서비스, 클라우드 네이티브 애플리케이션, 그리고 모바일 앱 개발의 필수 기술입니다. HTTP 프로토콜을 기반으로 하는 REST(Representational State Transfer) 아키텍처 스타일은 2000년 Roy Fielding의 박사 논문 "Architectural Styles and the Design of Network-based Software Architectures"에서 처음 제시되었으며, 이후 웹 서비스 개발의 사실상 표준(de facto standard)이 되었습니다.

**REST API의 역사적 발전:**

웹 서비스의 역사는 분산 컴퓨팅 패러다임의 진화를 보여줍니다. 1990년대 후반, CORBA(Common Object Request Broker Architecture)와 DCOM(Distributed Component Object Model)은 복잡한 원격 프로시저 호출(RPC) 메커니즘을 제공했지만, 플랫폼 종속성과 복잡성으로 인해 널리 채택되지 못했습니다. 2000년대 초반, SOAP(Simple Object Access Protocol)과 WSDL(Web Services Description Language)이 등장했지만, XML의 무거움과 복잡한 스펙이 문제였습니다.

Roy Fielding의 REST는 이러한 복잡성에 대한 반발로 탄생했습니다. REST는 **단순성(Simplicity)**, **확장성(Scalability)**, **무상태성(Statelessness)**, **캐시 가능성(Cacheability)**을 핵심 원칙으로 제시하며, HTTP 프로토콜의 본래 의도를 충실히 따르는 아키텍처 스타일을 제안했습니다. 2005년 Ruby on Rails의 등장과 함께 REST가 대중화되었고, 2010년대 모바일 혁명과 클라우드 컴퓨팅의 부상으로 REST API는 사실상의 표준이 되었습니다.

**REST의 아키텍처 제약 조건 (Architectural Constraints):**

REST는 다음 6가지 제약 조건을 통해 분산 시스템의 품질을 향상시킵니다:

1. **클라이언트-서버 아키텍처 (Client-Server)**: 관심사의 분리(Separation of Concerns)를 통해 UI와 데이터 저장소를 독립적으로 진화시킬 수 있습니다. 이는 Conway의 법칙과 일치하며, 조직 구조의 유연성을 제공합니다.

2. **무상태성 (Statelessness)**: 각 요청은 독립적이며, 서버는 클라이언트의 상태를 저장하지 않습니다. 이는 **확장성(Scalability)**을 극대화하며, 로드 밸런싱과 장애 복구를 간단하게 만듭니다. HTTP는 본질적으로 무상태 프로토콜이며, REST는 이를 충실히 따릅니다.

3. **캐시 가능성 (Cacheability)**: 응답은 명시적으로 캐시 가능 여부를 표시해야 합니다. HTTP 헤더(`Cache-Control`, `ETag`, `Last-Modified`)를 통해 구현되며, CDN(Content Delivery Network)과 프록시 캐싱을 가능하게 합니다. 이는 네트워크 효율성을 대폭 향상시킵니다.

4. **계층화된 시스템 (Layered System)**: 클라이언트는 중간 계층(프록시, 게이트웨이, 방화벽)의 존재를 알 수 없습니다. 이는 **보안(Security)**, **로드 밸런싱**, **레거시 시스템 캡슐화**를 가능하게 하며, 마이크로서비스 아키텍처의 핵심 원칙입니다.

5. **통일된 인터페이스 (Uniform Interface)**: 리소스 식별(URI), 표현을 통한 리소스 조작(HTTP 메서드), 자기 서술적 메시지(MIME 타입), HATEOAS(Hypermedia As The Engine Of Application State)를 통해 시스템 전체의 일관성을 제공합니다.

6. **코드 온 디맨드 (Code-On-Demand)** (선택적): 서버가 클라이언트에 실행 가능한 코드(JavaScript)를 전송할 수 있습니다. 이는 단일 페이지 애플리케이션(SPA)의 기반이 되었습니다.

**C#에서의 HTTP 클라이언트 진화:**

C#의 HTTP 클라이언트 라이브러리는 .NET Framework의 발전과 함께 진화했습니다:

- **.NET Framework 1.0 (2002)**: `WebRequest`와 `WebResponse` - 저수준 API, 복잡한 사용법
- **.NET Framework 2.0 (2005)**: `WebClient` - 간단한 API, 하지만 동기 메서드 중심
- **.NET Framework 4.5 (2012)**: `HttpClient` - 비동기 우선, 재사용 가능, Task 기반
- **.NET Core 2.1 (2018)**: `HttpClientFactory` - 소켓 고갈 문제 해결, DI 통합
- **.NET 5+ (2020~)**: HTTP/2, HTTP/3(QUIC) 지원, 성능 최적화

`HttpClient`는 .NET 4.5에서 도입된 현대적인 HTTP 클라이언트로, 다음과 같은 설계 철학을 따릅니다:

- **비동기 우선(Async-first)**: 모든 메서드가 비동기로 설계되어 I/O 바운드 작업의 확장성을 극대화합니다
- **재사용 가능(Reusable)**: 단일 인스턴스를 애플리케이션 생명주기 동안 재사용하도록 설계되었습니다
- **확장 가능(Extensible)**: `HttpMessageHandler` 파이프라인을 통해 로깅, 인증, 재시도 등을 추가할 수 있습니다
- **표준 준수(Standards-compliant)**: HTTP/1.1, HTTP/2, HTTP/3를 완전히 지원합니다

**REST API와 마이크로서비스의 시너지:**

마이크로서비스 아키텍처는 REST API의 가장 성공적인 적용 사례입니다. Martin Fowler와 James Lewis가 2014년 정의한 마이크로서비스는 다음과 같은 특징을 가집니다:

- **서비스 경계(Service Boundaries)**: 각 마이크로서비스는 독립적으로 배포 가능한 단위이며, REST API를 통해 통신합니다
- **분산 데이터 관리(Decentralized Data Management)**: 각 서비스가 자체 데이터베이스를 소유하며, API를 통해서만 접근합니다
- **장애 격리(Failure Isolation)**: Circuit Breaker 패턴을 통해 연쇄 장애를 방지합니다
- **독립적 확장(Independent Scaling)**: 부하가 높은 서비스만 선택적으로 확장할 수 있습니다

Netflix, Amazon, Uber 같은 대규모 서비스들은 수천 개의 마이크로서비스를 REST API로 연결하여 운영하고 있으며, 이는 현대 클라우드 네이티브 애플리케이션의 표준 패턴이 되었습니다.

## 이 장에서 배울 내용

이 장을 통해 독자 여러분은 REST API 클라이언트 개발의 이론적 기반과 실무 기술을 체계적으로 학습하게 됩니다:

- **HttpClient의 내부 구조와 설계 철학**: .NET의 HTTP 스택, 소켓 풀 관리, 연결 재사용, HTTP/2 멀티플렉싱의 원리를 이해하고, HttpClientFactory가 해결하는 소켓 고갈(Socket Exhaustion) 문제와 DNS 캐싱 이슈를 학습합니다.

- **HTTP 프로토콜의 본질**: HTTP 메서드(GET, POST, PUT, DELETE, PATCH)의 의미론(Semantics), 멱등성(Idempotency)과 안전성(Safety)의 개념, 상태 코드(Status Code)의 체계적 분류(1xx, 2xx, 3xx, 4xx, 5xx), 그리고 헤더(Headers)의 역할을 깊이 이해합니다.

- **JSON의 역사와 직렬화 메커니즘**: Douglas Crockford가 2001년 제안한 JSON(JavaScript Object Notation)의 등장 배경, XML 대비 장점, `System.Text.Json`의 내부 동작 원리(Utf8JsonReader의 zero-allocation 파싱), 그리고 성능 최적화 기법을 학습합니다.

- **비동기 HTTP 통신의 복잡성**: 네트워크 지연(Network Latency), 재시도(Retry) 전략, 백오프(Backoff) 알고리즘, Circuit Breaker 패턴, 타임아웃 관리, 그리고 Polly 라이브러리를 통한 복원력(Resilience) 구현 방법을 배웁니다.

- **실전 REST API 설계 패턴**: Richardson Maturity Model의 4가지 레벨, HATEOAS의 실용성, API 버저닝 전략(URI, Header, Media Type), 페이지네이션(Pagination), 필터링(Filtering), 정렬(Sorting), 그리고 rate limiting 처리 방법을 익힙니다.

**학습 목표:**

- REST 아키텍처 스타일의 제약 조건과 설계 원칙 이해
- HttpClient의 올바른 사용법과 생명주기 관리
- HTTP 프로토콜의 의미론적 이해와 실무 적용
- JSON 직렬화/역직렬화의 성능 최적화
- 분산 시스템의 장애 패턴과 복원력 전략
- 실제 공개 API와의 통합 경험

---

## 22.1 HttpClient 소개

`HttpClient`는 .NET 4.5에서 도입된 현대적인 HTTP 클라이언트 라이브러리로, 비동기 프로그래밍과 성능 최적화를 핵심 설계 목표로 합니다. System.Net.Http 네임스페이스에 위치하며, `WebClient`와 `WebRequest`의 후속 기술로 개발되었습니다. `HttpClient`는 단순한 HTTP 요청 라이브러리를 넘어, 현대적인 웹 통신의 복잡한 요구사항을 해결하는 포괄적인 솔루션입니다.

**HttpClient의 아키텍처와 내부 구조:**

`HttpClient`는 계층화된 아키�ecture를 따르며, 다음과 같은 구성 요소로 이루어져 있습니다:

```
HttpClient (고수준 API)
    ↓
HttpMessageHandler 파이프라인 (확장 가능한 미들웨어)
    ↓
HttpClientHandler (기본 핸들러)
    ↓
SocketsHttpHandler (.NET Core 2.1+의 기본 구현)
    ↓
Socket 풀 (연결 재사용과 관리)
    ↓
OS 네트워크 스택 (TCP/IP, TLS)
```

**SocketsHttpHandler**는 .NET Core 2.1에서 도입된 새로운 구현으로, 이전의 `HttpClientHandler`를 대체합니다. 주요 개선사항은:

- **완전한 관리 코드(Managed Code)**: 플랫폼 네이티브 API에 의존하지 않아 크로스 플랫폼 일관성이 보장됩니다
- **HTTP/2 지원**: 단일 TCP 연결에서 멀티플렉싱(Multiplexing)을 통해 여러 요청을 동시에 처리합니다
- **연결 풀 관리**: 효율적인 소켓 재사용으로 성능을 극대화합니다
- **커스텀 가능성**: 세밀한 설정과 확장이 가능합니다

**HTTP 연결의 생명주기:**

HTTP 통신의 이해를 위해서는 TCP/IP 연결의 생명주기를 알아야 합니다:

```
1. DNS 조회 (DNS Lookup)
   - 도메인 이름 → IP 주소 변환
   - 평균 20-120ms 소요

2. TCP 3-way 핸드셰이크
   - SYN → SYN-ACK → ACK
   - 네트워크 지연(RTT) × 1.5 소요

3. TLS 핸드셰이크 (HTTPS의 경우)
   - 인증서 교환, 암호화 협상
   - RTT × 2-3 소요

4. HTTP 요청/응답
   - 실제 데이터 전송
   - 요청 크기 + 응답 크기에 비례

5. 연결 종료 (Connection Close) 또는 재사용 (Keep-Alive)
   - HTTP/1.1의 기본값은 Keep-Alive
   - 연결 재사용으로 1-3단계 생략 가능
```

이러한 오버헤드 때문에, **연결 재사용(Connection Reuse)**은 성능의 핵심입니다. `HttpClient`를 재사용하면 내부 연결 풀을 통해 이러한 비용을 대폭 절감할 수 있습니다.

**소켓 고갈 문제 (Socket Exhaustion):**

`HttpClient`를 요청마다 생성하고 폐기하는 것은 심각한 성능 문제를 야기합니다:

```csharp
// ❌ 극도로 나쁜 예 - 소켓 고갈 유발
for (int i = 0; i < 1000; i++)
{
    using var client = new HttpClient();
    await client.GetStringAsync("https://api.example.com/data");
}
// 문제: TIME_WAIT 상태의 소켓이 수천 개 생성됨
```

TCP 소켓은 종료 후에도 **TIME_WAIT** 상태로 일정 시간(Windows에서 기본 240초) 대기합니다. 이는 지연된 패킷이나 재전송 패킷을 올바르게 처리하기 위한 TCP의 필수 메커니즘입니다. 따라서 `HttpClient`를 반복적으로 생성하면:

1. **포트 고갈(Port Exhaustion)**: 사용 가능한 임시 포트(Ephemeral Port) 고갈
2. **메모리 누수**: TIME_WAIT 상태의 소켓이 메모리 점유
3. **성능 저하**: 새 연결 설정의 반복적인 오버헤드
4. **서버 부하**: 불필요한 연결 생성으로 서버 리소스 소모

**DNS 변경 감지 문제:**

`HttpClient`를 영구적으로 재사용할 때의 또 다른 문제는 **DNS 캐싱**입니다:

```csharp
// ⚠️ 주의: DNS 변경을 감지하지 못함
private static readonly HttpClient client = new HttpClient();

// 문제: api.example.com의 IP가 변경되어도 
// 기존 연결을 계속 사용하려고 시도
```

`HttpClient`는 처음 DNS를 조회한 후 연결을 재사용하므로, 로드 밸런서나 장애 조치(Failover)로 인한 DNS 변경을 감지하지 못할 수 있습니다. 이는 클라우드 환경에서 특히 문제가 됩니다.

**HttpClientFactory의 등장 (.NET Core 2.1+):**

이러한 문제들을 해결하기 위해 Microsoft는 `HttpClientFactory`를 도입했습니다:

```csharp
// ✅ 최선의 방법 - HttpClientFactory 사용
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHttpClient("MyApiClient", client =>
        {
            client.BaseAddress = new Uri("https://api.example.com/");
            client.DefaultRequestHeaders.Add("User-Agent", "MyApp/1.0");
            client.Timeout = TimeSpan.FromSeconds(30);
        });
    }
}

public class MyService
{
    private readonly IHttpClientFactory _httpClientFactory;
    
    public MyService(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }
    
    public async Task<string> GetDataAsync()
    {
        var client = _httpClientFactory.CreateClient("MyApiClient");
        return await client.GetStringAsync("/data");
    }
}
```

`HttpClientFactory`의 이점:

1. **연결 풀 관리**: 내부적으로 `HttpMessageHandler`를 풀링하여 소켓 재사용
2. **Handler 교체**: 기본 2분마다 handler를 교체하여 DNS 변경 감지
3. **로깅 통합**: 요청/응답 자동 로깅 (ILogger 통합)
4. **정책 기반 설정**: Polly와 통합하여 재시도, Circuit Breaker 등 적용
5. **의존성 주입**: ASP.NET Core DI 컨테이너와 자연스러운 통합

### HttpClient 기본 사용법

간단한 콘솔 애플리케이션이나 학습 목적으로는 `HttpClient`를 직접 사용할 수 있습니다:

```csharp
using System.Net.Http;

// HttpClient 인스턴스 생성 (애플리케이션 수명 동안 재사용)
using HttpClient client = new HttpClient();

// BaseAddress 설정 (선택사항)
// 모든 요청의 기준 URL이 됩니다
client.BaseAddress = new Uri("https://api.example.com/");

// 기본 헤더 설정
// 모든 요청에 자동으로 포함됩니다
client.DefaultRequestHeaders.Add("User-Agent", "MyApp/1.0");
client.DefaultRequestHeaders.Add("Accept", "application/json");

// 타임아웃 설정 (기본값: 100초)
client.Timeout = TimeSpan.FromSeconds(30);
```

**HTTP 헤더의 의미와 중요성:**

HTTP 헤더는 요청과 응답의 메타데이터를 전달하는 핵심 메커니즘입니다:

- **User-Agent**: 클라이언트 식별자. 많은 API가 이를 요구하며, 누락 시 403 Forbidden을 반환할 수 있습니다
- **Accept**: 클라이언트가 이해할 수 있는 미디어 타입(MIME type). `application/json`, `text/html` 등
- **Authorization**: 인증 토큰 (Bearer, Basic 등). OAuth 2.0, JWT 등에서 사용
- **Content-Type**: 요청 본문의 미디어 타입. POST/PUT 요청에서 필수
- **Cache-Control**: 캐싱 정책 지시. `no-cache`, `max-age=3600` 등

### HttpClient 사용 시 주의사항과 모범 사례

**안티패턴 1: 요청마다 새로운 HttpClient 생성**

```csharp
// ❌ 최악의 안티패턴
async Task BadExampleAsync()
{
    using var client = new HttpClient();
    await client.GetStringAsync("https://api.example.com/data");
}
// 문제점:
// 1. 소켓 고갈 (TIME_WAIT 상태 누적)
// 2. DNS 조회 반복 (20-120ms 오버헤드)
// 3. TLS 핸드셰이크 반복 (RTT × 2-3)
// 4. 메모리 낭비
// 5. GC 압력 증가
```

**모범 사례 1: 싱글톤 패턴**

```csharp
// ✅ 좋은 예 - 애플리케이션 수준 싱글톤
class ApiService
{
    // static readonly로 스레드 안전하게 공유
    private static readonly HttpClient client = new HttpClient()
    {
        BaseAddress = new Uri("https://api.example.com/"),
        Timeout = TimeSpan.FromSeconds(30)
    };
    
    // static constructor로 헤더 초기화
    static ApiService()
    {
        client.DefaultRequestHeaders.Add("User-Agent", "MyApp/1.0");
        client.DefaultRequestHeaders.Add("Accept", "application/json");
    }
    
    public async Task<string> GetDataAsync()
    {
        return await client.GetStringAsync("/data");
    }
}
```

**모범 사례 2: HttpClientFactory (권장)**

```csharp
// ✅ 최선 - .NET Core 2.1+ ASP.NET Core 환경
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Named Client 방식
        services.AddHttpClient("GitHubApi", client =>
        {
            client.BaseAddress = new Uri("https://api.github.com/");
            client.DefaultRequestHeaders.Add("User-Agent", "MyApp");
            client.DefaultRequestHeaders.Add("Accept", "application/vnd.github.v3+json");
        })
        .SetHandlerLifetime(TimeSpan.FromMinutes(5))  // Handler 교체 주기
        .AddPolicyHandler(GetRetryPolicy());  // Polly 정책
    }
    
    static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
    {
        return HttpPolicyExtensions
            .HandleTransientHttpError()
            .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
            .WaitAndRetryAsync(3, retryAttempt => 
                TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));
    }
}

// Typed Client 방식 (더 강력한 타입 안전성)
public class GitHubService
{
    private readonly HttpClient _httpClient;
    
    public GitHubService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }
    
    public async Task<GitHubUser> GetUserAsync(string username)
    {
        var response = await _httpClient.GetStringAsync($"/users/{username}");
        return JsonSerializer.Deserialize<GitHubUser>(response);
    }
}

// Startup.cs에서 등록
services.AddHttpClient<GitHubService>(client =>
{
    client.BaseAddress = new Uri("https://api.github.com/");
});
```

**안티패턴 2: using 문으로 HttpClient 폐기**

```csharp
// ❌ 나쁜 예 - using은 소켓을 즉시 해제하지 않음
async Task AnotherBadExampleAsync()
{
    using (HttpClient client = new HttpClient())
    {
        await client.GetStringAsync("https://api.example.com/data");
    } // Dispose()가 호출되지만, 소켓은 TIME_WAIT 상태로 남음
}
```

`IDisposable.Dispose()`를 호출해도 내부 소켓은 즉시 해제되지 않습니다. TCP 프로토콜의 TIME_WAIT 상태는 운영체제 수준에서 강제되므로, `using` 문이 문제를 해결하지 못합니다.

**모범 사례 3: 비동기 메서드의 일관된 사용**

```csharp
// ✅ 모든 HTTP 작업을 비동기로 처리
async Task<string> GetDataAsync()
{
    // GetStringAsync, GetAsync, PostAsync 등은 모두 비동기
    return await client.GetStringAsync("https://api.example.com/data");
}

// ❌ 동기 차단은 데드락과 성능 저하 유발
string GetDataSync()
{
    // .Result나 .Wait()는 ASP.NET에서 데드락 위험
    return client.GetStringAsync("https://api.example.com/data").Result;
}
```

**HttpClient의 스레드 안전성:**

`HttpClient`는 **스레드 안전(Thread-safe)**하게 설계되었습니다. 여러 스레드에서 동시에 `HttpClient` 인스턴스를 사용해도 안전합니다:

```csharp
private static readonly HttpClient client = new HttpClient();

// 여러 스레드에서 동시 호출 가능
var tasks = new[]
{
    client.GetStringAsync("https://api.example.com/data1"),
    client.GetStringAsync("https://api.example.com/data2"),
    client.GetStringAsync("https://api.example.com/data3")
};

string[] results = await Task.WhenAll(tasks);
```

단, `HttpClient`의 **속성 설정(Properties)**은 스레드 안전하지 않으므로, 초기화 이후에는 속성을 변경하지 않아야 합니다:

```csharp
// ⚠️ 주의: 속성 변경은 초기화 시에만
private static readonly HttpClient client = new HttpClient()
{
    Timeout = TimeSpan.FromSeconds(30)  // ✅ 생성 시 설정
};

// ❌ 런타임에 속성 변경은 위험
client.Timeout = TimeSpan.FromSeconds(60);  // 경쟁 상태(Race condition) 발생 가능
```

---

## 22.2 GET 요청으로 데이터 받기

GET 메서드는 HTTP의 가장 기본적인 메서드로, **리소스를 조회(Retrieve)**하는 용도로 사용됩니다. REST 아키텍처에서 GET은 **안전한(Safe)** 작업이며 **멱등성(Idempotent)**을 가집니다. 이는 동일한 GET 요청을 여러 번 수행해도 서버의 상태가 변하지 않으며, 결과도 동일하다는 의미입니다.

**HTTP 메서드의 의미론적 특성:**

| 메서드 | 안전(Safe) | 멱등(Idempotent) | 캐시 가능(Cacheable) | 본문(Body) |
|--------|-----------|------------------|---------------------|-----------|
| GET    | ✅ Yes    | ✅ Yes           | ✅ Yes              | ❌ No     |
| POST   | ❌ No     | ❌ No            | ⚠️ 조건부            | ✅ Yes    |
| PUT    | ❌ No     | ✅ Yes           | ❌ No               | ✅ Yes    |
| DELETE | ❌ No     | ✅ Yes           | ❌ No               | ❌ No     |
| PATCH  | ❌ No     | ❌ No            | ❌ No               | ✅ Yes    |

- **안전(Safe)**: 서버의 상태를 변경하지 않습니다. 읽기 전용 작업입니다.
- **멱등(Idempotent)**: 동일한 요청을 여러 번 수행해도 결과가 같습니다. F(F(x)) = F(x)
- **캐시 가능(Cacheable)**: 응답을 중간 프록시나 브라우저에 캐싱할 수 있습니다.

GET의 멱등성은 네트워크 장애 시 **자동 재시도(Automatic Retry)**를 가능하게 하는 핵심 속성입니다. 브라우저나 프록시는 GET 요청이 실패하면 안전하게 재시도할 수 있습니다.

**GET 요청의 URL 구조:**

```
https://api.example.com/users/123?format=json&fields=name,email#section
└─┬─┘   └────┬────────┘ └──┬─┘└─┬┘└────────┬──────────┘└───────┬──────┘
 프로토콜      호스트       경로  리소스      쿼리 파라미터          프래그먼트
              (Host)      (Path) (ID)    (Query String)        (Fragment)
```

- **프로토콜(Scheme)**: `http` 또는 `https` (TLS 암호화)
- **호스트(Host)**: 도메인 이름 또는 IP 주소
- **경로(Path)**: 리소스의 계층적 위치
- **쿼리 파라미터(Query String)**: 필터링, 정렬, 페이지네이션 등에 사용
- **프래그먼트(Fragment)**: 클라이언트 측 내비게이션 (서버로 전송되지 않음)

### GetStringAsync - 가장 간단한 GET 요청

`GetStringAsync`는 응답 본문을 문자열로 직접 반환하는 편리한 메서드입니다:

```csharp
async Task<string> GetDataAsStringAsync(string url)
{
    using HttpClient client = new HttpClient();
    
    try
    {
        // 응답을 문자열로 자동 변환
        string response = await client.GetStringAsync(url);
        Console.WriteLine("응답 데이터:");
        Console.WriteLine(response);
        return response;
    }
    catch (HttpRequestException e)
    {
        // HTTP 수준 오류 (네트워크, DNS, 4xx/5xx 상태 코드)
        Console.WriteLine($"요청 실패: {e.Message}");
        
        // StatusCode 확인 (존재하는 경우)
        if (e.StatusCode.HasValue)
        {
            Console.WriteLine($"상태 코드: {e.StatusCode}");
        }
        
        return string.Empty;
    }
    catch (TaskCanceledException e)
    {
        // 타임아웃 발생 (Timeout 속성 초과)
        Console.WriteLine($"요청 시간 초과: {e.Message}");
        return string.Empty;
    }
}

// 사용 예
string data = await GetDataAsStringAsync("https://api.github.com/users/octocat");
```

**GetStringAsync의 내부 동작:**

```csharp
// GetStringAsync의 내부 구현 (간소화)
public async Task<string> GetStringAsync(string requestUri)
{
    HttpResponseMessage response = await GetAsync(requestUri);
    
    // 성공 상태 코드 확인 (2xx)
    response.EnsureSuccessStatusCode();
    
    // Content를 문자열로 읽기
    return await response.Content.ReadAsStringAsync();
}
```

`GetStringAsync`의 특징:
- ✅ **간결함**: 단 한 줄로 데이터 조회
- ✅ **자동 오류 처리**: 비성공 상태 코드 시 `HttpRequestException` 발생
- ⚠️ **세밀한 제어 불가**: 상태 코드나 헤더에 직접 접근 불가
- ⚠️ **큰 응답 주의**: 전체 응답을 메모리에 로드 (스트리밍 불가)

### GetAsync - 완전한 제어와 유연성

`GetAsync`는 `HttpResponseMessage` 객체를 반환하여 응답의 모든 측면에 접근할 수 있게 합니다:

```csharp
async Task GetDataWithDetailsAsync(string url)
{
    using HttpClient client = new HttpClient();
    
    // User-Agent 헤더는 많은 API에서 필수
    client.DefaultRequestHeaders.Add("User-Agent", "C# App");
    
    try
    {
        // 전체 응답 객체 받기
        HttpResponseMessage response = await client.GetAsync(url);
        
        // 상태 코드와 성공 여부 확인
        Console.WriteLine($"상태 코드: {(int)response.StatusCode} {response.StatusCode}");
        Console.WriteLine($"성공 여부: {response.IsSuccessStatusCode}");
        Console.WriteLine($"프로토콜 버전: {response.Version}");
        
        // 응답 헤더 출력
        Console.WriteLine("\n응답 헤더:");
        foreach (var header in response.Headers)
        {
            Console.WriteLine($"  {header.Key}: {string.Join(", ", header.Value)}");
        }
        
        // Content 헤더 (Content-Type, Content-Length 등)
        Console.WriteLine("\nContent 헤더:");
        foreach (var header in response.Content.Headers)
        {
            Console.WriteLine($"  {header.Key}: {string.Join(", ", header.Value)}");
        }
        
        // 응답 본문 읽기
        if (response.IsSuccessStatusCode)
        {
            string content = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"\n응답 내용 ({content.Length} 문자):");
            
            // 큰 응답의 경우 일부만 출력
            int preview = Math.Min(content.Length, 500);
            Console.WriteLine(content.Substring(0, preview));
            if (content.Length > preview)
            {
                Console.WriteLine("...(생략)...");
            }
        }
        else
        {
            // 오류 상태 코드 처리
            string errorContent = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"요청 실패: {errorContent}");
        }
    }
    catch (Exception e)
    {
        Console.WriteLine($"오류 발생: {e.GetType().Name} - {e.Message}");
    }
}
```

**HTTP 상태 코드의 분류와 의미:**

HTTP 상태 코드는 3자리 숫자로, 첫 번째 자리가 응답의 카테고리를 나타냅니다:

**1xx - 정보성 응답(Informational):**
- `100 Continue`: 클라이언트가 요청을 계속해도 됨
- `101 Switching Protocols`: WebSocket으로 프로토콜 전환

**2xx - 성공(Success):**
- `200 OK`: 요청 성공, 응답 본문에 데이터 포함
- `201 Created`: 리소스가 성공적으로 생성됨 (POST 요청 후)
- `204 No Content`: 성공했으나 응답 본문 없음 (DELETE 후 흔함)
- `206 Partial Content`: 범위 요청(Range Request)의 일부만 반환

**3xx - 리다이렉션(Redirection):**
- `301 Moved Permanently`: 리소스가 영구적으로 이동 (SEO에 중요)
- `302 Found`: 임시 리다이렉션
- `304 Not Modified`: 캐시된 버전을 사용하라 (조건부 GET)
- `307 Temporary Redirect`: 302와 유사하지만 메서드 변경 금지
- `308 Permanent Redirect`: 301과 유사하지만 메서드 변경 금지

**4xx - 클라이언트 오류(Client Error):**
- `400 Bad Request`: 잘못된 요청 구문
- `401 Unauthorized`: 인증 필요 (사실은 "Unauthenticated"가 정확함)
- `403 Forbidden`: 권한 없음 (인증되었지만 접근 거부)
- `404 Not Found`: 리소스를 찾을 수 없음
- `405 Method Not Allowed`: 허용되지 않는 HTTP 메서드
- `409 Conflict`: 리소스 상태 충돌 (동시성 문제)
- `429 Too Many Requests`: Rate limit 초과

**5xx - 서버 오류(Server Error):**
- `500 Internal Server Error`: 서버 내부 오류
- `502 Bad Gateway`: 게이트웨이/프록시 오류
- `503 Service Unavailable`: 서버 과부하 또는 유지보수
- `504 Gateway Timeout`: 게이트웨이 타임아웃

```csharp
// 상태 코드 기반 처리 예제
async Task<string> HandleStatusCodesAsync(string url)
{
    using var client = new HttpClient();
    var response = await client.GetAsync(url);
    
    return response.StatusCode switch
    {
        HttpStatusCode.OK => await response.Content.ReadAsStringAsync(),
        
        HttpStatusCode.NotFound => 
            throw new InvalidOperationException("리소스를 찾을 수 없습니다"),
        
        HttpStatusCode.Unauthorized => 
            throw new UnauthorizedAccessException("인증이 필요합니다"),
        
        HttpStatusCode.TooManyRequests =>
            throw new InvalidOperationException("요청 한도 초과, 잠시 후 다시 시도하세요"),
        
        HttpStatusCode status when (int)status >= 500 =>
            throw new Exception($"서버 오류: {status}"),
        
        _ => throw new Exception($"예상치 못한 상태 코드: {response.StatusCode}")
    };
}
```

### 쿼리 파라미터 추가하기

쿼리 파라미터는 GET 요청에서 필터링, 검색, 페이지네이션 등을 구현하는 표준 방법입니다:

```csharp
async Task<string> GetWithParametersAsync()
{
    using HttpClient client = new HttpClient();
    
    // 방법 1: 수동으로 쿼리 문자열 구성
    string baseUrl = "https://api.example.com/search";
    string query = "?q=csharp&page=1&limit=10";
    string url = baseUrl + query;
    
    string response = await client.GetStringAsync(url);
    return response;
}

// 방법 2: UriBuilder를 사용한 안전한 구성
async Task<string> GetWithParametersSafeAsync()
{
    using HttpClient client = new HttpClient();
    
    var uriBuilder = new UriBuilder("https://api.example.com/search");
    
    // 쿼리 파라미터를 프로그래밍 방식으로 추가
    var query = System.Web.HttpUtility.ParseQueryString(string.Empty);
    query["q"] = "c#";  // 특수 문자 자동 인코딩
    query["page"] = "1";
    query["limit"] = "10";
    query["sort"] = "relevance";
    
    uriBuilder.Query = query.ToString();
    
    string response = await client.GetStringAsync(uriBuilder.ToString());
    return response;
}

// 방법 3: 확장 메서드로 더 깔끔하게
public static class HttpClientExtensions
{
    public static async Task<string> GetWithQueryAsync(
        this HttpClient client,
        string baseUrl,
        Dictionary<string, string> parameters)
    {
        var uriBuilder = new UriBuilder(baseUrl);
        var query = System.Web.HttpUtility.ParseQueryString(string.Empty);
        
        foreach (var param in parameters)
        {
            query[param.Key] = param.Value;
        }
        
        uriBuilder.Query = query.ToString();
        return await client.GetStringAsync(uriBuilder.Uri);
    }
}

// 사용
var parameters = new Dictionary<string, string>
{
    { "q", "c#" },
    { "page", "1" },
    { "limit", "10" }
};

string result = await client.GetWithQueryAsync(
    "https://api.example.com/search",
    parameters
);
```

**URL 인코딩의 중요성:**

특수 문자는 URL에서 특별한 의미를 가지므로, 퍼센트 인코딩(Percent-encoding)이 필요합니다:

```
공백(Space) → %20 또는 +
#           → %23
&           → %26
=           → %3D
?           → %3F
한글(가)    → %EA%B0%80
```

```csharp
// URL 인코딩 예제
string searchTerm = "C# & .NET";
string encoded = Uri.EscapeDataString(searchTerm);
// 결과: "C%23%20%26%20.NET"

string url = $"https://api.example.com/search?q={encoded}";
```

### 헤더 기반 콘텐츠 협상 (Content Negotiation)

REST API는 동일한 리소스를 여러 형식으로 제공할 수 있습니다. `Accept` 헤더를 통해 원하는 형식을 지정합니다:

```csharp
async Task<string> GetWithContentNegotiationAsync(string url)
{
    using var client = new HttpClient();
    
    // JSON 형식 요청
    client.DefaultRequestHeaders.Accept.Clear();
    client.DefaultRequestHeaders.Accept.Add(
        new System.Net.Http.Headers.MediaTypeWithQualityHeaderValue("application/json"));
    
    // 또는 XML 형식
    // client.DefaultRequestHeaders.Accept.Add(
    //     new MediaTypeWithQualityHeaderValue("application/xml"));
    
    // 품질 값(q)을 지정한 우선순위 설정
    // q=1.0이 가장 높은 우선순위, 0.0이 가장 낮음
    client.DefaultRequestHeaders.Accept.Add(
        new System.Net.Http.Headers.MediaTypeWithQualityHeaderValue("application/json", 1.0));
    client.DefaultRequestHeaders.Accept.Add(
        new System.Net.Http.Headers.MediaTypeWithQualityHeaderValue("application/xml", 0.8));
    
    var response = await client.GetAsync(url);
    
    // 실제 반환된 Content-Type 확인
    var contentType = response.Content.Headers.ContentType;
    Console.WriteLine($"응답 형식: {contentType}");
    
    return await response.Content.ReadAsStringAsync();
}
```

### 조건부 GET과 캐싱 최적화

HTTP 캐싱은 대역폭과 지연 시간을 크게 줄일 수 있습니다:

```csharp
async Task<string> GetWithCachingAsync(string url)
{
    using var client = new HttpClient();
    
    // 첫 번째 요청 - 전체 데이터 받기
    var initialResponse = await client.GetAsync(url);
    string content = await initialResponse.Content.ReadAsStringAsync();
    
    // ETag 또는 Last-Modified 저장
    var etag = initialResponse.Headers.ETag;
    var lastModified = initialResponse.Content.Headers.LastModified;
    
    Console.WriteLine($"첫 요청: {content.Length} bytes");
    Console.WriteLine($"ETag: {etag?.Tag}");
    Console.WriteLine($"Last-Modified: {lastModified}");
    
    // 두 번째 요청 - 조건부 GET
    await Task.Delay(1000);  // 시간 경과 시뮬레이션
    
    var request = new HttpRequestMessage(HttpMethod.Get, url);
    
    // If-None-Match: ETag 사용
    if (etag != null)
    {
        request.Headers.IfNoneMatch.Add(etag);
    }
    
    // If-Modified-Since: Last-Modified 사용
    if (lastModified.HasValue)
    {
        request.Headers.IfModifiedSince = lastModified;
    }
    
    var conditionalResponse = await client.SendAsync(request);
    
    if (conditionalResponse.StatusCode == HttpStatusCode.NotModified)
    {
        Console.WriteLine("304 Not Modified - 캐시 사용");
        return content;  // 캐시된 내용 반환
    }
    else
    {
        Console.WriteLine("200 OK - 새 데이터");
        return await conditionalResponse.Content.ReadAsStringAsync();
    }
}
```

**캐싱의 이점:**

- **대역폭 절감**: 304 응답은 헤더만 포함 (본문 없음)
- **지연 시간 감소**: 네트워크 전송 시간 절약
- **서버 부하 감소**: 서버가 데이터를 재생성하지 않음
- **비용 절감**: 클라우드 환경에서 데이터 전송 비용 절감

### 범위 요청 (Range Requests) - 부분 다운로드

큰 파일의 일부만 다운로드하거나, 중단된 다운로드를 재개할 수 있습니다:

```csharp
async Task<byte[]> DownloadRangeAsync(string url, long start, long end)
{
    using var client = new HttpClient();
    
    var request = new HttpRequestMessage(HttpMethod.Get, url);
    
    // Range 헤더 설정 (bytes=start-end)
    request.Headers.Range = new System.Net.Http.Headers.RangeHeaderValue(start, end);
    
    var response = await client.SendAsync(request);
    
    if (response.StatusCode == HttpStatusCode.PartialContent)  // 206
    {
        Console.WriteLine($"범위 요청 성공: {start}-{end}");
        
        // Content-Range 헤더 확인
        var contentRange = response.Content.Headers.ContentRange;
        Console.WriteLine($"Content-Range: bytes {contentRange.From}-{contentRange.To}/{contentRange.Length}");
        
        return await response.Content.ReadAsByteArrayAsync();
    }
    else if (response.StatusCode == HttpStatusCode.OK)
    {
        Console.WriteLine("서버가 범위 요청을 지원하지 않음 - 전체 파일 반환");
        return await response.Content.ReadAsByteArrayAsync();
    }
    else
    {
        throw new Exception($"범위 요청 실패: {response.StatusCode}");
    }
}

// 사용: 파일의 처음 1MB만 다운로드
byte[] partial = await DownloadRangeAsync(
    "https://example.com/large-file.zip",
    0,
    1024 * 1024 - 1  // 0부터 1MB-1까지
);
```

---

## 22.3 POST 요청으로 데이터 전송하기

POST 요청은 서버에 데이터를 전송하여 새로운 리소스를 생성하거나 작업을 수행할 때 사용합니다.

### 문자열 데이터 전송

```csharp
async Task PostStringDataAsync()
{
    using HttpClient client = new HttpClient();
    
    string url = "https://api.example.com/data";
    string jsonData = "{\"name\":\"홍길동\",\"age\":30}";
    
    StringContent content = new StringContent(
        jsonData,
        System.Text.Encoding.UTF8,
        "application/json"
    );
    
    try
    {
        HttpResponseMessage response = await client.PostAsync(url, content);
        
        if (response.IsSuccessStatusCode)
        {
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"응답: {result}");
        }
        else
        {
            Console.WriteLine($"실패: {response.StatusCode}");
        }
    }
    catch (Exception e)
    {
        Console.WriteLine($"오류: {e.Message}");
    }
}
```

### FormUrlEncodedContent 사용

```csharp
async Task PostFormDataAsync()
{
    using HttpClient client = new HttpClient();
    
    var formData = new Dictionary<string, string>
    {
        { "username", "user123" },
        { "email", "user@example.com" }
    };
    
    FormUrlEncodedContent content = new FormUrlEncodedContent(formData);
    
    HttpResponseMessage response = await client.PostAsync(
        "https://api.example.com/register",
        content
    );
    
    string result = await response.Content.ReadAsStringAsync();
    Console.WriteLine(result);
}
```

### PUT, DELETE 요청

```csharp
// PUT 요청 - 데이터 수정
async Task UpdateDataAsync(int id)
{
    using HttpClient client = new HttpClient();
    
    string jsonData = "{\"name\":\"김철수\",\"age\":25}";
    StringContent content = new StringContent(jsonData, 
        System.Text.Encoding.UTF8, "application/json");
    
    HttpResponseMessage response = await client.PutAsync(
        $"https://api.example.com/users/{id}",
        content
    );
    
    Console.WriteLine($"수정 결과: {response.StatusCode}");
}

// DELETE 요청 - 데이터 삭제
async Task DeleteDataAsync(int id)
{
    using HttpClient client = new HttpClient();
    
    HttpResponseMessage response = await client.DeleteAsync(
        $"https://api.example.com/users/{id}"
    );
    
    Console.WriteLine($"삭제 결과: {response.StatusCode}");
}
```

---

## 22.4 JSON 데이터 파싱

REST API는 주로 JSON 형식으로 데이터를 주고받습니다. C#에서는 `System.Text.Json` 네임스페이스를 사용하여 JSON을 처리합니다.

### JSON 역직렬화 (Deserialization)

```csharp
using System.Text.Json;

// JSON 데이터를 표현할 클래스
class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

async Task<User> GetUserAsync(int id)
{
    using HttpClient client = new HttpClient();
    
    string url = $"https://api.example.com/users/{id}";
    string json = await client.GetStringAsync(url);
    
    // JSON을 객체로 변환
    User user = JsonSerializer.Deserialize<User>(json);
    
    Console.WriteLine($"이름: {user.Name}");
    Console.WriteLine($"이메일: {user.Email}");
    
    return user;
}
```

### JSON 직렬화 (Serialization)

```csharp
async Task CreateUserAsync()
{
    using HttpClient client = new HttpClient();
    
    var newUser = new User
    {
        Name = "이영희",
        Email = "lee@example.com"
    };
    
    // 객체를 JSON으로 변환
    string json = JsonSerializer.Serialize(newUser);
    
    StringContent content = new StringContent(json,
        System.Text.Encoding.UTF8, "application/json");
    
    HttpResponseMessage response = await client.PostAsync(
        "https://api.example.com/users",
        content
    );
    
    if (response.IsSuccessStatusCode)
    {
        string result = await response.Content.ReadAsStringAsync();
        User createdUser = JsonSerializer.Deserialize<User>(result);
        Console.WriteLine($"생성된 사용자 ID: {createdUser.Id}");
    }
}
```

### JsonSerializerOptions 사용

```csharp
async Task<User> GetUserWithOptionsAsync(int id)
{
    using HttpClient client = new HttpClient();
    
    string json = await client.GetStringAsync(
        $"https://api.example.com/users/{id}"
    );
    
    // JSON 파싱 옵션 설정
    var options = new JsonSerializerOptions
    {
        PropertyNameCaseInsensitive = true,  // 대소문자 구분 안함
        WriteIndented = true  // 보기 좋게 포맷팅
    };
    
    User user = JsonSerializer.Deserialize<User>(json, options);
    return user;
}
```

### 복잡한 JSON 구조 처리

```csharp
class ApiResponse
{
    public bool Success { get; set; }
    public string Message { get; set; }
    public UserData Data { get; set; }
}

class UserData
{
    public int Id { get; set; }
    public string Name { get; set; }
    public List<string> Tags { get; set; }
}

async Task<ApiResponse> GetComplexDataAsync()
{
    using HttpClient client = new HttpClient();
    
    string json = await client.GetStringAsync("https://api.example.com/data");
    
    ApiResponse response = JsonSerializer.Deserialize<ApiResponse>(json);
    
    if (response.Success)
    {
        Console.WriteLine($"사용자: {response.Data.Name}");
        Console.WriteLine($"태그: {string.Join(", ", response.Data.Tags)}");
    }
    
    return response;
}
```

---

## 22.5 실제 공개 API와 연동하기

실제 공개 API를 사용하여 데이터를 받아오고 처리하는 실습 예제입니다.

### GitHub API 사용 예제

```csharp
using System.Text.Json;

class GitHubUser
{
    public string login { get; set; }
    public int id { get; set; }
    public string name { get; set; }
    public string bio { get; set; }
    public int public_repos { get; set; }
    public int followers { get; set; }
}

async Task GetGitHubUserInfoAsync(string username)
{
    using HttpClient client = new HttpClient();
    
    // GitHub API는 User-Agent 헤더 필수
    client.DefaultRequestHeaders.Add("User-Agent", "C# REST Client");
    
    try
    {
        string url = $"https://api.github.com/users/{username}";
        string json = await client.GetStringAsync(url);
        
        GitHubUser user = JsonSerializer.Deserialize<GitHubUser>(json);
        
        Console.WriteLine($"사용자명: {user.login}");
        Console.WriteLine($"이름: {user.name ?? "없음"}");
        Console.WriteLine($"소개: {user.bio ?? "없음"}");
        Console.WriteLine($"공개 저장소: {user.public_repos}개");
        Console.WriteLine($"팔로워: {user.followers}명");
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine($"GitHub API 오류: {e.Message}");
    }
}

// 사용
await GetGitHubUserInfoAsync("octocat");
```

### JSONPlaceholder API 사용 예제

```csharp
class Post
{
    public int userId { get; set; }
    public int id { get; set; }
    public string title { get; set; }
    public string body { get; set; }
}

async Task GetPostsAsync()
{
    using HttpClient client = new HttpClient();
    
    string url = "https://jsonplaceholder.typicode.com/posts";
    string json = await client.GetStringAsync(url);
    
    List<Post> posts = JsonSerializer.Deserialize<List<Post>>(json);
    
    Console.WriteLine($"총 {posts.Count}개의 게시물");
    
    // 처음 5개만 출력
    foreach (var post in posts.Take(5))
    {
        Console.WriteLine($"\n게시물 #{post.id}");
        Console.WriteLine($"제목: {post.title}");
        Console.WriteLine($"내용: {post.body.Substring(0, Math.Min(50, post.body.Length))}...");
    }
}

async Task CreatePostAsync()
{
    using HttpClient client = new HttpClient();
    
    var newPost = new Post
    {
        userId = 1,
        title = "새로운 게시물",
        body = "C#에서 작성한 게시물입니다."
    };
    
    string json = JsonSerializer.Serialize(newPost);
    StringContent content = new StringContent(json,
        System.Text.Encoding.UTF8, "application/json");
    
    HttpResponseMessage response = await client.PostAsync(
        "https://jsonplaceholder.typicode.com/posts",
        content
    );
    
    if (response.IsSuccessStatusCode)
    {
        string result = await response.Content.ReadAsStringAsync();
        Post created = JsonSerializer.Deserialize<Post>(result);
        Console.WriteLine($"생성된 게시물 ID: {created.id}");
    }
}
```

### 날씨 API 사용 예제

```csharp
class WeatherData
{
    public string name { get; set; }
    public Main main { get; set; }
    public List<Weather> weather { get; set; }
}

class Main
{
    public double temp { get; set; }
    public int humidity { get; set; }
}

class Weather
{
    public string description { get; set; }
}

async Task GetWeatherAsync(string city, string apiKey)
{
    using HttpClient client = new HttpClient();
    
    string url = $"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={apiKey}&units=metric&lang=kr";
    
    try
    {
        string json = await client.GetStringAsync(url);
        WeatherData weather = JsonSerializer.Deserialize<WeatherData>(json);
        
        Console.WriteLine($"도시: {weather.name}");
        Console.WriteLine($"온도: {weather.main.temp}°C");
        Console.WriteLine($"습도: {weather.main.humidity}%");
        Console.WriteLine($"날씨: {weather.weather[0].description}");
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine($"날씨 정보를 가져올 수 없습니다: {e.Message}");
    }
}
```

### 실전 REST API 클라이언트 클래스

```csharp
class RestApiClient
{
    private readonly HttpClient client;
    private readonly string baseUrl;
    
    public RestApiClient(string baseUrl)
    {
        this.baseUrl = baseUrl;
        this.client = new HttpClient
        {
            BaseAddress = new Uri(baseUrl)
        };
        
        // 기본 헤더 설정
        client.DefaultRequestHeaders.Add("User-Agent", "My REST Client");
    }
    
    public async Task<T> GetAsync<T>(string endpoint)
    {
        string json = await client.GetStringAsync(endpoint);
        return JsonSerializer.Deserialize<T>(json);
    }
    
    public async Task<T> PostAsync<T>(string endpoint, object data)
    {
        string json = JsonSerializer.Serialize(data);
        StringContent content = new StringContent(json,
            System.Text.Encoding.UTF8, "application/json");
        
        HttpResponseMessage response = await client.PostAsync(endpoint, content);
        response.EnsureSuccessStatusCode();
        
        string result = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<T>(result);
    }
    
    public async Task<bool> DeleteAsync(string endpoint)
    {
        HttpResponseMessage response = await client.DeleteAsync(endpoint);
        return response.IsSuccessStatusCode;
    }
}

// 사용 예
var apiClient = new RestApiClient("https://api.example.com");
var user = await apiClient.GetAsync<User>("/users/1");
var newUser = await apiClient.PostAsync<User>("/users", new { name = "홍길동" });
```

---

## 22장 정리

이 장에서는 C#에서 REST API 클라이언트를 작성하는 기본 방법을 학습했습니다:

- **HttpClient**: HTTP 요청을 보내기 위한 기본 클래스
- **GET 요청**: 서버에서 데이터를 조회하는 방법
- **POST 요청**: 서버에 데이터를 전송하는 방법  
- **JSON 처리**: `System.Text.Json`을 사용한 직렬화/역직렬화
- **실제 API 연동**: GitHub, JSONPlaceholder 등 공개 API 활용

REST API는 현대 웹 개발의 필수 요소이며, 이 장에서 배운 기초를 바탕으로 다양한 웹 서비스와 통신하는 애플리케이션을 만들 수 있습니다.

### 다음 단계

- API 인증 (API Key, OAuth)
- 에러 처리 및 재시도 로직
- HttpClientFactory 패턴
- Polly 라이브러리를 사용한 복원력 있는 HTTP 요청
