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

POST 메서드는 HTTP에서 **리소스를 생성(Create)**하거나 **작업을 수행(Action)**하는 용도로 사용됩니다. GET과 달리 POST는 **안전하지 않으며(Not Safe)** **멱등하지 않습니다(Not Idempotent)**. 동일한 POST 요청을 여러 번 수행하면 여러 개의 리소스가 생성되거나 여러 번 작업이 실행될 수 있습니다.

**POST vs PUT vs PATCH의 의미론적 차이:**

REST 설계에서 리소스 조작 메서드의 선택은 매우 중요합니다:

```
POST   /users          → 새 사용자 생성 (ID는 서버가 할당)
                         비멱등적: 여러 번 호출 시 여러 사용자 생성

PUT    /users/123      → 사용자 123을 전체 교체 (없으면 생성)
                         멱등적: 여러 번 호출해도 결과 동일

PATCH  /users/123      → 사용자 123의 일부 속성만 수정
                         비멱등적 (구현에 따라 다름)

DELETE /users/123      → 사용자 123 삭제
                         멱등적: 이미 삭제된 것 다시 삭제해도 동일
```

**POST의 실무 사용 사례:**

1. **리소스 생성**: `/api/orders` - 새 주문 생성
2. **컨트롤러 액션**: `/api/orders/123/cancel` - 주문 취소 (RPC 스타일)
3. **검색 (복잡한 쿼리)**: `/api/search` - GET으로는 표현하기 어려운 복잡한 검색
4. **배치 작업**: `/api/users/batch` - 여러 사용자를 한 번에 처리
5. **파일 업로드**: `/api/files` - 멀티파트 폼 데이터 전송

**HTTP 요청 본문의 구조:**

POST 요청은 요청 본문(Request Body)에 데이터를 포함합니다. 본문의 형식은 `Content-Type` 헤더로 지정됩니다:

```
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json          ← 미디어 타입 지정
Content-Length: 45                      ← 본문 크기 (bytes)
Authorization: Bearer eyJhbGc...        ← 인증 토큰 (선택적)

{"name":"홍길동","age":30}                ← 실제 데이터
```

**주요 Content-Type:**

- `application/json`: JSON 데이터 (RESTful API의 표준)
- `application/x-www-form-urlencoded`: HTML 폼 데이터 (URL 인코딩)
- `multipart/form-data`: 파일 업로드와 폼 데이터 혼합
- `application/xml`: XML 데이터 (레거시 시스템)
- `text/plain`: 일반 텍스트
- `application/octet-stream`: 바이너리 데이터

### JSON 데이터 전송 - StringContent 사용

JSON은 REST API에서 사실상의 표준 데이터 형식입니다:

```csharp
async Task PostJsonDataAsync()
{
    using HttpClient client = new HttpClient();
    
    string url = "https://api.example.com/users";
    
    // JSON 문자열 직접 작성
    string jsonData = "{\"name\":\"홍길동\",\"age\":30,\"email\":\"hong@example.com\"}";
    
    // StringContent로 HTTP 본문 생성
    // 인코딩과 Content-Type을 명시적으로 지정
    StringContent content = new StringContent(
        jsonData,
        System.Text.Encoding.UTF8,  // UTF-8 인코딩 (JSON 표준)
        "application/json"           // MIME 타입
    );
    
    try
    {
        // POST 요청 전송
        HttpResponseMessage response = await client.PostAsync(url, content);
        
        // 응답 상태 확인
        Console.WriteLine($"상태 코드: {response.StatusCode}");
        Console.WriteLine($"성공 여부: {response.IsSuccessStatusCode}");
        
        if (response.IsSuccessStatusCode)
        {
            // 201 Created: 새 리소스 생성 성공
            // Location 헤더에 새 리소스의 URI가 포함될 수 있음
            if (response.Headers.Location != null)
            {
                Console.WriteLine($"생성된 리소스 위치: {response.Headers.Location}");
            }
            
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"응답 본문: {result}");
        }
        else
        {
            // 오류 응답 본문 확인 (API 오류 메시지 포함)
            string errorContent = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"오류: {response.StatusCode}");
            Console.WriteLine($"오류 내용: {errorContent}");
        }
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine($"HTTP 오류: {e.Message}");
        if (e.InnerException != null)
        {
            Console.WriteLine($"내부 예외: {e.InnerException.Message}");
        }
    }
    catch (TaskCanceledException)
    {
        Console.WriteLine("요청 시간 초과 (Timeout)");
    }
}
```

**HTTP 응답 본문의 의미:**

POST 요청 후 서버는 다양한 응답을 반환할 수 있습니다:

```csharp
// 일반적인 POST 응답 패턴
async Task<(bool Success, string ResourceId, string ErrorMessage)> CreateResourceAsync()
{
    var response = await client.PostAsync(url, content);
    
    switch (response.StatusCode)
    {
        case HttpStatusCode.Created:  // 201
            // 리소스 생성 성공
            // Location 헤더에서 새 리소스 URI 추출
            string location = response.Headers.Location?.ToString();
            string resourceId = location?.Split('/').Last();
            return (true, resourceId, null);
            
        case HttpStatusCode.OK:  // 200
            // 작업 성공 (리소스 생성은 아닐 수 있음)
            return (true, null, null);
            
        case HttpStatusCode.Accepted:  // 202
            // 요청 수락, 비동기 처리 중
            // 응답 본문에 작업 상태 확인 URI가 포함될 수 있음
            return (true, null, "비동기 처리 중");
            
        case HttpStatusCode.BadRequest:  // 400
            // 잘못된 요청 (유효성 검증 실패)
            string validationError = await response.Content.ReadAsStringAsync();
            return (false, null, validationError);
            
        case HttpStatusCode.Conflict:  // 409
            // 충돌 (예: 중복 생성 시도)
            return (false, null, "리소스가 이미 존재합니다");
            
        default:
            string error = await response.Content.ReadAsStringAsync();
            return (false, null, $"{response.StatusCode}: {error}");
    }
}
```

### FormUrlEncodedContent - HTML 폼 데이터 전송

HTML 폼 데이터 형식은 웹 폼의 기본 인코딩 방식입니다:

```csharp
async Task PostFormDataAsync()
{
    using HttpClient client = new HttpClient();
    
    // Key-Value 쌍으로 폼 데이터 구성
    var formData = new Dictionary<string, string>
    {
        { "username", "user123" },
        { "email", "user@example.com" },
        { "password", "SecurePassword123!" },
        { "accept_terms", "true" }
    };
    
    // FormUrlEncodedContent는 자동으로:
    // 1. URL 인코딩 적용 (특수 문자 → %XX)
    // 2. Content-Type을 application/x-www-form-urlencoded로 설정
    FormUrlEncodedContent content = new FormUrlEncodedContent(formData);
    
    HttpResponseMessage response = await client.PostAsync(
        "https://api.example.com/register",
        content
    );
    
    // 전송되는 실제 본문 형식:
    // username=user123&email=user%40example.com&password=SecurePassword123!&accept_terms=true
    
    if (response.IsSuccessStatusCode)
    {
        string result = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"등록 성공: {result}");
    }
    else
    {
        Console.WriteLine($"등록 실패: {response.StatusCode}");
    }
}
```

**폼 데이터 vs JSON:**

| 특성 | application/x-www-form-urlencoded | application/json |
|------|-----------------------------------|------------------|
| 구조 | 평면 Key-Value | 계층적, 중첩 가능 |
| 배열 | 어려움 | 간단함 |
| 타입 정보 | 없음 (모두 문자열) | 있음 (숫자, 불리언 등) |
| 사용 사례 | HTML 폼, OAuth | REST API, 복잡한 데이터 |
| 가독성 | 낮음 | 높음 |

