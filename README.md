# RaftLabs.UserService (.NET 8) – Assignment Solution

This repository contains a .NET 8 class library that integrates with the public [ReqRes API](https://reqres.in/) to fetch user data. It simulates real-world interaction with external APIs using best practices in code structure, resilience, and configuration.

---

## 📦 Projects Overview

RaftLabs.UserServiceSolution/
│
├── RaftLabs.UserService/ # .NET 8 Class Library (core service logic)
├── RaftLabs.UserService.Tests/ # Unit Test project (xUnit + Moq)
├── RaftLabs.ConsoleDemo/ # Optional Console App to demo the library
└── README.md # This file


---

## 🚀 Features

- ✅ Async API client using `HttpClient`
- ✅ Fetch single user or all users (with internal pagination)
- ✅ JSON response mapping to POCOs
- ✅ Graceful error handling
- ✅ Configurable base URL and API key
- ✅ Bonus: In-memory caching
- ✅ Bonus: Retry policy via [Polly](https://github.com/App-vNext/Polly)
- ✅ Bonus: `IOptions<T>` configuration pattern
- ✅ Bonus: Follows Clean Architecture folder boundaries

---

## 🛠 Build Instructions

### 1. Clone / Download Project

- If using ZIP: Unzip the solution into a working folder
- Or clone the repo:  
  ```bash
  git clone https://github.com/vivvicks/RaftLabs.UserService.git
2. Open in Visual Studio 2022+
Open the .sln file

Ensure .NET 8 SDK is installed

⚙️ Configuration
Create an appsettings.json in the RaftLabs.ConsoleDemo/ project:

json
Copy
Edit
{
  "ApiSettings": {
    "BaseUrl": "https://reqres.in/api",
    "ApiKey": "your-api-key",  // Optional if required by target API
    "CacheDurationSeconds": 60
  }
}
Make sure Copy to Output Directory is set to: Copy if newer.

🧪 Run & Test
Run the Console App
bash
Copy
Edit
dotnet run --project RaftLabs.ConsoleDemo
Run Unit Tests
bash
Copy
Edit
dotnet test RaftLabs.UserService.Tests
📐 Clean Architecture Principles
Models/ = Domain layer

Interfaces/ = Abstractions (ports)

Services/ = Application layer (use cases)

Configuration/ = Infrastructure configuration (external APIs, DI)

ExternalUserService = Implementation for an API gateway

✅ Bonus Features
1. Caching (In-Memory)
Implemented using MemoryCache. All user fetches are cached for CacheDurationSeconds.

csharp
Copy
Edit
services.AddMemoryCache();
services.AddScoped<IExternalUserService, ExternalUserService>();
2. Retry Logic with Polly
Retry added on transient HTTP failures (e.g., 5xx, timeouts):

csharp
Copy
Edit
services.AddHttpClient<IExternalUserService, ExternalUserService>()
    .AddTransientHttpErrorPolicy(builder =>
        builder.WaitAndRetryAsync(3, retry => TimeSpan.FromSeconds(Math.Pow(2, retry))));
3. Advanced Configuration Handling
Used .AddOptions() and IOptions<ApiSettings> to bind appsettings.json to POCO class.

csharp
Copy
Edit
services.Configure<ApiSettings>(config.GetSection("ApiSettings"));
4. Clean Architecture Ready
Code is organized by responsibility (not just technical layer)

Interfaces allow future swapping of API client, cache backend, etc.

💡 Design Decisions
Area	Decision
HTTP client	HttpClient with Polly for retries
Error handling	Central try-catch, meaningful exceptions, fallback for 404
Pagination	Handled internally via GetAllUsersAsync
Caching	Optional via IMemoryCache (per user + user list)
Config	IOptions<ApiSettings>
Architecture	Clean Architecture-aligned folder layout

📎 References
ReqRes API Docs

Microsoft.Extensions.Http

Polly

Clean Architecture
