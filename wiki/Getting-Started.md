# Getting Started with REST Express

Welcome to REST Express! This guide will help you get up and running with REST Express in your Unity project.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [First Steps](#first-steps)
- [Next Steps](#next-steps)

## Prerequisites

Before you begin, ensure you have:
- Unity 2020.3 LTS or later
- A Postman collection (v2.1 format)
- Basic understanding of REST APIs
- .NET 4.x scripting runtime

## Installation

1. Download REST Express from the [Unity Asset Store](https://assetstore.unity.com/packages/slug/319060)
2. Import the package into your Unity project via:
   - Assets → Import Package → Custom Package...
   - Select the downloaded .unitypackage file
3. The REST Express tools will appear under the "SyE" menu in Unity

## Quick Start

1. **Import Your Collection**
   - Open the REST Express window (SyE → REST Express)
   - Click "Import Collection"
   - Select your Postman collection JSON file

2. **Test Your APIs**
   - Select any request from the collection tree
   - Fill in any required parameters
   - Click "Send Request" to test
   - View the response in real-time

3. **Generate Code**
   - Open the Script Generator (SyE → Script Generator)
   - Select your collection
   - Choose Async or Coroutine mode
   - Click "Generate" to create your API client

## First Steps

### 1. Understanding the Interface

The REST Express window has two main components:
- **API Importer**: Test and manage your API requests
- **Script Generator**: Generate C# client code

### 2. Working with Collections

Your Postman collection will appear in a tree view:
- Folders organize related requests
- Click any request to view/edit details
- Right-click for context actions
- Use the toolbar for common actions

### 3. Testing Requests

Each request can be tested with:
- Custom headers
- Query parameters
- Request body (JSON/Form data)
- File uploads
- Variable substitution

### 4. Generating Code

The Script Generator offers:
- Choice between Async and Coroutine methods
- Customizable class names
- Clean, documented code
- Proper error handling
- Type-safe requests/responses

## Next Steps

Now that you're set up, explore these topics:
- [API Reference](API-Reference.md) for detailed functionality
- [Code Generation Guide](Code-Generation.md) for customization options
- [Best Practices](Best-Practices.md) for optimal usage
- [Examples](Examples.md) for common scenarios
- [Troubleshooting](Troubleshooting.md) for common issues

Need help? Check our [Troubleshooting](Troubleshooting.md) guide or [contact support](mailto:aqaddora96@gmail.com). 