### MultipartFormDataContent - 파일 업로드

파일과 폼 데이터를 함께 전송할 때 사용합니다:

```csharp
async Task UploadFileWithMetadataAsync(string filePath)
{
    using var client = new HttpClient();
    using var content = new MultipartFormDataContent();
    
    // 1. 파일 추가
    byte[] fileBytes = await File.ReadAllBytesAsync(filePath);
    var fileContent = new ByteArrayContent(fileBytes);
    fileContent.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/jpeg");
    
    // 파일 이름 지정
    content.Add(fileContent, "file", Path.GetFileName(filePath));
    
    // 2. 메타데이터 추가 (폼 필드)
    content.Add(new StringContent("My Image"), "title");
    content.Add(new StringContent("A beautiful sunset"), "description");
    content.Add(new StringContent("public"), "visibility");
    
    // 3. POST 요청
    var response = await client.PostAsync("https://api.example.com/upload", content);
    
    if (response.IsSuccessStatusCode)
    {
        string result = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"업로드 성공: {result}");
    }
    else
    {
        Console.WriteLine($"업로드 실패: {response.StatusCode}");
    }
}

// 스트림을 사용한 대용량 파일 업로드
async Task UploadLargeFileAsync(string filePath)
{
    using var client = new HttpClient();
    using var content = new MultipartFormDataContent();
    
    // 파일을 스트림으로 읽어 메모리 효율적으로 업로드
    using var fileStream = File.OpenRead(filePath);
    var streamContent = new StreamContent(fileStream);
    streamContent.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("application/octet-stream");
    
    content.Add(streamContent, "file", Path.GetFileName(filePath));
    
    // 진행률 추적을 위한 커스텀 핸들러 사용 가능
    var response = await client.PostAsync("https://api.example.com/upload", content);
    
    Console.WriteLine($"업로드 결과: {response.StatusCode}");
}
```

**Multipart 본문의 구조:**

```
POST /upload HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="image.jpg"
Content-Type: image/jpeg

[바이너리 이미지 데이터]
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="title"

My Image
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="description"

A beautiful sunset
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

### PUT 요청 - 리소스 전체 교체

PUT은 **리소스 전체를 교체(Replace)**하는 멱등적 작업입니다:

```csharp
// PUT의 의미론: 리소스의 모든 속성을 교체
async Task UpdateUserAsync(int userId, User updatedUser)
{
    using HttpClient client = new HttpClient();
    
    // 사용자 객체를 JSON으로 직렬화
    string jsonData = JsonSerializer.Serialize(updatedUser);
    
    var content = new StringContent(
        jsonData,
        System.Text.Encoding.UTF8,
        "application/json"
    );
    
    // PUT은 리소스 ID를 URL에 포함
    string url = $"https://api.example.com/users/{userId}";
    HttpResponseMessage response = await client.PutAsync(url, content);
    
    if (response.StatusCode == HttpStatusCode.OK)  // 200
    {
        Console.WriteLine("사용자 정보 수정 성공");
    }
    else if (response.StatusCode == HttpStatusCode.NoContent)  // 204
    {
        Console.WriteLine("사용자 정보 수정 성공 (응답 본문 없음)");
    }
    else if (response.StatusCode == HttpStatusCode.NotFound)  // 404
    {
        Console.WriteLine("사용자를 찾을 수 없습니다");
    }
    else
    {
        string error = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"수정 실패: {response.StatusCode} - {error}");
    }
}
```

**PUT의 멱등성:**

```csharp
// 동일한 PUT을 여러 번 수행해도 결과는 같음
await client.PutAsync("/users/123", content);  // 첫 호출
await client.PutAsync("/users/123", content);  // 두 번째 호출 - 결과 동일
await client.PutAsync("/users/123", content);  // 세 번째 호출 - 결과 동일
```

### PATCH 요청 - 리소스 부분 수정

PATCH는 **리소스의 일부만 수정(Partial Update)**합니다. RFC 5789에서 정의되었습니다:

```csharp
async Task PartialUpdateUserAsync(int userId)
{
    using HttpClient client = new HttpClient();
    
    // JSON Patch 형식 (RFC 6902)
    string patchData = @"
    [
        { ""op"": ""replace"", ""path"": ""/email"", ""value"": ""new@example.com"" },
        { ""op"": ""add"", ""path"": ""/phone"", ""value"": ""010-1234-5678"" },
        { ""op"": ""remove"", ""path"": ""/temporary_field"" }
    ]";
    
    var content = new StringContent(
        patchData,
        System.Text.Encoding.UTF8,
        "application/json-patch+json"  // JSON Patch의 미디어 타입
    );
    
    // PATCH 메서드 사용
    var request = new HttpRequestMessage(new HttpMethod("PATCH"), 
        $"https://api.example.com/users/{userId}");
    request.Content = content;
    
    var response = await client.SendAsync(request);
    
    if (response.IsSuccessStatusCode)
    {
        Console.WriteLine("부분 수정 성공");
    }
}

// 간단한 PATCH (단순 JSON 사용)
async Task SimplePatchAsync(int userId)
{
    using var client = new HttpClient();
    
    // 수정할 필드만 포함
    var patchData = new
    {
        email = "new@example.com",
        phone = "010-1234-5678"
    };
    
    string json = JsonSerializer.Serialize(patchData);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    
    var request = new HttpRequestMessage(new HttpMethod("PATCH"), 
        $"https://api.example.com/users/{userId}");
    request.Content = content;
    
    var response = await client.SendAsync(request);
    Console.WriteLine($"PATCH 결과: {response.StatusCode}");
}
```

### DELETE 요청 - 리소스 삭제

DELETE는 **리소스를 삭제(Remove)**하는 멱등적 작업입니다:

```csharp
async Task DeleteResourceAsync(int userId)
{
    using HttpClient client = new HttpClient();
    
    string url = $"https://api.example.com/users/{userId}";
    HttpResponseMessage response = await client.DeleteAsync(url);
    
    switch (response.StatusCode)
    {
        case HttpStatusCode.OK:  // 200
            // 삭제 성공, 삭제된 리소스 정보 반환
            string deletedResource = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"삭제됨: {deletedResource}");
            break;
            
        case HttpStatusCode.NoContent:  // 204
            // 삭제 성공, 응답 본문 없음 (가장 일반적)
            Console.WriteLine("리소스 삭제 성공");
            break;
            
        case HttpStatusCode.NotFound:  // 404
            // 리소스가 존재하지 않음
            // 멱등성 때문에 이미 삭제된 경우도 성공으로 간주 가능
            Console.WriteLine("리소스가 존재하지 않음 (이미 삭제됨?)");
            break;
            
        case HttpStatusCode.Conflict:  // 409
            // 삭제 불가 (예: 다른 리소스가 참조 중)
            string conflictMessage = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"삭제 불가: {conflictMessage}");
            break;
            
        default:
            Console.WriteLine($"삭제 실패: {response.StatusCode}");
            break;
    }
}
```

**DELETE의 멱등성과 비즈니스 로직:**

```csharp
// 멱등적 DELETE: 여러 번 호출해도 안전
await client.DeleteAsync("/users/123");  // 첫 호출 - 삭제됨
await client.DeleteAsync("/users/123");  // 두 번째 호출 - 404 또는 204 (안전함)

