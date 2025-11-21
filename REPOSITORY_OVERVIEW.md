# OpenBullet Repository Overview

## Summary

This repository contains **OpenBullet 1**, a web testing automation suite written in C# (.NET Framework 4.7.2). It provides both a GUI application and CLI tool for automating web requests, testing credentials, scraping data, and performing browser automation.

**⚠️ Status**: OpenBullet 1 has reached end-of-life. Consider migrating to [OpenBullet 2](https://github.com/openbullet/OpenBullet2).

## Repository Contents

### Documentation Files

- **[README.md](README.md)**: Original project README
- **[USAGE_GUIDE.md](USAGE_GUIDE.md)**: Comprehensive usage guide
- **[QUICK_START.md](QUICK_START.md)**: Quick start tutorial
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: Technical architecture overview
- **[REPOSITORY_OVERVIEW.md](REPOSITORY_OVERVIEW.md)**: This file

### Source Code

#### 1. OpenBullet (GUI Application)
- **Location**: `OpenBullet/`
- **Type**: WPF (Windows Presentation Foundation) application
- **Purpose**: Full-featured GUI for creating configs, managing wordlists, running tests
- **Entry Point**: `MainWindow.xaml.cs`
- **Key Features**:
  - Visual config editor (Stacker)
  - Runner manager with real-time stats
  - Wordlist manager
  - Proxy manager
  - Hits database viewer
  - Settings management

#### 2. OpenBulletCLI (Command-Line Interface)
- **Location**: `OpenBulletCLI/`
- **Type**: Console application
- **Purpose**: Headless execution for automation/scripts
- **Entry Point**: `Program.cs`
- **Key Features**:
  - Command-line argument parsing
  - Config execution
  - Progress tracking via console
  - Minimal dependencies

#### 3. RuriLib (Core Library)
- **Location**: `RuriLib/`
- **Type**: Class library (.dll)
- **Purpose**: Core automation engine
- **Key Components**:
  - **Blocks/**: Block implementations (Request, Parse, Keycheck, etc.)
  - **Runner/**: Execution engine (multi-threaded)
  - **LoliScript/**: Script parser and interpreter
  - **Models/**: Data models (Config, BotData, etc.)
  - **Functions/**: Utility functions
  - **IOManager.cs**: File I/O operations

#### 4. PluginFramework
- **Location**: `PluginFramework/`
- **Type**: Class library
- **Purpose**: Plugin system for extending functionality
- **Usage**: Create custom blocks via plugins

### Configuration Files

- **OpenBullet.sln**: Visual Studio solution file
- **appveyor.yml**: CI/CD configuration
- **LICENSE**: MIT License
- **DefaultEnvFile.ini**: Default environment settings template

### External Dependencies

- **Extreme.Net.dll**: HTTP client library
- **CloudflareSolverRe.dll**: Cloudflare bypass library

## How to Use This Repository

### For Developers

1. **Build the Project**:
   ```bash
   # Open in Visual Studio
   # Or use MSBuild:
   msbuild OpenBullet.sln /p:Configuration=Release
   ```

2. **Understand the Architecture**:
   - Read [ARCHITECTURE.md](ARCHITECTURE.md) for technical details
   - Explore `RuriLib/` for core functionality
   - Check `OpenBullet/Views/` for GUI implementation

3. **Extend Functionality**:
   - Create custom blocks in `RuriLib/Blocks/`
   - Develop plugins using `PluginFramework/`
   - Add new functions in `RuriLib/Functions/`

### For Users

1. **Get Started**:
   - Read [QUICK_START.md](QUICK_START.md) for basic usage
   - Follow [USAGE_GUIDE.md](USAGE_GUIDE.md) for detailed instructions

2. **Create Configs**:
   - Use GUI Stacker editor
   - Or write `.loli` files manually
   - See examples in documentation

3. **Run Tests**:
   - GUI: Load config → Load wordlist → Start
   - CLI: Use command-line arguments

## Project Structure Deep Dive

### Config System

Configs are stored in `.loli` format (LoliScript):

```
[SETTINGS]
{
  "SuggestedBots": 10,
  "MaxCPM": 60,
  ...
}

[SCRIPT]
BLOCK:Request
  URL=https://example.com
ENDBLOCK
...
```

### Block System

Blocks are the building blocks of automation:
- **Request**: HTTP requests
- **Parse**: Extract data
- **Keycheck**: Validate responses
- **Function**: Execute functions
- **Selenium**: Browser automation
- And more...

### Execution Flow

1. Config loaded → Parsed into blocks
2. Wordlist loaded → Distributed to bots
3. Bots execute blocks sequentially
4. Results collected (Hits/Fails/Custom/ToCheck)
5. Statistics updated in real-time

## Key Concepts

### Bots
- Concurrent workers that execute configs
- Each bot processes one wordlist line at a time
- Multiple bots run in parallel threads

### Wordlists
- Text files with data to test
- Types: Default, Email, Credentials, etc.
- Format defined by regex patterns

### Proxies
- HTTP/SOCKS proxies for request routing
- Rotation and health checking
- Support for authentication

### Hits
- Successful results from tests
- Stored in LiteDB database
- Exportable in various formats

## Common Use Cases

1. **Credential Testing**: Test username/password combinations
2. **Data Scraping**: Extract data from websites
3. **API Testing**: Test API endpoints
4. **Browser Automation**: Complex web interactions via Selenium
5. **Penetration Testing**: Automated security testing (with permission)

## Development Workflow

### Adding a New Block

1. Create class in `RuriLib/Blocks/`
2. Inherit from `BlockBase`
3. Implement required methods:
   - `FromLS()`: Parse from LoliScript
   - `ToLS()`: Convert to LoliScript
   - `Process()`: Execute block logic
4. Register in block parser

### Adding a New Function

1. Add to `RuriLib/Functions/`
2. Implement function logic
3. Register in function registry
4. Use in Function blocks

### Modifying GUI

1. Edit XAML files in `OpenBullet/Views/`
2. Update ViewModels in `OpenBullet/ViewModels/`
3. Follow MVVM pattern

## Testing

### Manual Testing
- Create test configs
- Use small wordlists
- Verify block execution
- Check results

### Debugging
- Use verbose mode in CLI
- Check log files
- Use Visual Studio debugger
- Enable detailed logging

## Building and Deployment

### Requirements
- .NET Framework 4.7.2 Developer Pack
- Visual Studio 2017+ (or MSBuild)
- NuGet package manager

### Build Output
- `OpenBullet/bin/Release/`: GUI executable
- `OpenBulletCLI/bin/Release/`: CLI executable
- Dependencies copied to `bin/` subdirectory

### Distribution
- Copy entire Release folder
- Include all DLLs in `bin/` subdirectory
- Include `Settings/` folder with `Environment.ini`

## Migration to OpenBullet 2

If migrating to OpenBullet 2:
1. Export configs from OB1
2. Convert configs to OB2 format
3. Migrate wordlists (usually compatible)
4. Update proxy formats if needed
5. Test thoroughly

## Resources

- **Official Docs**: https://openbullet.github.io/ob1
- **Forum**: https://discourse.openbullet.dev/
- **OpenBullet 2**: https://github.com/openbullet/OpenBullet2
- **Sample Plugin**: https://github.com/openbullet/openbullet-plugin

## Legal Notice

**⚠️ IMPORTANT**: This software is for legitimate testing purposes only. Using it for unauthorized access, credential stuffing, or DDoS attacks is illegal. Only use on:
- Sites you own
- Sites with explicit written permission
- Your own applications for testing

## Contributing

While OpenBullet 1 is end-of-life, contributions may still be useful:
1. Fork the repository
2. Create feature branch
3. Make changes
4. Submit pull request

However, consider contributing to OpenBullet 2 instead for active development.

## Support

- **GitHub Issues**: For bug reports
- **Forum**: For questions and community help
- **Email**: ruri [at] openbullet [dot] dev (not checked frequently)

## License

MIT License - See [LICENSE](LICENSE) file

## Credits

Developed by the OpenBullet community. Special thanks to:
- **demiurgo** and **meinname** for beta testing
- All community contributors

---

**Last Updated**: Based on repository exploration
**Version**: OpenBullet 1 (End of Life)
**Recommendation**: Migrate to OpenBullet 2 for continued support

