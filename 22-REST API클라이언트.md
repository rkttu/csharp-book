# 22장. REST API 클라이언트

REST API는 현대 웹 애플리케이션에서 가장 널리 사용되는 통신 방식입니다. C#에서는 `HttpClient` 클래스를 사용하여 REST API와 통신할 수 있습니다. 이 장에서는 기본적인 HTTP 요청을 보내고 JSON 데이터를 처리하는 방법을 학습합니다.

---

## 22.1 HttpClient 소개

`HttpClient`는 .NET에서 HTTP 요청을 보내고 응답을 받기 위한 클래스입니다. 비동기 프로그래밍을 지원하며, REST API와의 통신에 최적화되어 있습니다.

### HttpClient 기본 사용법

```csharp
using System.Net.Http;

// HttpClient 인스턴스 생성
using HttpClient client = new HttpClient();

// BaseAddress 설정 (선택사항)
client.BaseAddress = new Uri("https://api.example.com/");

// 기본 헤더 설정
client.DefaultRequestHeaders.Add("User-Agent", "MyApp/1.0");
```

### HttpClient 사용 시 주의사항

- **싱글톤 패턴 권장**: `HttpClient`는 재사용하도록 설계되었습니다. 요청마다 새로 생성하지 마세요.
- **비동기 메서드 사용**: 모든 HTTP 작업은 `async`/`await`를 사용하여 비동기로 처리합니다.
- **리소스 해제**: `using` 문을 사용하거나 장기간 재사용합니다.

```csharp
// ❌ 나쁜 예 - 매번 새로 생성
async Task BadExampleAsync()
{
    using var client = new HttpClient();
    await client.GetStringAsync("https://api.example.com/data");
}

// ✅ 좋은 예 - 재사용
class ApiService
{
    private static readonly HttpClient client = new HttpClient();
    
    public async Task<string> GetDataAsync()
    {
        return await client.GetStringAsync("https://api.example.com/data");
    }
}
```

---

## 22.2 GET 요청으로 데이터 받기

GET 요청은 서버에서 데이터를 조회할 때 사용합니다. REST API에서 가장 기본적인 작업입니다.

### GetStringAsync 사용하기

```csharp
async Task<string> GetDataAsStringAsync(string url)
{
    using HttpClient client = new HttpClient();
    
    try
    {
        string response = await client.GetStringAsync(url);
        Console.WriteLine("응답 데이터:");
        Console.WriteLine(response);
        return response;
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine($"요청 실패: {e.Message}");
        return string.Empty;
    }
}

// 사용 예
string data = await GetDataAsStringAsync("https://api.github.com/users/octocat");
```

### GetAsync로 상세 정보 받기

```csharp
async Task GetDataWithDetailsAsync(string url)
{
    using HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("User-Agent", "C# App");
    
    try
    {
        HttpResponseMessage response = await client.GetAsync(url);
        
        // 상태 코드 확인
        Console.WriteLine($"상태 코드: {response.StatusCode}");
        Console.WriteLine($"성공 여부: {response.IsSuccessStatusCode}");
        
        // 응답 본문 읽기
        if (response.IsSuccessStatusCode)
        {
            string content = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"응답 내용: {content}");
        }
        else
        {
            Console.WriteLine("요청 실패");
        }
    }
    catch (Exception e)
    {
        Console.WriteLine($"오류 발생: {e.Message}");
    }
}
```

### 쿼리 파라미터 추가하기

```csharp
async Task<string> GetWithParametersAsync()
{
    using HttpClient client = new HttpClient();
    
    // URL에 쿼리 파라미터 추가
    string baseUrl = "https://api.example.com/search";
    string query = "?q=csharp&page=1&limit=10";
    string url = baseUrl + query;
    
    string response = await client.GetStringAsync(url);
    return response;
}
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