// 소프트 삭제(Soft Delete) 구현
async Task SoftDeleteAsync(int userId)
{
    // DELETE 대신 PATCH로 is_deleted 플래그 설정
    var patchData = new { is_deleted = true, deleted_at = DateTime.UtcNow };
    string json = JsonSerializer.Serialize(patchData);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    
    var request = new HttpRequestMessage(new HttpMethod("PATCH"), 
        $"https://api.example.com/users/{userId}");
    request.Content = content;
    
    var response = await client.SendAsync(request);
    Console.WriteLine($"소프트 삭제: {response.StatusCode}");
}
```

### 인증 헤더 추가 - Bearer Token

대부분의 REST API는 인증을 요구합니다:

```csharp
async Task AuthenticatedRequestAsync(string token)
{
    using var client = new HttpClient();
    
    // Authorization 헤더에 Bearer 토큰 추가
    client.DefaultRequestHeaders.Authorization = 
        new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
    
    // 또는 직접 헤더 추가
    // client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
    
    var userData = new { name = "홍길동", email = "hong@example.com" };
    string json = JsonSerializer.Serialize(userData);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    
    var response = await client.PostAsync("https://api.example.com/users", content);
    
    if (response.StatusCode == HttpStatusCode.Unauthorized)  // 401
    {
        Console.WriteLine("인증 실패: 토큰이 유효하지 않습니다");
    }
    else if (response.StatusCode == HttpStatusCode.Forbidden)  // 403
    {
        Console.WriteLine("권한 없음: 이 작업을 수행할 권한이 없습니다");
    }
    else if (response.IsSuccessStatusCode)
    {
        Console.WriteLine("요청 성공");
    }
}

// API Key 인증
async Task ApiKeyRequestAsync(string apiKey)
{
    using var client = new HttpClient();
    
    // 커스텀 헤더로 API Key 전송
    client.DefaultRequestHeaders.Add("X-API-Key", apiKey);
    
    // 또는 쿼리 파라미터로 전송 (보안상 권장하지 않음)
    // string url = $"https://api.example.com/data?api_key={apiKey}";
    
    var response = await client.GetAsync("https://api.example.com/data");
    Console.WriteLine($"API Key 인증: {response.StatusCode}");
}
```

---

## 22.4 JSON 데이터 파싱

JSON(JavaScript Object Notation)은 2001년 Douglas Crockford가 제안한 경량 데이터 교환 형식으로, XML의 복잡성에 대한 반발로 탄생했습니다. JSON은 **인간이 읽기 쉽고(Human-readable)**, **기계가 파싱하기 쉬우며(Machine-parseable)**, **언어 독립적(Language-independent)**인 특성으로 REST API의 사실상 표준 데이터 형식이 되었습니다.

**JSON의 역사적 맥락:**

- **2001년**: Douglas Crockford가 JSON 명세 발표 (json.org)
- **2005년**: AJAX (Asynchronous JavaScript and XML)의 부상과 함께 JSON 대중화
- **2006년**: RFC 4627로 표준화
- **2013년**: ECMA-404 국제 표준
- **2017년**: RFC 8259로 갱신 (현재 표준)
- **2020년대**: GraphQL, gRPC 등 새로운 프로토콜과 공존

JSON은 XML 대비 다음과 같은 장점이 있습니다:

| 특성 | JSON | XML |
|------|------|-----|
| 크기 | 작음 (~30% 절감) | 큼 (태그 오버헤드) |
| 가독성 | 높음 | 중간 (태그 verbose) |
| 파싱 속도 | 빠름 | 느림 |
| 데이터 타입 | 내장 (숫자, 불리언) | 없음 (모두 문자열) |
| 네임스페이스 | 없음 | 있음 (XML 복잡성) |
| 스키마 검증 | JSON Schema | XSD |

**C#의 JSON 라이브러리 진화:**

- **.NET Framework 3.5**: `JavaScriptSerializer` (System.Web.Script.Serialization) - 느리고 제한적
- **.NET Framework 4.5**: `DataContractJsonSerializer` - 복잡하고 무거움
- **Newtonsoft.Json (Json.NET)**: 2008년부터 사실상 표준, 기능 풍부, 널리 사용됨
- **.NET Core 3.0+**: `System.Text.Json` - 고성능, 제로 할당 목표, 내장 라이브러리

`System.Text.Json`은 성능과 보안을 최우선으로 설계되었으며, 다음과 같은 특징이 있습니다:

- **고성능**: Span<T>와 Memory<T>를 활용한 제로 할당 파싱
- **Utf8JsonReader**: UTF-8 바이트를 직접 읽어 변환 오버헤드 제거
- **보안**: 기본적으로 안전한 설정 (깊이 제한, 순환 참조 방지)
- **AOT 친화적**: Ahead-of-Time 컴파일 지원

### JSON 역직렬화 (Deserialization) - JSON → C# 객체

역직렬화는 JSON 문자열을 C# 객체로 변환하는 과정입니다:

```csharp
using System.Text.Json;

// JSON 데이터를 표현할 DTO (Data Transfer Object)
class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public DateTime CreatedAt { get; set; }
    public bool IsActive { get; set; }
}

