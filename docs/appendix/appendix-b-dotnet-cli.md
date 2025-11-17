# 부록 B. .NET CLI 명령어

.NET CLI는 크로스 플랫폼으로 작동하는 명령줄 도구입니다. 이 부록에서는 자주 사용하는 핵심 명령어를 빠르게 참조할 수 있도록 정리했습니다.

## B.1 프로젝트 관리

### 새 프로젝트 생성

```bash
# 콘솔 애플리케이션
dotnet new console -n MyApp

# 클래스 라이브러리
dotnet new classlib -n MyLibrary

# 웹 API
dotnet new webapi -n MyApi

# xUnit 테스트 프로젝트
dotnet new xunit -n MyTests
```

### 솔루션 관리

```bash
# 새 솔루션 생성
dotnet new sln -n MySolution

# 솔루션에 프로젝트 추가
dotnet sln add MyApp/MyApp.csproj

# 프로젝트 참조 추가
dotnet add reference ../MyLibrary/MyLibrary.csproj
```

## B.2 빌드와 실행

```bash
# 빌드
dotnet build

# Release 구성으로 빌드
dotnet build -c Release

# 실행
dotnet run

# Release로 게시
dotnet publish -c Release -o ./publish
```

## B.3 패키지 관리

```bash
# 패키지 추가
dotnet add package Newtonsoft.Json

# 패키지 제거
dotnet remove package Newtonsoft.Json

# 패키지 복원
dotnet restore

# 설치된 패키지 목록
dotnet list package
```

## B.4 테스트 실행

```bash
# 모든 테스트 실행
dotnet test

# 코드 커버리지 수집
dotnet test /p:CollectCoverage=true
```

더 자세한 옵션은 공식 문서를 참조하세요: <https://learn.microsoft.com/dotnet/core/tools/>
