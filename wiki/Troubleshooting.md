# Troubleshooting

This guide helps you resolve common issues you might encounter while using REST Express.

## Table of Contents
- [Import Issues](#import-issues)
- [Code Generation Issues](#code-generation-issues)
- [Runtime Issues](#runtime-issues)
- [Common Error Messages](#common-error-messages)
- [Unity-Specific Issues](#unity-specific-issues)

## Import Issues

### Collection Won't Import

✖️ **Problem**: Collection file fails to import or shows empty in the tree view.

✔️ **Solutions**:
1. Verify collection format:
   ```bash
   # Check file extension
   collection.json   ✓ Correct
   collection.txt    ✗ Wrong
   ```
2. Validate JSON structure:
   - Use Postman's "Export" feature for correct format
   - Ensure collection is v2.1 format
   - Check for JSON syntax errors

### Missing Variables

✖️ **Problem**: Collection variables not appearing in the editor.

✔️ **Solutions**:
1. Check variable scope:
   - Collection variables should be at root level
   - Environment variables need separate import
2. Export collection with variables:
   ```json
   {
     "variables": [
       {
         "key": "base_url",
         "value": "https://api.example.com"
       }
     ]
   }
   ```

## Code Generation Issues

### Invalid Method Names

✖️ **Problem**: Generated method names are incorrect or invalid.

✔️ **Solutions**:
1. Check request names:
   ```
   ✓ "Get User Profile"  → GetUserProfileAsync
   ✗ "GET /users/{id}"   → GetUsersIdAsync
   ```
2. Follow naming conventions in [Best Practices](Best-Practices.md)

### Type Generation Errors

✖️ **Problem**: Generated types don't match API response.

✔️ **Solutions**:
1. Include example responses in Postman:
   ```json
   {
     "response": [
       {
         "example": {
           "id": 123,
           "name": "Example"
         }
       }
     ]
   }
   ```
2. Use explicit type definitions in descriptions

### Compilation Errors

✖️ **Problem**: Generated code doesn't compile.

✔️ **Solutions**:
1. Check Unity version compatibility
2. Verify all dependencies:
   ```csharp
   using UnityEngine;
   using System.Threading.Tasks;  // Required for async
   using System.Collections;      // Required for coroutines
   ```
3. Update to latest REST Express version

## Runtime Issues

### Authentication Failures

✖️ **Problem**: API calls fail with 401/403 errors.

✔️ **Solutions**:
1. Verify token handling:
   ```csharp
   // Correct
   api.SetAuthProvider(new JwtAuthProvider(token));
   
   // Wrong
   request.SetRequestHeader("Authorization", token);
   ```
2. Check token expiration and refresh flow

### Request Timeouts

✖️ **Problem**: Requests time out or fail to complete.

✔️ **Solutions**:
1. Adjust timeout settings:
   ```csharp
   UnityWebRequest.timeout = 30;  // Seconds
   ```
2. Check network connectivity
3. Verify API endpoint availability

## Common Error Messages

### "Invalid Collection Format"

✖️ **Problem**: `Error: Invalid collection format at line X`

✔️ **Solutions**:
1. Export collection directly from Postman
2. Verify JSON validity
3. Check for unsupported features

### "Type Not Found"

✖️ **Problem**: `CS0246: The type or namespace 'X' could not be found`

✔️ **Solutions**:
1. Add missing using directives:
   ```csharp
   using System.Threading.Tasks;
   using UnityEngine.Networking;
   ```
2. Regenerate code with proper namespace

## Unity-Specific Issues

### Editor vs Runtime

✖️ **Problem**: Code works in editor but fails in build.

✔️ **Solutions**:
1. Use correct compilation directives:
   ```csharp
   #if UNITY_EDITOR
   // Editor-only code
   #endif
   ```
2. Verify platform support for features

### Coroutine Issues

✖️ **Problem**: Coroutines not executing properly.

✔️ **Solutions**:
1. Ensure proper MonoBehaviour usage:
   ```csharp
   // Correct
   StartCoroutine(RequestCoroutine());
   
   // Wrong
   RequestCoroutine();
   ```
2. Check coroutine lifecycle

## Still Having Issues?

If you're still experiencing problems:

1. Check the [FAQ](FAQ.md) for common questions
2. Review [Best Practices](Best-Practices.md)
3. Search [existing issues](https://github.com/simple-yet-efficient/rest-express/issues)
4. Contact [support](mailto:aqaddora96@gmail.com) with:
   - REST Express version
   - Unity version
   - Collection sample
   - Error messages
   - Steps to reproduce 