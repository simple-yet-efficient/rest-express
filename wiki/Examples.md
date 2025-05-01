# Examples

This guide provides practical examples of using REST Express in common scenarios. Each example includes both the Postman setup and the resulting Unity implementation.

## Table of Contents
- [Authentication Flow](#authentication-flow)
- [File Upload](#file-upload)
- [Pagination](#pagination)
- [Real-time Updates](#real-time-updates)
- [Error Handling](#error-handling)
- [Complex Data Types](#complex-data-types)

## Authentication Flow

### Postman Collection Setup
```json
{
  "item": [
    {
      "name": "Auth",
      "item": [
        {
          "name": "Login",
          "request": {
            "method": "POST",
            "url": "{{base_url}}/auth/login",
            "body": {
              "mode": "raw",
              "raw": {
                "email": "user@example.com",
                "password": "password123"
              }
            }
          }
        },
        {
          "name": "Refresh Token",
          "request": {
            "method": "POST",
            "url": "{{base_url}}/auth/refresh",
            "header": {
              "Authorization": "Bearer {{refresh_token}}"
            }
          }
        }
      ]
    }
  ]
}
```

### Generated Code Usage
```csharp
// Login and store tokens
public class GameManager : MonoBehaviour
{
    private ApiClient api;
    private string accessToken;
    private string refreshToken;

    async void Start()
    {
        api = new ApiClient("https://api.example.com");
        
        try
        {
            var response = await api.Auth.LoginAsync(new LoginRequest
            {
                Email = "user@example.com",
                Password = "password123"
            });
            
            accessToken = response.AccessToken;
            refreshToken = response.RefreshToken;
            
            // Setup auth provider
            api.SetAuthProvider(new JwtAuthProvider(accessToken));
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Login failed: {ex.Message}");
        }
    }
}

// Custom auth provider
public class JwtAuthProvider : IAuthProvider
{
    private string token;
    
    public JwtAuthProvider(string token)
    {
        this.token = token;
    }
    
    public void ApplyAuth(UnityWebRequest request)
    {
        request.SetRequestHeader("Authorization", $"Bearer {token}");
    }
}
```

## File Upload

### Postman Collection Setup
```json
{
  "name": "Upload Profile Picture",
  "request": {
    "method": "POST",
    "url": "{{base_url}}/users/profile/picture",
    "body": {
      "mode": "formdata",
      "formdata": [
        {
          "key": "picture",
          "type": "file",
          "src": "/path/to/file"
        },
        {
          "key": "userId",
          "value": "123",
          "type": "text"
        }
      ]
    }
  }
}
```

### Unity Implementation
```csharp
public class ProfileManager : MonoBehaviour
{
    public async Task<bool> UploadProfilePicture(string userId, string imagePath)
    {
        try
        {
            var response = await api.Users.UploadProfilePictureAsync(
                userId,
                imagePath
            );
            
            Debug.Log($"Picture uploaded: {response.PictureUrl}");
            return true;
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Upload failed: {ex.Message}");
            return false;
        }
    }
}

// Usage in UI
public class ProfileUI : MonoBehaviour
{
    public void OnPictureSelected(string path)
    {
        StartCoroutine(UploadPicture(path));
    }
    
    private IEnumerator UploadPicture(string path)
    {
        var loadingUI = ShowLoadingUI();
        
        yield return api.Users.UploadProfilePicture(
            UserManager.CurrentUserId,
            path,
            onSuccess: (response) => {
                UpdateProfileImage(response.PictureUrl);
                loadingUI.Hide();
            },
            onError: (error) => {
                ShowError("Upload failed");
                loadingUI.Hide();
            }
        );
    }
}
```

## Pagination

### Postman Collection Setup
```json
{
  "name": "List Items",
  "request": {
    "method": "GET",
    "url": "{{base_url}}/items",
    "query": [
      {
        "key": "page",
        "value": "1"
      },
      {
        "key": "limit",
        "value": "20"
      }
    ]
  }
}
```

### Unity Implementation
```csharp
public class ItemListManager : MonoBehaviour
{
    private int currentPage = 1;
    private const int ItemsPerPage = 20;
    
    public async Task<List<Item>> LoadNextPage()
    {
        try
        {
            var response = await api.Items.ListItemsAsync(
                page: currentPage,
                limit: ItemsPerPage
            );
            
            currentPage++;
            return response.Items;
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Failed to load items: {ex.Message}");
            return new List<Item>();
        }
    }
}

// UI Implementation
public class InfiniteScrollView : MonoBehaviour
{
    private ItemListManager itemManager;
    
    public void OnScrolledToBottom()
    {
        LoadMoreItems();
    }
    
    private async void LoadMoreItems()
    {
        var items = await itemManager.LoadNextPage();
        foreach (var item in items)
        {
            AddItemToUI(item);
        }
    }
}
```

## Real-time Updates

### Polling Example
```csharp
public class GameStateManager : MonoBehaviour
{
    private float pollInterval = 5f;
    private bool isPolling = false;
    
    public void StartPolling()
    {
        if (!isPolling)
        {
            isPolling = true;
            StartCoroutine(PollGameState());
        }
    }
    
    private IEnumerator PollGameState()
    {
        while (isPolling)
        {
            yield return api.Game.GetStateAsync(
                onSuccess: (state) => UpdateGameState(state),
                onError: (error) => Debug.LogError(error)
            );
            
            yield return new WaitForSeconds(pollInterval);
        }
    }
}
```

## Error Handling

### Global Error Handler
```csharp
public class ApiErrorHandler : MonoBehaviour
{
    public static void HandleError(ApiException ex)
    {
        switch (ex.StatusCode)
        {
            case 401:
                GameManager.Instance.ShowLoginPrompt();
                break;
            case 403:
                UIManager.ShowError("Access denied");
                break;
            case 404:
                UIManager.ShowError("Resource not found");
                break;
            case 429:
                StartCoroutine(RetryAfterDelay(ex.RetryAfter));
                break;
            default:
                UIManager.ShowError("An unexpected error occurred");
                break;
        }
    }
}

// Usage in API calls
try
{
    await api.Users.UpdateProfileAsync(userData);
}
catch (ApiException ex)
{
    ApiErrorHandler.HandleError(ex);
}
```

## Complex Data Types

### Nested Objects
```csharp
// API Response
public class UserProfile
{
    public string Id { get; set; }
    public string Name { get; set; }
    public Address Address { get; set; }
    public List<Achievement> Achievements { get; set; }
}

public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string Country { get; set; }
}

public class Achievement
{
    public string Id { get; set; }
    public string Title { get; set; }
    public DateTime UnlockedAt { get; set; }
}

// Usage
public class ProfileView : MonoBehaviour
{
    public async void LoadProfile(string userId)
    {
        try
        {
            var profile = await api.Users.GetProfileAsync(userId);
            
            // Update UI
            nameText.text = profile.Name;
            addressText.text = $"{profile.Address.Street}, {profile.Address.City}";
            
            foreach (var achievement in profile.Achievements)
            {
                AddAchievementToUI(achievement);
            }
        }
        catch (ApiException ex)
        {
            ApiErrorHandler.HandleError(ex);
        }
    }
}
```

These examples demonstrate common use cases and best practices when using REST Express in a Unity project. For more specific examples or custom scenarios, check our [Best Practices](Best-Practices.md) guide or [contact support](mailto:aqaddora96@gmail.com). 