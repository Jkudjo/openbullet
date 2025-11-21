# OpenBullet 2 Upgrade Plan

## Overview

This document outlines the plan to upgrade OpenBullet 1 to OpenBullet 2 architecture with modern .NET and enhanced features.

## Key Improvements

### 1. Modern .NET Platform
- **Current**: .NET Framework 4.7.2 (Windows-only)
- **Target**: .NET 8 (Cross-platform: Windows, Linux, macOS)
- **Benefits**: 
  - Cross-platform support
  - Better performance
  - Modern async/await patterns
  - Smaller deployment size

### 2. Architecture Modernization
- **Current**: WPF desktop app with synchronous operations
- **Target**: 
  - Web-based UI (Blazor Server/WebAssembly)
  - REST API backend
  - Async/await throughout
  - Microservices-ready architecture

### 3. Enhanced Features
- **Guests System**: Multi-user support with permissions
- **Remote Configs**: Cloud-based config management
- **Better Plugin System**: More extensible plugin architecture
- **Advanced Logging**: Structured logging with Serilog
- **Docker Support**: Containerized deployment
- **REST API**: Programmatic access

### 4. Performance Improvements
- **Current**: Thread-based multithreading
- **Target**: 
  - Async/await for I/O operations
  - Better resource management
  - Improved proxy rotation
  - Optimized memory usage

## Migration Strategy

### Phase 1: Foundation (Current)
- Create modern project structure
- Upgrade to .NET 8
- Modernize RuriLib core

### Phase 2: Backend
- Implement REST API
- Add async operations
- Improve runner performance

### Phase 3: Frontend
- Build web UI (Blazor)
- Migrate existing features
- Add new UI features

### Phase 4: Advanced Features
- Guests system
- Remote configs
- Enhanced plugins
- Docker support

## Project Structure

```
openbullet/
├── src/
│   ├── OpenBullet.Core/          # Core library (modernized RuriLib)
│   ├── OpenBullet.API/           # REST API backend
│   ├── OpenBullet.Web/           # Blazor web UI
│   ├── OpenBullet.CLI/           # Modern CLI
│   └── OpenBullet.Plugin/        # Plugin framework
├── tests/
│   ├── OpenBullet.Core.Tests/
│   └── OpenBullet.API.Tests/
├── docker/
│   ├── Dockerfile
│   └── docker-compose.yml
└── docs/
    └── API/
```

## Technology Stack

- **.NET 8**: Runtime and SDK
- **Blazor Server**: Web UI
- **ASP.NET Core**: REST API
- **Entity Framework Core**: Database (if needed)
- **Serilog**: Logging
- **SignalR**: Real-time updates
- **Docker**: Containerization
- **xUnit**: Testing

## Backward Compatibility

- Import `.loli` configs (convert to `.opk`)
- Support existing wordlist formats
- Maintain plugin compatibility where possible
- Migration tool for existing data

## Implementation Steps

1. ✅ Create upgrade plan
2. ⏳ Upgrade project files to SDK-style
3. ⏳ Modernize RuriLib with async
4. ⏳ Create REST API
5. ⏳ Build Blazor UI
6. ⏳ Add new features
7. ⏳ Testing and migration tools

