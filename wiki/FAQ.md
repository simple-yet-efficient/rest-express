# Frequently Asked Questions (FAQ)

## General Questions

### Q: What is REST Express?
**A:** REST Express is a Unity editor extension that converts Postman collections into Unity-ready API clients. It allows you to test API endpoints directly in the Unity Editor and generate clean, maintainable C# code.

### Q: What Unity versions are supported?
**A:** REST Express supports Unity 2020.3 LTS and later versions. It's regularly tested with the latest LTS releases.

### Q: Is it free to use?
**A:** REST Express is available on the Unity Asset Store. Check our [store page](https://assetstore.unity.com/packages/slug/319060) for pricing details.

## Features

### Q: Can I use both async and coroutine methods?
**A:** Yes! REST Express generates both async and coroutine versions of your API methods. You can choose which style to use based on your needs.

### Q: Does it support file uploads?
**A:** Yes, REST Express fully supports file uploads through multipart/form-data requests. See our [Examples](Examples.md#file-upload) for implementation details.

### Q: Can I customize the generated code?
**A:** Absolutely! REST Express offers extensive customization options through:
- Custom templates
- Naming conventions
- Type generation rules
- Base classes
See our [Code Generation Guide](Code-Generation.md) for details.

## Technical Questions

### Q: How does authentication work?
**A:** REST Express supports various authentication methods:
- Bearer tokens
- API keys
- Custom headers
- OAuth flows
Implement the `IAuthProvider` interface for custom authentication.

### Q: Can I use it with WebGL builds?
**A:** Yes, REST Express works with WebGL builds. However, consider:
- CORS policies
- SSL/HTTPS requirements
- Browser security restrictions

### Q: Does it handle HTTPS/SSL certificates?
**A:** Yes, REST Express handles HTTPS connections and certificates. For development environments with self-signed certificates, use the provided certificate handler.

## Usage Questions

### Q: How do I organize large collections?
**A:** Best practices include:
1. Group related endpoints in folders
2. Use consistent naming conventions
3. Document request/response examples
4. Utilize variables for common values
See [Best Practices](Best-Practices.md) for detailed guidance.

### Q: Can I use environment variables?
**A:** Yes, REST Express supports Postman environment variables. They can be:
- Imported with collections
- Overridden locally
- Used in generated code

### Q: How do I handle errors?
**A:** REST Express provides several error handling options:
1. Try-catch blocks with ApiException
2. Error callbacks in coroutines
3. Global error handlers
See [Error Handling](Examples.md#error-handling) examples.

## Troubleshooting

### Q: Why isn't my collection importing?
**A:** Common reasons include:
1. Invalid JSON format
2. Unsupported Postman version
3. Missing required fields
See [Troubleshooting](Troubleshooting.md#import-issues) for solutions.

### Q: Why doesn't my generated code compile?
**A:** Check these common issues:
1. Missing dependencies
2. Incorrect Unity version
3. Invalid type references
See [Compilation Errors](Troubleshooting.md#compilation-errors).

### Q: How do I report bugs?
**A:** To report bugs:
1. Check [existing issues](https://github.com/simple-yet-efficient/rest-express/issues)
2. Provide reproduction steps
3. Include error messages
4. Share collection sample (if possible)

## Updates and Support

### Q: How often is it updated?
**A:** We regularly update REST Express with:
- Bug fixes
- New features
- Unity compatibility updates
- Performance improvements

### Q: Where can I get help?
**A:** Support options include:
1. [Documentation](https://github.com/simple-yet-efficient/rest-express/wiki)
2. [Issue tracker](https://github.com/simple-yet-efficient/rest-express/issues)
3. Email support: [aqaddora96@gmail.com](mailto:aqaddora96@gmail.com)
4. Unity Asset Store comments

### Q: Can I request features?
**A:** Yes! We welcome feature requests through:
1. GitHub issues
2. Email
3. Asset Store reviews

## Best Practices

### Q: What's the best way to structure my API code?
**A:** We recommend:
1. Group related endpoints
2. Use meaningful names
3. Document responses
4. Handle errors consistently
See [Best Practices](Best-Practices.md) for more.

### Q: Should I use async or coroutines?
**A:** Choose based on your needs:
- **Async**: Modern C# development, better error handling
- **Coroutines**: Unity-specific workflows, simpler integration

### Q: How do I maintain generated code?
**A:** Best practices include:
1. Keep collections updated
2. Use version control
3. Document customizations
4. Follow naming conventions 