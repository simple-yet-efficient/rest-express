# API Reference

This reference guide provides detailed information about REST Express's features, components, and generated code.

## Table of Contents
- [API Importer Window](#api-importer-window)
- [Script Generator Window](#script-generator-window)
- [Generated Code Structure](#generated-code-structure)
- [Request Types](#request-types)
- [Response Handling](#response-handling)
- [Error Handling](#error-handling)

## API Importer Window

### Collection Management
- **Import Collection**: Imports Postman collections (.json)
- **Collection Tree**: Hierarchical view of requests
- **Context Menu**: Right-click actions for requests/folders
- **Search**: Filter requests by name/URL

### Request Testing
- **URL Field**: Edit request URL
- **Method Selector**: Choose HTTP method
- **Headers**: Add/edit request headers
- **Query Parameters**: URL parameters
- **Request Body**: JSON/Form data input
- **File Upload**: Handle file attachments
- **Send Button**: Execute request
- **Response View**: View formatted response

### Variable Support
- **Collection Variables**: From Postman collection
- **Environment Variables**: Local overrides
- **Variable Substitution**: {{variable}} syntax
- **Variable Management**: Add/edit/delete

## Script Generator Window

### Configuration Options
- **Script Name**: Output class name
- **Method Type**: Async/Coroutine
- **Collection**: Source collection
- **Output Path**: Generated file location

### Generated Features
- **Method Types**: GET, POST, PUT, DELETE, etc.
- **Parameter Handling**: Query, body, headers
- **Response Types**: JSON deserialization
- **Error Handling**: Try-catch blocks
- **Cancellation**: Token support
- **Documentation**: XML comments

## Generated Code Structure

### Base Classes
```csharp
public class ApiClient
{
    private readonly string baseUrl;
    private readonly IAuthProvider authProvider;
    
    public ApiClient(string baseUrl, IAuthProvider authProvider = null)
    {
        this.baseUrl = baseUrl;
        this.authProvider = authProvider;
    }
    
    // ... Common functionality
}
```

### Request Methods
```csharp
// Async Method
public async Task<TResponse> GetDataAsync<TResponse>(
    string endpoint,
    Dictionary<string, string> queryParams = null,
    CancellationToken cancellationToken = default)
{
    // Implementation
}

// Coroutine Method
public IEnumerator GetData<TResponse>(
    string endpoint,
    Action<TResponse> onSuccess,
    Action<string> onError,
    Dictionary<string, string> queryParams = null)
{
    // Implementation
}
```

## Request Types

### GET Request
```csharp
public async Task<UserData> GetUserAsync(string userId)
{
    return await SendRequestAsync<UserData>($"users/{userId}");
}
```

### POST Request
```csharp
public async Task<LoginResponse> LoginAsync(LoginRequest request)
{
    return await SendRequestAsync<LoginResponse>("auth/login", HttpMethod.Post, request);
}
```

### File Upload
```csharp
public async Task<FileResponse> UploadFileAsync(string filePath)
{
    var form = new WWWForm();
    form.AddBinaryData("file", File.ReadAllBytes(filePath));
    return await SendFormRequestAsync<FileResponse>("upload", form);
}
```

## Response Handling

### Success Response
```csharp
public class ApiResponse<T>
{
    public T Data { get; set; }
    public bool Success { get; set; }
    public string Message { get; set; }
}
```

### Error Response
```csharp
public class ApiException : Exception
{
    public int StatusCode { get; }
    public string ResponseBody { get; }
    
    public ApiException(int statusCode, string message, string body)
        : base(message)
    {
        StatusCode = statusCode;
        ResponseBody = body;
    }
}
```

## Error Handling

### Try-Catch Pattern
```csharp
try
{
    var response = await api.GetUserAsync("123");
    // Handle success
}
catch (ApiException ex) when (ex.StatusCode == 404)
{
    // Handle not found
}
catch (ApiException ex)
{
    // Handle other API errors
}
catch (Exception ex)
{
    // Handle unexpected errors
}
```

### Coroutine Error Handling
```csharp
api.GetUser("123",
    onSuccess: (user) => { /* Handle success */ },
    onError: (error) => { /* Handle error */ }
);
```

For more detailed examples, check the [Examples](Examples.md) page. 