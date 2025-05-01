# Code Generation Guide

This guide explains the code generation options and customization features available in REST Express.

## Table of Contents
- [Basic Configuration](#basic-configuration)
- [Method Types](#method-types)
- [Naming Conventions](#naming-conventions)
- [Type Generation](#type-generation)
- [Advanced Options](#advanced-options)
- [Custom Templates](#custom-templates)

## Basic Configuration

### Script Settings
```csharp
// Basic settings
ScriptName = "ApiClient"           // Generated class name
MethodType = "Async"               // Async or Coroutine
OutputPath = "Assets/Scripts/API"  // Where to save generated files
```

### Namespace Configuration
```csharp
// Generated code structure
namespace YourGame.API
{
    public partial class ApiClient
    {
        // Generated methods
    }
}
```

## Method Types

### Async Methods
```csharp
// Generated async method
public async Task<UserProfile> GetUserProfileAsync(
    string userId,
    CancellationToken cancellationToken = default)
{
    var response = await SendRequestAsync<UserProfile>(
        $"users/{userId}",
        HttpMethod.Get,
        cancellationToken: cancellationToken
    );
    return response;
}
```

### Coroutine Methods
```csharp
// Generated coroutine method
public IEnumerator GetUserProfile(
    string userId,
    Action<UserProfile> onSuccess,
    Action<string> onError = null)
{
    yield return SendRequest<UserProfile>(
        $"users/{userId}",
        HttpMethod.Get,
        onSuccess,
        onError
    );
}
```

## Naming Conventions

### Method Names
```csharp
// Request name → Method name
"Get User Profile" → GetUserProfileAsync
"Create New Post" → CreateNewPostAsync
"Update Settings" → UpdateSettingsAsync
```

### Parameter Names
```csharp
// URL parameters
"{userId}"     → string userId
"{postId}"     → string postId
"?limit={num}" → int limit

// Body parameters
"userData"     → UserData userData
"settings"     → Settings settings
```

## Type Generation

### Response Types
```csharp
// From JSON example
{
    "user": {
        "id": "123",
        "name": "John",
        "scores": [10, 20, 30]
    }
}

// Generated types
public class UserResponse
{
    public User User { get; set; }
}

public class User
{
    public string Id { get; set; }
    public string Name { get; set; }
    public List<int> Scores { get; set; }
}
```

### Request Types
```csharp
// From request body
{
    "email": "user@example.com",
    "password": "secret"
}

// Generated type
public class LoginRequest
{
    public string Email { get; set; }
    public string Password { get; set; }
}
```

## Advanced Options

### Custom Base Class
```csharp
// Custom base class
public abstract class CustomApiClient
{
    protected readonly string baseUrl;
    protected readonly ILogger logger;
    
    protected CustomApiClient(string baseUrl, ILogger logger)
    {
        this.baseUrl = baseUrl;
        this.logger = logger;
    }
}

// Generated client
public partial class ApiClient : CustomApiClient
{
    public ApiClient(string baseUrl, ILogger logger)
        : base(baseUrl, logger)
    {
    }
}
```

### Response Wrapping
```csharp
// Custom response wrapper
public class ApiResponse<T>
{
    public T Data { get; set; }
    public bool Success { get; set; }
    public string Message { get; set; }
}

// Usage in generated code
public async Task<ApiResponse<UserProfile>> GetUserProfileAsync(
    string userId)
{
    return await SendRequestAsync<ApiResponse<UserProfile>>(
        $"users/{userId}"
    );
}
```

### Error Handling
```csharp
// Custom error handling
public class ApiException : Exception
{
    public int StatusCode { get; }
    public string ResponseBody { get; }
    
    public ApiException(int statusCode, string message)
        : base(message)
    {
        StatusCode = statusCode;
    }
}

// Generated error handling
try
{
    return await SendRequestAsync<T>(endpoint);
}
catch (HttpRequestException ex)
{
    throw new ApiException(500, ex.Message);
}
```

## Custom Templates

### Template Variables
```csharp
// Available template variables
${ClassName}        // ApiClient
${MethodName}       // GetUserProfile
${ReturnType}       // UserProfile
${Parameters}       // string userId, int limit
${Endpoint}         // users/{userId}
${HttpMethod}       // GET, POST, etc.
```

### Custom Method Template
```csharp
// Template file: async_method.template
public async Task<${ReturnType}> ${MethodName}Async(
    ${Parameters},
    CancellationToken cancellationToken = default)
{
    var response = await SendRequestAsync<${ReturnType}>(
        "${Endpoint}",
        HttpMethod.${HttpMethod},
        cancellationToken: cancellationToken
    );
    
    // Custom logging
    logger.Log($"${MethodName} completed: {response}");
    
    return response;
}
```

### Customizing Generation
1. Create template files in `Editor/Templates/`
2. Select template in Script Generator window
3. Generate code using custom template

## Best Practices

1. **Consistent Naming**
   ```csharp
   // Good
   GetUserProfile
   CreateNewPost
   UpdateSettings
   
   // Avoid
   get_user
   createPost
   UPDATE_SETTINGS
   ```

2. **Type Organization**
   ```csharp
   // Group related types
   namespace YourGame.API
   {
       public class UserModels
       {
           public class Profile { }
           public class Settings { }
       }
       
       public class PostModels
       {
           public class Post { }
           public class Comment { }
       }
   }
   ```

3. **Documentation**
   ```csharp
   /// <summary>
   /// Retrieves a user's profile by ID.
   /// </summary>
   /// <param name="userId">The unique identifier of the user</param>
   /// <returns>The user's profile information</returns>
   public async Task<UserProfile> GetUserProfileAsync(
       string userId)
   ```

For more examples and best practices, check the [Examples](Examples.md) guide. 