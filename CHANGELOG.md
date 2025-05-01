# Changelog

All notable changes to REST Express for Unity will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2024-03-19

### Added
- Initial release of REST Express for Unity
- API Client Manager window with collection management
  - Import Postman collections from JSON files
  - Tree view for collection organization
  - Request editing and testing
  - Support for headers, query parameters, and request bodies
  - File upload support
  - Variable substitution
  - Real-time request testing
- Script Generator window
  - Generate C# API clients from collections
  - Support for Async and Coroutine methods
  - Customizable script names and types
  - Type inference and generation
  - Template customization
- Modern and intuitive UI
  - Responsive layout
  - Dark theme support
  - Clear visual hierarchy
  - Context menus for common actions
- Comprehensive documentation
  - Installation guide
  - Usage instructions
  - API reference
  - Code generation guide
  - Best practices
  - Troubleshooting guide

### Technical Details
- Minimum Unity version: 2020.3 LTS
- .NET 4.x compatibility
- Built-in error handling and logging
- Support for Postman Collection v2.1 format
- Automatic variable substitution
- Form data and multipart request support
- Custom authentication handlers 