async Task<User> GetUserAsync(int id)
{
    using HttpClient client = new HttpClient();
    
    string url = $"https://api.example.com/users/{id}";
    string json = await client.GetStringAsync(url);
    
    // JSON을 객체로 변환
    // Deserialize<T>는 제네릭 메서드로 타입 안전성 제공
    User user = JsonSerializer.Deserialize<User>(json);
    
    // null 체크 (JSON이 올바르지 않거나 변환 실패 시)
    if (user == null)
    {
        throw new InvalidOperationException("사용자 데이터를 파싱할 수 없습니다");
    }
    
    Console.WriteLine($"ID: {user.Id}");
    Console.WriteLine($"이름: {user.Name}");
    Console.WriteLine($"이메일: {user.Email}");
    Console.WriteLine($"생성일: {user.CreatedAt:yyyy-MM-dd}");
    Console.WriteLine($"활성: {user.IsActive}");
    
    return user;
}
```

**JSON과 C# 타입 매핑:**

| JSON 타입 | C# 타입 |
|-----------|---------|
| `null` | `null` |
| `true`, `false` | `bool` |
| 숫자 | `int`, `long`, `double`, `decimal` |
| 문자열 | `string` |
| 배열 | `List<T>`, `T[]`, `IEnumerable<T>` |
| 객체 | `class`, `record`, `struct` |

```json
{
  "id": 123,
  "name": "홍길동",
  "email": "hong@example.com",
  "createdAt": "2024-01-15T10:30:00Z",
  "isActive": true,
  "tags": ["developer", "admin"],
  "metadata": {
    "lastLogin": "2024-11-15T07:00:00Z"
  }
}
```

### JSON 직렬화 (Serialization) - C# 객체 → JSON

직렬화는 C# 객체를 JSON 문자열로 변환하는 과정입니다:

```csharp
async Task CreateUserAsync()
{
    using HttpClient client = new HttpClient();
    
    // C# 객체 생성
    var newUser = new User
    {
        Name = "이영희",
        Email = "lee@example.com",
        CreatedAt = DateTime.UtcNow,
        IsActive = true
    };
    
    // 객체를 JSON으로 변환
    // Serialize는 객체의 public 속성을 JSON으로 변환
    string json = JsonSerializer.Serialize(newUser);
    
    // 결과 JSON (한 줄로 압축됨):
    // {"Id":0,"Name":"이영희","Email":"lee@example.com","CreatedAt":"2024-11-15T07:00:00Z","IsActive":true}
    
    Console.WriteLine($"직렬화된 JSON: {json}");
    
    // HTTP 요청 본문으로 사용
    var content = new StringContent(json,
        System.Text.Encoding.UTF8, 
        "application/json");
    
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

### JsonSerializerOptions - 세밀한 제어

`JsonSerializerOptions`는 직렬화/역직렬화 동작을 제어합니다:

```csharp
async Task<User> GetUserWithOptionsAsync(int id)
{
    using HttpClient client = new HttpClient();
    
    string json = await client.GetStringAsync(
        $"https://api.example.com/users/{id}"
    );
    
    // 옵션 설정
    var options = new JsonSerializerOptions
    {
        // 1. 속성 이름 대소문자 구분 안함
        // API: "Name" ↔ C#: "name" 자동 매칭
        PropertyNameCaseInsensitive = true,
        
        // 2. 들여쓰기로 보기 좋게 포맷팅 (디버깅용)
        WriteIndented = true,
        
        // 3. 속성 이름을 camelCase로 (C#: PascalCase → JSON: camelCase)
        PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
        
        // 4. null 값을 가진 속성 무시
        DefaultIgnoreCondition = System.Text.Json.Serialization.JsonIgnoreCondition.WhenWritingNull,
        
        // 5. 읽기 전용 속성 포함 (get만 있는 속성)
        IncludeFields = false,
        
        // 6. 숫자를 문자열로 읽기 허용
        NumberHandling = System.Text.Json.Serialization.JsonNumberHandling.AllowReadingFromString,
        
        // 7. 주석 허용 (표준 JSON은 주석 미지원)
        ReadCommentHandling = JsonCommentHandling.Skip,
        
        // 8. 트레일링 쉼표 허용 (표준 JSON은 불허)
        AllowTrailingCommas = true,
        
        // 9. 최대 깊이 (순환 참조 방지)
        MaxDepth = 64
    };
    
    User user = JsonSerializer.Deserialize<User>(json, options);
    
    // 직렬화 시에도 동일한 옵션 사용
    string serialized = JsonSerializer.Serialize(user, options);
    Console.WriteLine(serialized);
    
    return user;
}
```

**옵션 재사용과 성능:**

```csharp
// ✅ 좋은 예: 옵션을 static readonly로 재사용
public class ApiClient
{
    private static readonly JsonSerializerOptions DefaultOptions = new()
    {
        PropertyNameCaseInsensitive = true,
        PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
        DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
    };
    
    public async Task<T> GetAsync<T>(string url)
    {
        string json = await httpClient.GetStringAsync(url);
        return JsonSerializer.Deserialize<T>(json, DefaultOptions);
    }
}
```

### 복잡한 JSON 구조와 중첩 객체

실제 API는 중첩된 복잡한 JSON을 반환합니다:

```csharp
// API 응답 래퍼 패턴
class ApiResponse<T>
{
    public bool Success { get; set; }
    public string Message { get; set; }
    public T Data { get; set; }
    public ErrorInfo Error { get; set; }
    public PaginationInfo Pagination { get; set; }
}

class ErrorInfo
{
    public string Code { get; set; }
    public string Message { get; set; }
    public Dictionary<string, string[]> ValidationErrors { get; set; }
}

class PaginationInfo
{
    public int Page { get; set; }
    public int PageSize { get; set; }
    public int TotalPages { get; set; }
    public int TotalItems { get; set; }
}

class UserData
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public List<string> Tags { get; set; }
    public Address Address { get; set; }
    public List<Order> RecentOrders { get; set; }
}

class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string ZipCode { get; set; }
}

class Order
{
    public int OrderId { get; set; }
    public decimal Total { get; set; }
    public DateTime OrderDate { get; set; }
}

async Task<UserData> GetComplexDataAsync()
{
    using HttpClient client = new HttpClient();
    
    string json = await client.GetStringAsync("https://api.example.com/users/123");
    
    // 복잡한 JSON을 중첩 객체로 파싱
    var response = JsonSerializer.Deserialize<ApiResponse<UserData>>(json);
    
    if (!response.Success)
    {
        Console.WriteLine($"오류: {response.Error.Code} - {response.Error.Message}");
        
        // 검증 오류 출력
        if (response.Error.ValidationErrors != null)
        {
            foreach (var (field, errors) in response.Error.ValidationErrors)
            {
                Console.WriteLine($"  {field}: {string.Join(", ", errors)}");
            }
        }
        
        return null;
    }
    
    var user = response.Data;
    Console.WriteLine($"사용자: {user.Name} ({user.Email})");
    Console.WriteLine($"주소: {user.Address.City}, {user.Address.Street}");
    Console.WriteLine($"태그: {string.Join(", ", user.Tags)}");
    
    Console.WriteLine($"\n최근 주문:");
    foreach (var order in user.RecentOrders)
    {
        Console.WriteLine($"  주문 #{order.OrderId}: {order.Total:C} ({order.OrderDate:yyyy-MM-dd})");
    }
    
    // 페이지네이션 정보
    if (response.Pagination != null)
    {
        Console.WriteLine($"\n페이지 {response.Pagination.Page}/{response.Pagination.TotalPages}");
    }
    
    return user;
}
```

### 속성 이름 변환 - JsonPropertyName 특성

API의 JSON 속성 이름과 C# 명명 규칙이 다를 때 매핑합니다:

```csharp
using System.Text.Json.Serialization;

class GitHubUser
{
    // JSON: "login" ↔ C#: "Username"
    [JsonPropertyName("login")]
    public string Username { get; set; }
    
    // JSON: "avatar_url" ↔ C#: "AvatarUrl"
    [JsonPropertyName("avatar_url")]
    public string AvatarUrl { get; set; }
    
    // JSON: "public_repos" ↔ C#: "PublicRepositories"
    [JsonPropertyName("public_repos")]
    public int PublicRepositories { get; set; }
    
    // JSON: "created_at" ↔ C#: "CreatedAt"
    [JsonPropertyName("created_at")]
    public DateTime CreatedAt { get; set; }
    
    // 속성 무시
    [JsonIgnore]
    public string InternalNote { get; set; }
    
    // 읽기만 허용 (역직렬화만, 직렬화 제외)
    [JsonIgnore(Condition = JsonIgnoreCondition.WhenWritingDefault)]
    public int CalculatedScore { get; set; }
}

// GitHub API 사용 예
async Task<GitHubUser> GetGitHubUserAsync(string username)
{
    using var client = new HttpClient();
    client.DefaultRequestHeaders.Add("User-Agent", "MyApp");
    
    string json = await client.GetStringAsync($"https://api.github.com/users/{username}");
    
    var user = JsonSerializer.Deserialize<GitHubUser>(json);
    
    Console.WriteLine($"사용자: {user.Username}");
    Console.WriteLine($"저장소: {user.PublicRepositories}개");
    Console.WriteLine($"가입일: {user.CreatedAt:yyyy-MM-dd}");
    
    return user;
}
```

### 동적 JSON과 JsonDocument

타입이 미리 정의되지 않은 JSON을 처리할 때는 `JsonDocument`를 사용합니다:

```csharp
async Task ParseDynamicJsonAsync()
{
    using var client = new HttpClient();
    string json = await client.GetStringAsync("https://api.example.com/dynamic-data");
    
    // JsonDocument는 읽기 전용 DOM (Document Object Model)
    using JsonDocument document = JsonDocument.Parse(json);
    
    // 루트 요소 접근
    JsonElement root = document.RootElement;
    
    // 속성 존재 확인 및 접근
    if (root.TryGetProperty("users", out JsonElement usersElement))
    {
        // 배열 순회
        foreach (JsonElement userElement in usersElement.EnumerateArray())
        {
            string name = userElement.GetProperty("name").GetString();
            int age = userElement.GetProperty("age").GetInt32();
            
            Console.WriteLine($"{name} ({age}세)");
            
            // 선택적 속성
            if (userElement.TryGetProperty("email", out JsonElement emailElement))
            {
                Console.WriteLine($"  이메일: {emailElement.GetString()}");
            }
        }
    }
    
    // 중첩 객체 탐색
    if (root.TryGetProperty("metadata", out JsonElement metadata))
    {
        if (metadata.TryGetProperty("timestamp", out JsonElement timestamp))
        {
            DateTime dt = timestamp.GetDateTime();
            Console.WriteLine($"타임스탬프: {dt}");
        }
    }
}
```

### 스트리밍 JSON 파싱 - Utf8JsonReader

대용량 JSON을 메모리 효율적으로 파싱합니다:

```csharp
async Task ParseLargeJsonAsync()
{
    using var client = new HttpClient();
    using var stream = await client.GetStreamAsync("https://api.example.com/large-data");
    
    // UTF-8 바이트를 직접 읽어 제로 할당 파싱
    byte[] buffer = new byte[4096];
    int bytesRead = await stream.ReadAsync(buffer);
    
    var reader = new Utf8JsonReader(buffer.AsSpan(0, bytesRead));
    
    string currentProperty = null;
    
    while (reader.Read())
    {
        switch (reader.TokenType)
        {
            case JsonTokenType.PropertyName:
                currentProperty = reader.GetString();
                break;
                
            case JsonTokenType.String:
                if (currentProperty == "name")
                {
                    string value = reader.GetString();
                    Console.WriteLine($"이름: {value}");
                }
                break;
                
            case JsonTokenType.Number:
                if (currentProperty == "age")
                {
                    int value = reader.GetInt32();
                    Console.WriteLine($"나이: {value}");
                }
                break;
        }
    }
}
```

### JSON 검증과 오류 처리

```csharp
async Task<User> SafeDeserializeAsync(string url)
{
    using var client = new HttpClient();
    
    try
    {
        string json = await client.GetStringAsync(url);
        
        // 옵션으로 엄격한 검증
        var options = new JsonSerializerOptions
        {
            AllowTrailingCommas = false,  // 트레일링 쉼표 불허
            ReadCommentHandling = JsonCommentHandling.Disallow,  // 주석 불허
            MaxDepth = 32  // 깊이 제한
        };
        
        User user = JsonSerializer.Deserialize<User>(json, options);
        
        // 수동 검증
        if (string.IsNullOrWhiteSpace(user.Name))
        {
            throw new InvalidDataException("사용자 이름이 유효하지 않습니다");
        }
        
        if (!user.Email.Contains("@"))
        {
            throw new InvalidDataException("이메일 형식이 올바르지 않습니다");
        }
        
        return user;
    }
    catch (JsonException ex)
    {
        // JSON 파싱 오류
        Console.WriteLine($"JSON 파싱 실패: {ex.Message}");
        Console.WriteLine($"위치: Line {ex.LineNumber}, Position {ex.BytePositionInLine}");
        throw;
    }
    catch (NotSupportedException ex)
    {
        // 지원되지 않는 JSON 구조
        Console.WriteLine($"지원되지 않는 JSON: {ex.Message}");
        throw;
    }
}
```

---

## 22.5 실제 공개 API와 연동하기

실제 공개 API와의 통합은 이론을 실전으로 전환하는 결정적 단계입니다. 이 절에서는 GitHub, JSONPlaceholder, OpenWeatherMap 등 실제 서비스의 API를 사용하여, **인증(Authentication)**, **Rate Limiting**, **오류 처리(Error Handling)**, **페이지네이션(Pagination)** 등 실무에서 마주하는 다양한 상황을 경험합니다.

**공개 API 선택 시 고려사항:**

1. **인증 요구사항**: API Key, OAuth 2.0, Bearer Token 등
2. **Rate Limit**: 요청 횟수 제한 (예: 60 requests/hour)
3. **문서화 품질**: API 엔드포인트, 파라미터, 응답 형식 명세
4. **안정성**: SLA (Service Level Agreement), 가용성
5. **비용**: 무료 tier, 유료 플랜, 초과 요금
6. **CORS 정책**: 브라우저 환경에서의 제약사항

### GitHub API - 버전 관리 시스템 데이터

GitHub REST API v3는 GitHub의 데이터를 프로그래밍 방식으로 접근할 수 있게 합니다. 인증 없이도 시간당 60회 요청이 가능하며, 인증 시 5,000회로 증가합니다.

```csharp
using System.Text.Json;
using System.Text.Json.Serialization;

// GitHub API 응답을 위한 DTO
class GitHubUser
{
    [JsonPropertyName("login")]
    public string Login { get; set; }
    
    [JsonPropertyName("id")]
    public int Id { get; set; }
    
    [JsonPropertyName("name")]
    public string Name { get; set; }
    
    [JsonPropertyName("bio")]
    public string Bio { get; set; }
    
    [JsonPropertyName("public_repos")]
    public int PublicRepos { get; set; }
    
    [JsonPropertyName("followers")]
    public int Followers { get; set; }
    
    [JsonPropertyName("following")]
    public int Following { get; set; }
    
    [JsonPropertyName("created_at")]
    public DateTime CreatedAt { get; set; }
    
    [JsonPropertyName("updated_at")]
    public DateTime UpdatedAt { get; set; }
    
    [JsonPropertyName("avatar_url")]
    public string AvatarUrl { get; set; }
}

class GitHubRepository
{
    [JsonPropertyName("id")]
    public int Id { get; set; }
    
    [JsonPropertyName("name")]
    public string Name { get; set; }
    
    [JsonPropertyName("full_name")]
    public string FullName { get; set; }
    
    [JsonPropertyName("description")]
    public string Description { get; set; }
    
    [JsonPropertyName("stargazers_count")]
    public int Stars { get; set; }
    
    [JsonPropertyName("forks_count")]
    public int Forks { get; set; }
    
    [JsonPropertyName("language")]
    public string Language { get; set; }
    
    [JsonPropertyName("created_at")]
    public DateTime CreatedAt { get; set; }
}

class GitHubApiClient
{
    private readonly HttpClient _client;
    private readonly JsonSerializerOptions _jsonOptions;
    
    public GitHubApiClient(string userAgent = "C# REST API Client")
    {
        _client = new HttpClient
        {
            BaseAddress = new Uri("https://api.github.com/")
        };
        
        // GitHub API는 User-Agent 헤더 필수
        _client.DefaultRequestHeaders.Add("User-Agent", userAgent);
        
        // GitHub API v3 버전 지정 (선택적, 안정성 확보)
        _client.DefaultRequestHeaders.Add("Accept", "application/vnd.github.v3+json");
        
        _jsonOptions = new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        };
    }
    
    // 사용자 정보 조회
    public async Task<GitHubUser> GetUserAsync(string username)
    {
        try
        {
            string json = await _client.GetStringAsync($"users/{username}");
            return JsonSerializer.Deserialize<GitHubUser>(json, _jsonOptions);
        }
        catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
        {
            Console.WriteLine($"사용자 '{username}'을 찾을 수 없습니다.");
            return null;
        }
        catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.Forbidden)
        {
            Console.WriteLine("API Rate Limit 초과. 잠시 후 다시 시도하세요.");
            return null;
        }
    }
    
    // 사용자의 저장소 목록 조회 (페이지네이션)
    public async Task<List<GitHubRepository>> GetUserRepositoriesAsync(
        string username, 
        int page = 1, 
        int perPage = 30)
    {
        try
        {
            // 쿼리 파라미터로 페이지네이션
            string url = $"users/{username}/repos?page={page}&per_page={perPage}&sort=updated";
            string json = await _client.GetStringAsync(url);
            
            return JsonSerializer.Deserialize<List<GitHubRepository>>(json, _jsonOptions);
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"저장소 조회 실패: {ex.Message}");
            return new List<GitHubRepository>();
        }
    }
    
    // Rate Limit 정보 확인
    public async Task CheckRateLimitAsync()
    {
        var response = await _client.GetAsync("rate_limit");
        string json = await response.Content.ReadAsStringAsync();
        
        // Rate limit 헤더 확인
        if (response.Headers.TryGetValues("X-RateLimit-Limit", out var limitValues))
        {
            Console.WriteLine($"시간당 허용 요청: {limitValues.First()}");
        }
        
        if (response.Headers.TryGetValues("X-RateLimit-Remaining", out var remainingValues))
        {
            Console.WriteLine($"남은 요청: {remainingValues.First()}");
        }
        
        if (response.Headers.TryGetValues("X-RateLimit-Reset", out var resetValues))
        {
            long resetUnix = long.Parse(resetValues.First());
            DateTime resetTime = DateTimeOffset.FromUnixTimeSeconds(resetUnix).LocalDateTime;
            Console.WriteLine($"리셋 시간: {resetTime}");
        }
    }
}

// 사용 예제
async Task DemoGitHubApiAsync()
{
    var github = new GitHubApiClient();
    
    // Rate Limit 확인
    await github.CheckRateLimitAsync();
    
    // 사용자 정보 조회
    var user = await github.GetUserAsync("torvalds");
    if (user != null)
    {
        Console.WriteLine($"\n사용자: {user.Login}");
        Console.WriteLine($"이름: {user.Name}");
        Console.WriteLine($"Bio: {user.Bio}");
        Console.WriteLine($"공개 저장소: {user.PublicRepos}개");
        Console.WriteLine($"팔로워: {user.Followers}명");
        Console.WriteLine($"가입일: {user.CreatedAt:yyyy-MM-dd}");
    }
    
    // 저장소 목록 조회
    var repos = await github.GetUserRepositoriesAsync("torvalds", page: 1, perPage: 5);
    Console.WriteLine($"\n최근 업데이트된 저장소 (상위 {repos.Count}개):");
    
    foreach (var repo in repos)
    {
        Console.WriteLine($"\n  {repo.FullName}");
        Console.WriteLine($"  설명: {repo.Description ?? "설명 없음"}");
        Console.WriteLine($"  언어: {repo.Language ?? "N/A"}");
        Console.WriteLine($"  ⭐ {repo.Stars} | 🍴 {repo.Forks}");
    }
}
```

### JSONPlaceholder - 테스트용 REST API

JSONPlaceholder는 무료 테스트 API로, 프로토타이핑과 학습에 ideal합니다. 인증이 필요 없고, CRUD 작업을 모두 지원합니다 (실제 데이터는 변경되지 않음).

```csharp
class Post
{
    [JsonPropertyName("userId")]
    public int UserId { get; set; }
    
    [JsonPropertyName("id")]
    public int Id { get; set; }
    
    [JsonPropertyName("title")]
    public string Title { get; set; }
    
    [JsonPropertyName("body")]
    public string Body { get; set; }
}

class Comment
{
    [JsonPropertyName("postId")]
    public int PostId { get; set; }
    
    [JsonPropertyName("id")]
    public int Id { get; set; }
    
    [JsonPropertyName("name")]
    public string Name { get; set; }
    
    [JsonPropertyName("email")]
    public string Email { get; set; }
    
    [JsonPropertyName("body")]
    public string Body { get; set; }
}

class JsonPlaceholderClient
{
    private readonly HttpClient _client;
    private readonly JsonSerializerOptions _jsonOptions;
    
    public JsonPlaceholderClient()
    {
        _client = new HttpClient
        {
            BaseAddress = new Uri("https://jsonplaceholder.typicode.com/")
        };
        
        _jsonOptions = new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true,
            WriteIndented = true
        };
    }
    
    // 모든 게시물 조회
    public async Task<List<Post>> GetAllPostsAsync()
    {
        string json = await _client.GetStringAsync("posts");
        return JsonSerializer.Deserialize<List<Post>>(json, _jsonOptions);
    }
    
    // 특정 게시물 조회
    public async Task<Post> GetPostAsync(int id)
    {
        string json = await _client.GetStringAsync($"posts/{id}");
        return JsonSerializer.Deserialize<Post>(json, _jsonOptions);
    }
    
    // 게시물 생성 (실제로는 생성되지 않지만 201 Created 응답)
    public async Task<Post> CreatePostAsync(Post newPost)
    {
        string jsonContent = JsonSerializer.Serialize(newPost, _jsonOptions);
        var content = new StringContent(jsonContent, Encoding.UTF8, "application/json");
        
        var response = await _client.PostAsync("posts", content);
        response.EnsureSuccessStatusCode();
        
        string jsonResponse = await response.Content.ReadAsStringAsync();
        var created = JsonSerializer.Deserialize<Post>(jsonResponse, _jsonOptions);
        
        Console.WriteLine($"게시물 생성됨: ID={created.Id}");
        return created;
    }
    
    // 게시물 수정
    public async Task<Post> UpdatePostAsync(int id, Post updatedPost)
    {
        updatedPost.Id = id;  // ID 유지
        
        string jsonContent = JsonSerializer.Serialize(updatedPost, _jsonOptions);
        var content = new StringContent(jsonContent, Encoding.UTF8, "application/json");
        
        var response = await _client.PutAsync($"posts/{id}", content);
        response.EnsureSuccessStatusCode();
        
        string jsonResponse = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<Post>(jsonResponse, _jsonOptions);
    }
    
    // 게시물 부분 수정
    public async Task<Post> PatchPostAsync(int id, object partialUpdate)
    {
        string jsonContent = JsonSerializer.Serialize(partialUpdate, _jsonOptions);
        var content = new StringContent(jsonContent, Encoding.UTF8, "application/json");
        
        var request = new HttpRequestMessage(new HttpMethod("PATCH"), $"posts/{id}")
        {
            Content = content
        };
        
        var response = await _client.SendAsync(request);
        response.EnsureSuccessStatusCode();
        
        string jsonResponse = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<Post>(jsonResponse, _jsonOptions);
    }
    
    // 게시물 삭제
    public async Task<bool> DeletePostAsync(int id)
    {
        var response = await _client.DeleteAsync($"posts/{id}");
        return response.IsSuccessStatusCode;
    }
    
    // 게시물의 댓글 조회
    public async Task<List<Comment>> GetPostCommentsAsync(int postId)
    {
        string json = await _client.GetStringAsync($"posts/{postId}/comments");
        return JsonSerializer.Deserialize<List<Comment>>(json, _jsonOptions);
    }
}

// CRUD 작업 데모
async Task DemoJsonPlaceholderAsync()
{
    var client = new JsonPlaceholderClient();
    
    // CREATE - 새 게시물 생성
    Console.WriteLine("=== CREATE ===");
    var newPost = new Post
    {
        UserId = 1,
        Title = "C#에서 REST API 호출하기",
        Body = "HttpClient를 사용하여 REST API와 통신하는 방법을 학습했습니다."
    };
    
    var created = await client.CreatePostAsync(newPost);
    Console.WriteLine($"생성된 게시물 ID: {created.Id}");
    
    // READ - 게시물 조회
    Console.WriteLine("\n=== READ ===");
    var post = await client.GetPostAsync(1);
    Console.WriteLine($"제목: {post.Title}");
    Console.WriteLine($"내용: {post.Body.Substring(0, Math.Min(50, post.Body.Length))}...");
    
    // UPDATE - 전체 수정
    Console.WriteLine("\n=== UPDATE (PUT) ===");
    post.Title = "수정된 제목";
    post.Body = "수정된 내용입니다.";
    var updated = await client.UpdatePostAsync(1, post);
    Console.WriteLine($"수정됨: {updated.Title}");
    
    // PATCH - 부분 수정
    Console.WriteLine("\n=== PATCH ===");
    var patched = await client.PatchPostAsync(1, new { title = "부분 수정된 제목" });
    Console.WriteLine($"부분 수정됨: {patched.Title}");
    
    // DELETE - 삭제
    Console.WriteLine("\n=== DELETE ===");
    bool deleted = await client.DeletePostAsync(1);
    Console.WriteLine($"삭제 성공: {deleted}");
    
    // 댓글 조회
    Console.WriteLine("\n=== Comments ===");
    var comments = await client.GetPostCommentsAsync(1);
    Console.WriteLine($"게시물 #1의 댓글 {comments.Count}개");
    foreach (var comment in comments.Take(3))
    {
        Console.WriteLine($"  - {comment.Name} ({comment.Email})");
        Console.WriteLine($"    {comment.Body.Substring(0, Math.Min(60, comment.Body.Length))}...\n");
    }
}
```

### 실전 REST API 클라이언트 - 재사용 가능한 추상화

프로덕션급 API 클라이언트는 오류 처리, 재시도, 로깅 등을 포함해야 합니다:

```csharp
class RestApiClient : IDisposable
{
    private readonly HttpClient _client;
    private readonly JsonSerializerOptions _jsonOptions;
    private readonly string _baseUrl;
    
    public RestApiClient(string baseUrl, string authToken = null)
    {
        _baseUrl = baseUrl;
        _client = new HttpClient
        {
            BaseAddress = new Uri(baseUrl),
            Timeout = TimeSpan.FromSeconds(30)
        };
        
        // 기본 헤더 설정
        _client.DefaultRequestHeaders.Add("User-Agent", "Advanced REST Client");
        _client.DefaultRequestHeaders.Add("Accept", "application/json");
        
        // 인증 토큰이 있으면 추가
        if (!string.IsNullOrEmpty(authToken))
        {
            _client.DefaultRequestHeaders.Authorization = 
                new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", authToken);
        }
        
        _jsonOptions = new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true,
            DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,
            PropertyNamingPolicy = JsonNamingPolicy.CamelCase
        };
    }
    
    // GET 요청
    public async Task<T> GetAsync<T>(string endpoint)
    {
        try
        {
            var response = await _client.GetAsync(endpoint);
            await EnsureSuccessStatusCodeAsync(response);
            
            string json = await response.Content.ReadAsStringAsync();
            return JsonSerializer.Deserialize<T>(json, _jsonOptions);
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"GET 요청 실패: {endpoint}");
            Console.WriteLine($"오류: {ex.Message}");
            throw;
        }
    }
    
    // POST 요청
    public async Task<TResponse> PostAsync<TRequest, TResponse>(string endpoint, TRequest data)
    {
        try
        {
            string json = JsonSerializer.Serialize(data, _jsonOptions);
            var content = new StringContent(json, Encoding.UTF8, "application/json");
            
            var response = await _client.PostAsync(endpoint, content);
            await EnsureSuccessStatusCodeAsync(response);
            
            string responseJson = await response.Content.ReadAsStringAsync();
            return JsonSerializer.Deserialize<TResponse>(responseJson, _jsonOptions);
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"POST 요청 실패: {endpoint}");
            Console.WriteLine($"오류: {ex.Message}");
            throw;
        }
    }
    
    // PUT 요청
    public async Task<TResponse> PutAsync<TRequest, TResponse>(string endpoint, TRequest data)
    {
        try
        {
            string json = JsonSerializer.Serialize(data, _jsonOptions);
            var content = new StringContent(json, Encoding.UTF8, "application/json");
            
            var response = await _client.PutAsync(endpoint, content);
            await EnsureSuccessStatusCodeAsync(response);
            
            string responseJson = await response.Content.ReadAsStringAsync();
            return JsonSerializer.Deserialize<TResponse>(responseJson, _jsonOptions);
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"PUT 요청 실패: {endpoint}");
            Console.WriteLine($"오류: {ex.Message}");
            throw;
        }
    }
    
    // DELETE 요청
    public async Task<bool> DeleteAsync(string endpoint)
    {
        try
        {
            var response = await _client.DeleteAsync(endpoint);
            return response.IsSuccessStatusCode;
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"DELETE 요청 실패: {endpoint}");
            Console.WriteLine($"오류: {ex.Message}");
            return false;
        }
    }
    
    // 오류 응답 처리
    private async Task EnsureSuccessStatusCodeAsync(HttpResponseMessage response)
    {
        if (response.IsSuccessStatusCode)
            return;
        
        string errorContent = await response.Content.ReadAsStringAsync();
        
        throw new HttpRequestException(
            $"HTTP {(int)response.StatusCode} {response.StatusCode}\n" +
            $"응답: {errorContent}",
            null,
            response.StatusCode);
    }
    
    public void Dispose()
    {
        _client?.Dispose();
    }
}

// 사용 예제
async Task DemoRestApiClientAsync()
{
    using var client = new RestApiClient("https://jsonplaceholder.typicode.com");
    
    // GET
    var post = await client.GetAsync<Post>("/posts/1");
    Console.WriteLine($"조회: {post.Title}");
    
    // POST
    var newPost = new Post
    {
        UserId = 1,
        Title = "New Post",
        Body = "Content"
    };
    var created = await client.PostAsync<Post, Post>("/posts", newPost);
    Console.WriteLine($"생성: ID={created.Id}");
    
    // PUT
    created.Title = "Updated Title";
    var updated = await client.PutAsync<Post, Post>($"/posts/{created.Id}", created);
    Console.WriteLine($"수정: {updated.Title}");
    
    // DELETE
    bool deleted = await client.DeleteAsync($"/posts/{created.Id}");
    Console.WriteLine($"삭제: {deleted}");
}
```

---

## 22장 정리 및 요약

이 장에서는 C#에서 REST API 클라이언트를 작성하는 포괄적인 방법을 학습했습니다. REST 아키텍처의 역사적 맥락부터 실전 코드까지, 현대 분산 시스템 개발의 핵심 기술을 습득했습니다.

### 핵심 개념의 재조명

**1. REST 아키텍처의 본질:**

Roy Fielding이 2000년에 제안한 REST는 단순한 API 설계 방법이 아니라, 웹의 확장성을 극대화하기 위한 **아키텍처 스타일**입니다. 6가지 제약 조건(무상태성, 캐시 가능성, 계층화된 시스템 등)을 통해 분산 시스템의 복잡도를 관리하고, HTTP 프로토콜의 본래 의도를 충실히 따릅니다.

REST는 SOAP의 복잡성에 대한 반발로 대중화되었으며, 2010년대 모바일 혁명과 클라우드 컴퓨팅의 부상으로 사실상의 표준이 되었습니다. 오늘날 마이크로서비스 아키텍처의 핵심 통신 메커니즘이며, Netflix, Amazon, Google 등 대규모 서비스의 기반입니다.

**2. HttpClient의 설계 철학과 함정:**

`HttpClient`는 .NET 4.5에서 도입된 현대적인 HTTP 클라이언트로, 비동기 우선, 재사용 가능, 확장 가능한 설계를 따릅니다. 그러나 잘못 사용하면 **소켓 고갈(Socket Exhaustion)**과 **DNS 캐싱** 문제를 야기할 수 있습니다.

핵심 원칙:
- ✅ **싱글톤 패턴**: `HttpClient`를 애플리케이션 생명주기 동안 재사용
- ✅ **HttpClientFactory**: .NET Core 2.1+에서 소켓 관리와 DNS 갱신 자동화
- ❌ **안티패턴**: 요청마다 새로운 `HttpClient` 생성 (TIME_WAIT 상태 누적)
- ❌ **동기 차단**: `.Result`나 `.Wait()` 사용 (데드락 위험)

**3. HTTP 메서드의 의미론과 실무 적용:**

HTTP 메서드는 단순한 CRUD 매핑이 아니라, **안전성(Safety)**과 **멱등성(Idempotency)**이라는 수학적 속성을 가집니다:

- **GET**: 안전하고 멱등적 → 자동 재시도 가능, 캐싱 가능
- **POST**: 안전하지 않고 비멱등적 → 중복 요청 주의, 리소스 생성
- **PUT**: 비안전하지만 멱등적 → 전체 교체, 여러 번 호출해도 안전
- **PATCH**: 비안전하고 비멱등적 → 부분 수정, RFC 5789 표준
- **DELETE**: 비안전하지만 멱등적 → 삭제, 이미 삭제된 것 재삭제 가능

이러한 의미론은 네트워크 장애, 재시도 로직, 캐싱 전략의 기반이 되며, RESTful API 설계의 핵심입니다.

**4. JSON의 진화와 System.Text.Json:**

JSON은 2001년 Douglas Crockford가 XML의 복잡성에 대한 대안으로 제안했으며, 현재 REST API의 사실상 표준 데이터 형식입니다. C#의 JSON 라이브러리는 다음과 같이 진화했습니다:

- **초기**: `JavaScriptSerializer`, `DataContractJsonSerializer` - 느리고 제한적
- **중기**: Newtonsoft.Json (Json.NET) - 기능 풍부, 널리 사용됨
- **현대**: `System.Text.Json` - 고성능, 제로 할당, 보안 우선

`System.Text.Json`은 Span<T>와 Memory<T>를 활용한 **제로 할당 파싱**, Utf8JsonReader의 **직접 UTF-8 파싱**, 그리고 기본적으로 안전한 설정(깊이 제한, 순환 참조 방지)으로 성능과 보안을 동시에 달성합니다.

**5. 실전 API 통합의 도전과제:**

실제 API와의 통합에서 마주하는 도전:

- **인증과 권한**: API Key, OAuth 2.0, JWT 토큰 관리
- **Rate Limiting**: 요청 한도 준수, 429 Too Many Requests 처리
- **오류 처리**: 일시적 오류 vs 영구적 오류, 재시도 전략
- **페이지네이션**: Offset-based, Cursor-based, Link headers
- **버전 관리**: URI 버전 vs Header 버전 vs Media Type 버전
- **타임아웃**: 적절한 타임아웃 설정, 부분 응답 처리
- **CORS**: 브라우저 환경에서의 Cross-Origin 제약

### 실무에서의 모범 사례

**1. 복원력 있는 HTTP 통신 (Resilient HTTP):**

```csharp
// Polly를 사용한 재시도와 Circuit Breaker
services.AddHttpClient("ResilientApi")
    .AddTransientHttpErrorPolicy(p => 
        p.WaitAndRetryAsync(3, retryAttempt => 
            TimeSpan.FromSeconds(Math.Pow(2, retryAttempt))))
    .AddTransientHttpErrorPolicy(p =>
        p.CircuitBreakerAsync(5, TimeSpan.FromSeconds(30)));
```

**2. 구조화된 로깅:**

```csharp
// HTTP 요청/응답 로깅
services.AddHttpClient("LoggedApi")
    .AddLogger<ConsoleLoggingHandler>();
```

**3. 테스트 가능한 설계:**

```csharp
// HttpClient를 인터페이스로 추상화
public interface IApiClient
{
    Task<T> GetAsync<T>(string endpoint);
}

// 테스트에서 Mock 사용
var mockClient = new Mock<IApiClient>();
mockClient.Setup(x => x.GetAsync<User>("/users/1"))
    .ReturnsAsync(new User { Id = 1, Name = "Test" });
```

### 다음 단계와 심화 학습

이 장에서 다룬 기초를 바탕으로 다음 주제로 확장할 수 있습니다:

**고급 인증:**
- **OAuth 2.0 Flow**: Authorization Code, Refresh Token, PKCE
- **OpenID Connect**: 신원 확인과 SSO (Single Sign-On)
- **JWT (JSON Web Tokens)**: Claims 기반 인증, 토큰 검증

**성능 최적화:**
- **HTTP/2 멀티플렉싱**: 단일 연결에서 여러 요청 동시 처리
- **HTTP/3 (QUIC)**: UDP 기반 프로토콜, 더 낮은 지연 시간
- **Connection Pooling**: SocketsHttpHandler의 내부 최적화
- **압축**: Gzip, Brotli 압축 활용

**복원력 패턴 (Resilience Patterns):**
- **Polly 라이브러리**: 재시도, Circuit Breaker, Bulkhead, Timeout
- **지수 백오프(Exponential Backoff)**: 재시도 간격 점진적 증가
- **Jitter**: 재시도 타이밍에 무작위성 추가로 thundering herd 방지

**관찰 가능성 (Observability):**
- **분산 추적(Distributed Tracing)**: OpenTelemetry, Jaeger
- **메트릭(Metrics)**: Prometheus, Grafana
- **구조화된 로깅(Structured Logging)**: Serilog, ELK Stack

**API 설계 원칙:**
- **Richardson Maturity Model**: Level 0-3의 REST 성숙도
- **HATEOAS**: Hypermedia As The Engine Of Application State
- **API 버저닝**: 하위 호환성 유지 전략
- **GraphQL**: REST의 대안, 정확한 데이터 요청

### 권장 리소스

**공식 문서:**
- Microsoft: "HttpClient guidelines for .NET"
- RFC 7230-7235: HTTP/1.1 명세
- RFC 7231: HTTP Semantics and Content
- RFC 6902: JSON Patch

**도서:**
- "RESTful Web APIs" by Leonard Richardson & Mike Amundsen
- "Building Microservices" by Sam Newman
- "Web API Design" by Arnaud Lauret

**온라인 리소스:**
- GitHub REST API v3 Documentation
- JSONPlaceholder (https://jsonplaceholder.typicode.com/)
- Public APIs List (https://github.com/public-apis/public-apis)

REST API 클라이언트 개발은 현대 소프트웨어 엔지니어링의 필수 기술입니다. 이 장에서 학습한 원칙과 패턴은 마이크로서비스, 클라우드 네이티브 애플리케이션, 모바일 앱 개발 등 다양한 분야에 적용됩니다. 이론적 기반과 실무 경험을 결합하여, 견고하고 확장 가능한 분산 시스템을 구축할 수 있습니다!
