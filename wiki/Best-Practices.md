# Best Practices

This guide outlines the best practices for using REST Express effectively, with special emphasis on collection organization and variable usage for optimal code generation.

## Table of Contents
- [Collection Organization](#collection-organization)
- [Variable Management](#variable-management)
- [Request Naming](#request-naming)
- [Testing Strategy](#testing-strategy)
- [Code Generation Tips](#code-generation-tips)

## Collection Organization

### Folder Structure
Organize your Postman collection with a clear hierarchy:
```
MyAPI/
├── Auth/
│   ├── Login
│   ├── Register
│   └── Refresh Token
├── Users/
│   ├── Get User
│   ├── Update User
│   └── Delete User
└── Content/
    ├── List Items
    └── Search Items
```

**Why This Matters**: 
- Generated code mirrors this structure
- Cleaner class organization
- Better code navigation
- Logical API grouping

### Request Grouping
Group related requests together:
- Keep CRUD operations for the same resource together
- Place authentication-related endpoints in one folder
- Organize by feature or domain

**Impact on Generated Code**:
```csharp
// Well-organized collection generates:
public class ApiClient
{
    public AuthClient Auth { get; }
    public UsersClient Users { get; }
    public ContentClient Content { get; }
}

// Versus unorganized:
public class ApiClient
{
    public Task<LoginResponse> LoginAsync() { }
    public Task<UserData> GetUserAsync() { }
    // ... mixed functionality
}
```

## Variable Management

### Base URL Variables
```json
{
  "variables": {
    "base_url": "https://api.example.com/v1",
    "api_version": "v1"
  }
}
```

**Benefits**:
- Easy environment switching
- Consistent URL handling
- Clean generated code

### Authentication Variables
```json
{
  "variables": {
    "auth_token": "{{token}}",
    "api_key": "{{key}}"
  }
}
```

**Generated Code Impact**:
```csharp
public class ApiClient
{
    private readonly string baseUrl;
    private readonly string apiVersion;
    private readonly IAuthProvider authProvider;

    // Clean constructor with defaults
    public ApiClient(
        string baseUrl = "https://api.example.com/v1",
        string apiVersion = "v1",
        IAuthProvider authProvider = null)
    {
        // ...
    }
}
```

### Environment-Specific Variables
- Development variables
- Staging variables
- Production variables

**Best Practice**: Use environment variables for:
- API keys
- Endpoints
- Test accounts
- Feature flags

## Request Naming

### Naming Conventions
✅ Good Names:
- `Get User Profile`
- `Create New Post`
- `Update User Settings`

❌ Bad Names:
- `GET /api/v1/users/{id}`
- `POST endpoint`
- `do thing`

**Why**: Clean names generate clean method names:
```csharp
// Good names generate:
await api.Users.GetUserProfileAsync(userId);
await api.Posts.CreateNewPostAsync(post);

// Bad names generate:
await api.GetApiV1UsersAsync(id);
await api.PostEndpointAsync();
```

### Request Documentation
Add descriptions to your requests:
```json
{
  "name": "Get User Profile",
  "description": "Retrieves a user's full profile data",
  "parameters": [
    {
      "name": "userId",
      "description": "The unique identifier of the user"
    }
  ]
}
```

**Result**: Well-documented generated code:
```csharp
/// <summary>
/// Retrieves a user's full profile data
/// </summary>
/// <param name="userId">The unique identifier of the user</param>
public async Task<UserProfile> GetUserProfileAsync(string userId)
```

## Testing Strategy

### Request Examples
Include example responses:
```json
{
  "response": {
    "example": {
      "id": "123",
      "name": "John Doe"
    }
  }
}
```

**Benefit**: Better type generation and validation

### Test Data
- Use realistic test data
- Cover edge cases
- Include error scenarios

## Code Generation Tips

### Method Type Selection
Choose based on your needs:
- **Async**: Modern C# development
- **Coroutine**: Unity-specific workflows

### Response Types
Define clear response structures:
```json
{
  "data": {
    "id": "string",
    "created": "datetime"
  }
}
```

**Generated Code**:
```csharp
public class ResponseData
{
    public string Id { get; set; }
    public DateTime Created { get; set; }
}
```

### Error Handling
Include error response examples:
```json
{
  "error": {
    "code": 400,
    "message": "Invalid input"
  }
}
```

**Result**: Proper error handling generation:
```csharp
try
{
    await api.Users.CreateAsync(userData);
}
catch (ApiException ex) when (ex.StatusCode == 400)
{
    // Handle validation error
}
```

## Summary

Following these best practices ensures:
1. Clean, maintainable generated code
2. Consistent API integration
3. Better developer experience
4. Reduced maintenance overhead
5. Faster integration time

Remember: The quality of your generated code directly reflects the organization and documentation of your Postman collection. 